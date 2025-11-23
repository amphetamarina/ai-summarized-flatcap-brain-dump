# Architecture & System Design

This document describes the core architectural foundations of NeoMutt, covering the overall system design, data hierarchies, event systems, and window management. Understanding these concepts is essential for any significant development work on NeoMutt.

## Source Files
- `brain-dump/mvc.txt` - Model-View-Controller architecture
- `brain-dump/account.txt` - Account management system (1300+ lines of detailed notes)
- `brain-dump/config.txt` - Configuration system design
- `brain-dump/notifications.txt` - Event and notification architecture
- `brain-dump/windows.txt` - Window management system

---

## Overview: The NeoMutt Architecture

NeoMutt follows a layered architecture that separates data management from presentation. At its heart is a hierarchy of objects (NeoMutt → Account → Mailbox → Email) with an event-driven notification system that keeps all components synchronized. The UI is built on a nested window system that provides flexible layouts and clean separation of concerns.

### Key Design Principles

1. **Separation of Data and View**: Data objects (Account, Mailbox, Email) are distinct from their visual representations (Views)
2. **Observer Pattern**: Components communicate through notifications rather than direct coupling
3. **Hierarchical Inheritance**: Configuration and behavior flow down through the object hierarchy
4. **State Machine Navigation**: Dialog and screen transitions are managed through a state stack

---

## The Object Hierarchy

NeoMutt organizes email data in a clear hierarchy that mirrors how users think about their mail:

```
NeoMutt (N)
├── AccountList
│   └── Account (A)
│       ├── MailboxList
│       │   └── Mailbox (M)
│       │       └── EmailList
│       │           └── Email (E)
│       │               ├── Envelope
│       │               └── Body
│       └── Account-specific config
├── Global Config
├── Window Root
├── History
├── Aliases
└── Hooks
```

### NeoMutt Object (N)

The root object that owns everything. It maintains:

- **AccountList**: All configured email accounts
- **Config**: Global configuration settings
- **WindowRoot**: The window hierarchy
- **History**: Command and search history
- **Aliases**: Address book entries
- **Hooks**: Event triggers (folder-hook, account-hook, etc.)

### Account Object (A)

Represents a single email account, which varies by backend type:

| Backend | Account Represents |
|---------|-------------------|
| IMAP | One server/port/user combination |
| POP | One server/port/user combination |
| NNTP | One news server with subscribed groups |
| Notmuch | One notmuch database |
| Maildir | A configurable set of local folders |

Each Account contains:
- A list of Mailboxes
- Backend-specific connection data (`adata`)
- Account-specific configuration (inheriting from global)
- Credentials and authentication state

### Mailbox Object (M)

Represents a single folder or mailbox containing emails:

- **Name and description** for display
- **Email list** with the actual messages
- **Path information**: user-specified path and canonical path
- **State**: counts (new, unread, total), last update time
- **Backend-specific data** (`mdata`)

### Email Object (E)

The individual message with its components:

- **Envelope**: Headers (From, To, Subject, Date, etc.)
- **Body**: Message content and attachments
- **Backend-specific data** (`edata`): UIDs, flags, etc.

---

## The View System

Each data object can have one or more Views that provide a UI-specific perspective:

```
Data Object          View Object
-----------          -----------
Account (A)    →     AccountView (AV)
Mailbox (M)    →     MailboxView (MV)
Email (E)      →     EmailView (EV)
```

### Why Separate Views?

Views contain information that's specific to how the data is displayed:

- **MailboxView (MV)**: Sorting order, threading state, limit/filter patterns, current selection
- **EmailView (EV)**: Tag state, deleted flag, cached display text, color information
- **AccountView (AV)**: Which accounts are visible, sidebar position, expand/collapse state

This separation allows:
1. Multiple views of the same data (e.g., two Index windows)
2. Read-only views (e.g., attaching a message from another mailbox)
3. Clean ownership semantics (data persists, views come and go)

