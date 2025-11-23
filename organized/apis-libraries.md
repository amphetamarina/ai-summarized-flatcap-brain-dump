# APIs & Libraries

API specifications, library design, and interface documentation.

## Sources
- api/*.txt (api.txt, completion.txt, menu.txt, path.txt, pattern.txt, list.txt)
- libaddress.txt, libmenu.md, library.txt
- mxapi.txt

---

## Core API Principles

### Design Goals
- Clean separation of concerns
- Minimal dependencies between modules
- Clear ownership of data
- Consistent naming conventions

### Naming Conventions
- Variables: `adata`, `mdata`, `edata`
- Functions: `BE_DATA_{free,new,get}` (in this order)
- Variables: `a` Account, `m` Mailbox, `e` Email, `ctx` Context

---

## Mailbox API (mxapi.txt)

### MX Operations Split
**Account Level:**
- `path*` - path operations
- `mbox_open*` - open mailbox
- `msg_padding` - message padding

**Mailbox Level:**
- `msg_open*` - open message
- `mbox_check/sync/close` - mailbox operations

**Email Level:**
- `tags*` - tag operations (or at M level)
- `msg_commit` - commit message
- `msg_close` - close message

### API Guarantees
For all M in AllMailboxes:
- `M->A` exists
- `M->magic` is set
- `M->mxops` is set
- `M->path` set and tidy
- `M->realpath` set (canonical)

### Key Functions
```c
int mx_mbox_open(M*, flags);
M* mx_mbox_find(path, flags);
M* mx_resolve_path(P);  // -> M(P, R, magic, A), orphan flag
```

### Path Handling
- `path` - user specified, tidied, undecorated
- `realpath/canon` - backend-specific, fully decorated
- `path_canon($folder, mailbox)` -> `canon_mailbox`
- `path_pretty` -> `path_tidy` (just path section)

---

## Completion API (api/completion.txt)

### Features
- Tab completion for commands
- Context-aware completion
- Account/mailbox scoping
- History integration

### Completion Types
- Config variables
- Commands
- Paths/files
- Mailboxes
- Account names

### Scoped Completion
```
"set <tab>"       -> all BASE config names + account names
"set acc:<tab>"   -> Account-specific config + mailbox names
"set acc:mbox:<tab>" -> Mailbox-specific config names
```

---

## Menu API (api/menu.txt)

### Menu Structure
```
Window -> Menu -> Draw Data
```

### Menu Functions
- `menu_recalc()` <- recalc window position/limits
- `menu_repaint()` <- loop, make_entry() & display

### Menu States
- Custom data array
- Max, top, pagelen, index
- Window dimensions

### Redraw Triggers
- Data changes -> WA_REPAINT
- Dimension changes -> WA_RECALC

---

## Path API (api/path.txt)

### Path Types
- User path (entered by user)
- Canonical path (backend-specific)
- Pretty path (for display)

### Path Operations
```c
canon(M)           // Canonicalize path
probe(M)           // Detect mailbox type
tidy(path)         // Clean up path string
path_wrap(P)       // Wrap path in Mailbox
```

### Path Decorations
- IMAP: `imap://user@host:port/path`
- Notmuch: `nm://query`
- Maildir: plain filesystem path

---

## List API (api/list.txt)

### List Types
- AccountList
- MailboxList
- EmailList
- Array-based lists

### List Operations
- Add/remove items
- Find by ID/path
- Iterate
- Sort

### Reference Counting
```
Mailbox refcounts:
  new +1, free -1, resolve 0
  subscribe +1, unsubscribe -1
  find +1, ac_add +1, ac_remove -1
```

---

## Pattern API (api/pattern.txt)

### Pattern Matching
- Regex patterns
- NeoMutt patterns (~f, ~t, etc.)
- Combined patterns

### Pattern Functions
- Create/compile pattern
- Match against email
- Free pattern

---

## Library Design

### libaddress
- Address parsing
- RFC822 formatting
- IDNA support
- Group handling

### libmenu
- Menu creation/destruction
- Item management
- Selection handling
- Scrolling

### libmutt
- Core utilities
- String handling
- Memory management
- File operations

---

## API Best Practices

### Function Parameters
- Use `**` for out-params that allocate
- Use `*const` for read-only pointers
- Add Buffer ptr for error messages

### Error Handling
- Return error codes
- Fill error buffer with message
- Don't call mutt_error() in APIs

### Memory Management
- Clear ownership rules
- Reference counting where needed
- Free functions paired with new

### Thread Safety
- Add locking to set functions
- Separate state per thread
- Avoid global state

---

## Typedefs for Library Headers

To reduce dependencies:
```c
typedef int (*sort_t)(const void *, const void *);
typedef int (*format_t)(char *, size_t, ...);
typedef int (*handler_t)(...);
typedef int (*module_init_config_t)(struct ConfigSet *);
```

---

## Future API Work

1. **Abstract Config**: All config through subset interface
2. **Event-Driven**: APIs notify on changes
3. **Plugin Support**: Stable API for external plugins
4. **Backend Abstraction**: Unified interface for all mailbox types
