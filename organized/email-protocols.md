# Email Protocols & Backends

IMAP, NNTP, Notmuch, POP, Maildir integrations and protocol handling.

## Sources
- nntp.txt, notmuch.txt, imap-related files
- hcache.txt, hcache2.txt
- maildir.txt, new-mail.txt

---

## Backend Architecture

### Hierarchy
```
Account (backend-specific)
  -> Mailbox
    -> Email
```

### Backend Types
| Type | Description |
|------|-------------|
| IMAP | Remote IMAP server |
| POP | Remote POP server |
| NNTP | Usenet news groups |
| Notmuch | Email indexing/search |
| Maildir | Local maildir format |
| Mbox | Local mbox format |
| Compressed | Compressed archives |

### Backend Data
- `adata` - Account-specific data
- `mdata` - Mailbox-specific data
- `edata` - Email-specific data

---

## IMAP Integration

### Account Model
- One server/port/user combo per account
- Connection info stored in adata
- LSUB for subscribed folders

### Path Handling
- Canon path: `imaps://user@host:port/path`
- Login info in Account->data.conn
- $folder stored in Account->config

### Email Data
- `edata`: flags, uid

### Features
- Auto-subscription
- Keepalive timeout
- Header caching
- Body caching

---

## NNTP Integration (nntp.txt)

### Account Model
- One server/port/user combo
- All subscribed groups
- newsrc file for state

### Features
- Auto subscription (like IMAP)
- Article numbering
- Group descriptions
- Catchup/uncatchup

### Feature Requests
- Save newsgroup descriptions to file
- Display descriptions from server or local
- Format: `<newsgroup>:<description>` or `<newsgroup> <description>`

---

## Notmuch Integration (notmuch.txt)

### Account Model
- One notmuch database per account
- Query-based virtual mailboxes
- Cross-mailbox search

### Path Handling
- Path: `nm://query`
- Realpath includes db path

### Multi-Owner Emails
- Email has "canonical" owner (maildir)
- Notmuch listens to canonical M
- Transplants events to notmuch M

### Features
- Saved/pre-defined queries
- Window-back/forward navigation
- Tagging support

### Deprecation
- `virtual-mailboxes NAME PATH` deprecated
- Use `mailboxes -label NAME PATH` instead

---

## POP Integration

### Account Model
- One server/port/user combo
- Download-based model

### Email Data
- `edata`: uid

### Features
- pop_fetch_mail
- Message caching

---

## Maildir Integration

### Account Model
- Named account: root is A-$folder
- Unnamed: root is /

### Path Handling
- path = user entered, tidied
- canon_path = realpath()

### Features
- cur/new/tmp directory structure
- Flags in filename
- Header caching

---

## Header Cache (hcache.txt)

### Purpose
- Cache email headers for fast startup
- Reduce server round-trips

### Backends
- BDB
- GDBM
- Kyoto Cabinet
- LMDB
- QDBM
- Tokyo Cabinet

### Integration
- Opened at parse time
- Keep as A-data, keep open
- Move hcache/bcache -> maildir

### Future Work
- Inline hcache
- Offline support

---

## Body Cache

### Purpose
- Cache message bodies locally
- Support offline reading

### Features
- Backend-specific storage
- Expiration policies
- Compression support

---

## New Mail Detection (new-mail.txt)

### Methods
- Polling (timeout-based)
- IMAP IDLE
- inotify (local mailboxes)

### Notification Flow
1. Backend detects new mail
2. Notify Mailbox (new E * n)
3. Notify Account
4. UI updates (index, sidebar, status)

### Event Format
- `ENewMail { M, EL }`
- Bundle notifications for efficiency

---

## Backend Operations

### Open Sequence
```
mx_mbox_open(M)
  -> imap_acc_open(M -> A)
  -> imap_acc_open(A) // socket, conn
  -> imap_mbox_open(M)
  -> get email IDs (may be in hcache)
```

### States
**Mailbox:**
- Unknown (no mailboxes, no opens)
- Known (after mailboxes command)
- Open (populated)
- Double open (save, postpone, attach)

**Account:**
- Unknown
- Empty (conn info & config)
- Populated

### Check Operations
- List new
- List all
- Get headers (list)
- Get struct (list)
- Get body (list)

---

## Backend API Functions

### Account Functions
```c
ac_find_account(P)     // Find account for path
ac_find_mailbox(A, M)  // Find mailbox in account
BE_identify(P)         // path, canon, magic, A*
BE_resolve(M)          // Resolve to real M
```

### Mailbox Functions
```c
mbox_create(path, canon, magic)
mbox_open(M, flags)
mbox_check(M)
mbox_sync(M)
mbox_close(M)
```

### Path Functions
```c
BE->A_path_match(M->canon)  // Match path to account
BE->path_canon(path)        // Canonicalize path
BE->path_probe(path)        // Detect backend type
```

---

## Connection Handling

### Connection Info
- Host, port, user, password
- TLS/SSL settings
- Authentication methods

### Connection State
- Connected/disconnected
- Authenticated
- Keepalive timer

### Reconnection
- Automatic reconnect on connection loss
- Re-run account-hook on reconnect
- Preserve mailbox state

---

## Future Work

1. **Unified Backend Interface**: Common API for all backends
2. **Async Operations**: Non-blocking email fetches
3. **Better Caching**: Smarter cache invalidation
4. **OAuth Support**: Modern authentication
5. **Multiple Connections**: Parallel operations per account