### View Ownership

- **Index** holds MailboxView and EmailView for the current folder
- **Pager** holds an EmailView for the displayed message
- **Sidebar** holds AccountViews for all visible accounts
- **Status bar** references the Index's views for formatting

---

## The Main Event Loop

NeoMutt's main loop is the heart of the application:

```
┌─────────────────────────────────────────┐
│              Main Loop                   │
├─────────────────────────────────────────┤
│  1. Read keyboard input                 │
│  2. Process key → find binding          │
│  3. Execute function                    │
│  4. Check timers (timeout-hook)         │
│  5. Check signals                       │
│  6. Process notifications               │
│  7. Repaint dirty windows               │
│  8. Repeat                              │
└─────────────────────────────────────────┘
```

### Event Types

The system handles several types of events:

| Event Type | Source | Examples |
|------------|--------|----------|
| Keyboard | User input | Key presses, escape sequences |
| Mouse | User input | Clicks, scrolls, drags |
| Timer | System | Timeout hooks, keepalive |
| Filesystem | System | inotify for new mail |
| Network | Backend | IMAP IDLE notifications |
| Signal | System | SIGWINCH (resize), SIGINT |

### Key Binding Resolution

When a key is pressed, NeoMutt searches for a matching binding:

1. Check the focused window's local bindings
2. Check parent windows up the hierarchy
3. Check global bindings
4. Handle multi-key sequences (e.g., "gg" for go-to-top)

---

## The Notification System

NeoMutt uses an observer pattern for loose coupling between components. Objects send notifications when they change, and interested parties subscribe to receive updates.

### Notification Flow

```
Data Change → Notify Object → Observer List → Callbacks
```

### Event Categories

**NeoMutt Events**:
- `NT_NEOMUTT_ACCOUNT_ADD`: New account created
- `NT_NEOMUTT_ACCOUNT_DELETE`: Account removed
- `NT_NEOMUTT_STARTUP`: Application started
- `NT_NEOMUTT_SHUTDOWN`: Application closing

**Account Events**:
- `NT_ACCOUNT_MAILBOX_ADD`: Mailbox added to account
- `NT_ACCOUNT_MAILBOX_DELETE`: Mailbox removed
- `NT_ACCOUNT_OPEN`: Connection established
- `NT_ACCOUNT_CLOSE`: Connection closed

**Mailbox Events**:
- `NT_MAILBOX_EMAIL_ADD`: New email(s) arrived
- `NT_MAILBOX_EMAIL_DELETE`: Email(s) removed
- `NT_MAILBOX_CHANGE`: Mailbox state changed
- `NT_MAILBOX_RESORT`: Sort order changed

**Config Events**:
- `NT_CONFIG_SET`: Configuration value changed
- `NT_CONFIG_RESET`: Value reset to default
- `NT_CONFIG_INITIAL_SET`: Initial value set during startup

### Subscribing to Notifications

Components register their interest:

```c
// Sidebar wants to know about all account/mailbox changes
notify_observer_add(NeoMutt->notify, NT_ACCOUNT, sidebar_observer, sidebar_data);

// Index wants to know about its specific mailbox
notify_observer_add(Mailbox->notify, NT_MAILBOX, index_observer, index_data);
```

### Notification Propagation

Notifications propagate upward through the hierarchy:

```
Email change → Mailbox notified → Account notified → NeoMutt notified
```

This allows high-level observers (like the sidebar) to watch for any changes without subscribing to every individual object.

---

## The Window System

NeoMutt uses a hierarchical window system inspired by modern GUI toolkits. Windows are nested containers that automatically handle layout, resizing, and focus management.

### Window Hierarchy

```
RootWindow (Vertical Container)
├── HelpLine
├── Horizontal Container
│   ├── Sidebar
│   └── Vertical Container
│       ├── Index
│       ├── IndexBar (status)
│       ├── Pager
│       └── PagerBar (status)
└── MessageWindow (command line)
```

