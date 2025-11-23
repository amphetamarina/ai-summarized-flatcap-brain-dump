# Architecture & System Design

Core architectural designs and system-level decisions for NeoMutt.

## Sources
- mvc.txt, account.txt, account-stack.txt, account-stack2.txt
- config.txt, config-command.txt, config-validation.txt
- notifications.txt, windows.txt, event-queue.txt

---

## MVC Architecture (mvc.txt)

### Main Loop Components
- Reads keyboard input
- Manages timers and signals
- Holds Accounts (set) and Windows (set)
- Tracks current focus and window state

### Core Objects

**Account**
- MX type (or set of function pointers)
- Set of Folders
- Credentials

**Folder**
- Name and description
- Set of Emails
- Attributes (e.g., Visible)

**Window**
- Attributes (min/max size, borders, color, z-order, modal)
- Set of children windows
- Render function
- Private data

### Event System
- Events: key, mouse, timer, filesystem, network, signal
- Object tree rendering with ACList hierarchy
- Listeners: Sidebar (all accounts/folders), Index (one folder), Pager (one email)

### Views and Tabs
- Multiple views supported (V1: A1,A2,A3; V2: A1,A3)
- View commands: new, close, clone
- Tab-based navigation for emails and accounts
- Breadcrumbs for navigation help

### State Transitions
- States: Index/Pager (home), Help, Browser, PGP Key selection
- State management: start(), event(), finish()
- Stack of states for navigation
- Key bindings can be local (focused window) or global

---

## Account Management (account.txt)

### Hierarchy
```
NeoMutt
  -> AccountList
    -> Account
      -> MailboxList
        -> EmailList
          -> Env, Body, etc
```

### Account Types
- IMAP: 1 server/port/user combo
- POP: 1 server/port/user combo
- Maildir: user-configurable set of folders
- NNTP: 1 server/port/user combo
- Notmuch: 1 notmuch database

### Account Configuration
- Named accounts with inherited config
- `account` command creates scoped configuration
- Mailboxes attached to accounts
- URL syntax: `account://NAME/rel-path` or `mailbox://ANAME/rel-path`

### Account-Specific Config
- Base -> Account -> Mailbox inheritance
- Subsets for each level
- Config lookup: M -> A -> N (fallback chain)

### Orphan Mailboxes
- Mailboxes without accounts (temporary)
- Resolved when needed
- Weak pointers for email references

---

## Configuration System (config.txt)

### Config Types
- Bool, Number, String, Quad, Enum
- Path, Command, Address, Regex
- Slist (string list with configurable separator)

### Features
- Validation before setting
- Notifications after changes
- Reset to defaults
- Inheritance (N -> A -> M levels)

### Config States
- Enabled: normal operation
- Disabled: quietly accepted but ignored
- Obsolete: accepted with warning
- Synonym: maps to alternative name

### Scoped Variables
- `set name = val` (global)
- `set acc:name = val` (account-specific)
- `set acc:mbox:name = val` (mailbox-specific)

### Advanced Features
- Push/pop config for temporary changes
- Validators for format strings, paths, etc.
- Lazy expansion with `:=` operator

---

## Notification System (notifications.txt)

### Notifiable Objects
- NeoMutt (N): AccountList, Config, WindowRoot
- Account (A): MailboxList, AView
- Mailbox (M): EmailList, MView
- Email (E): EView

### Event Types
- Config: initial set, set, reset
- NeoMutt: new account, delete account
- Account: new mailbox, delete mailbox, opened, closed, synced
- Mailbox: new email, delete email, changed email
- Window: hidden, visible, resize, focus in/out

### Listener Pattern
- Listen to object with (EventType, flags, callback, data)
- Notifications propagate upward through hierarchy
- Bundle notifications for batch operations

### Implementation
- Notifier (Type, parent, LIST(listeners))
- Event structs for versioned events
- Observer pattern for loose coupling

---

## Window System (windows.txt)

### Window Hierarchy
- RootWindow (V or H container)
- Nested containers (V = vertical, H = horizontal)
- Five main windows: Help, Index, Message, Sidebar, Status

### Window Attributes
- Min/Max size
- Borders
- Default color
- Expansion direction (vertical/horizontal)
- Z-order
- Modal flag

### Reflow Algorithm
- Invisible: skip, no space
- Fixed: allocate if possible
- Min: allocate, recurse, shrink around children
- Max: share remaining space

### Focus Management
- Keys: focus-up, focus-down, focus-left, focus-right
- Absolute moves: move-to-Sidebar, move-to-Index, etc.
- Focus stack for navigation history

### Future Directions
- Widescreen layout (Index and Pager side-by-side)
- Popup windows for questions
- Drop-down menus
- Modal dialogs with z-order

---

## Dialog System

### Dialog Types
- Index/Pager dialog
- Compose dialog
- Browser dialog
- Simple dialogs (help, version, etc.)

### Dialog Lifecycle
- dlg_new(): create dialog and windows
- dlg_push(): insert into hierarchy
- dlg_run(): main event loop
- dlg_pop(): remove from hierarchy

### Shared Data
- Current Account, Mailbox, Email
- Views (AV, MV, EV)
- Config subset

### Event Distribution
- Events tried on focused window first
- Propagate to siblings, then parents
- Generic events handled at root level

---

## Key Architectural Decisions

1. **Separation of Concerns**: Data objects (A, M, E) separate from views (AV, MV, EV)
2. **Observer Pattern**: Loose coupling through notifications
3. **Inheritance**: Config inheritance through N -> A -> M chain
4. **State Machine**: Dialog states with push/pop navigation
5. **Window Nesting**: Flexible layouts through container hierarchy