### Container Types

- **Vertical (V)**: Children stacked top-to-bottom
- **Horizontal (H)**: Children placed left-to-right

### Window Attributes

Each window has attributes that control its layout behavior:

| Attribute | Description |
|-----------|-------------|
| `MIN_SIZE` | Minimum dimensions |
| `MAX_SIZE` | Maximum dimensions |
| `FIXED` | Exact size, doesn't grow |
| `EXPAND` | Grows to fill available space |
| `VISIBLE` | Whether window is shown |
| `FOCUSABLE` | Can receive keyboard focus |

### The Reflow Algorithm

When the terminal resizes or a window's visibility changes, the reflow algorithm redistributes space:

1. **Invisible windows**: Skip entirely
2. **Fixed-size windows**: Allocate their requested space
3. **Minimum-size windows**: Allocate minimum, expand if space remains
4. **Maximum-size windows**: Share remaining space equally

### Focus Management

Focus determines which window receives keyboard input:

- `<Tab>` / `<Shift-Tab>`: Move focus to next/previous window
- Direct commands: `<focus-sidebar>`, `<focus-index>`, etc.
- Mouse clicks: Focus follows click (when enabled)

The focus system maintains a stack for returning focus after dialogs close.

---

## Configuration Architecture

NeoMutt's configuration system supports inheritance through scoping:

### Scope Levels

```
NeoMutt (Global)
    └── Account Scope
            └── Mailbox Scope
```

### Setting Scoped Config

```
set sidebar_visible = yes              # Global setting
set work:sidebar_visible = no          # Account "work" only
set work:inbox:sidebar_visible = yes   # Specific mailbox
```

### Config Lookup

When code requests a config value, the system searches:

1. Mailbox-specific setting (if in mailbox context)
2. Account-specific setting (if in account context)
3. Global setting
4. Default value

### Config Types

| Type | Description | Example |
|------|-------------|---------|
| Bool | True/false | `sidebar_visible` |
| Number | Integer value | `timeout` |
| String | Text value | `strstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstrstr`sidebar_format` |
| Path | File/directory path | `folder` |
| Quad | Yes/No/Ask-Yes/Ask-No | `quit` |
| Enum | Named options | `sort` |
| Regex | Regular expression | `reply_regex` |
| Address | Email address | `from` |
| Slist | List of strings | `mailcap_path` |

---

## State Management

NeoMutt uses a state machine for navigation between screens:

### State Stack

```
┌─────────┐
│ Compose │  ← Current state (top of stack)
├─────────┤
│ Index   │
├─────────┤
│ Init    │  ← Bottom of stack
└─────────┘
```

### State Transitions

Each state implements three methods:

- **start()**: Initialize the state, create windows
- **event()**: Handle input events
- **finish()**: Clean up, destroy windows

### Example: Opening the Pager

```
1. User presses <Enter> in Index
2. Index.event(Enter) → push_state(Pager)
3. Pager.start() → create pager window, set focus
4. Event loop now routes events to Pager
5. User presses 'q'
6. Pager.event('q') → pop_state()
7. Pager.finish() → hide pager window
8. Focus returns to Index
```

### Screen Types

| State | Description |
|-------|-------------|
| Index | Main email list view |
| Pager | Email content viewer |
| Compose | Email composition |
| Browser | File/folder browser |
| Help | Help pages |
| Alias | Address book |
| PGP/SMIME | Key selection dialogs |

---

## Future Architecture Goals

### Planned Improvements

1. **Widescreen Layout**: Index and Pager side-by-side
2. **Multiple Views**: Several emails open simultaneously
3. **Tab Support**: Browser-like tabs for different contexts
4. **Popup Windows**: Floating dialogs for prompts and selections
5. **Plugin Architecture**: External extensions with stable API

### Design Principles for New Features

- Follow the existing hierarchy patterns
- Use notifications for communication
- Keep data separate from views
- Make windows self-contained and reusable
