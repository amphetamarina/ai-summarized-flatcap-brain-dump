# Features & UI/UX Enhancements

Feature requests, UI/UX improvements, and enhancement proposals.

## Sources
- dialog.txt, summary.txt, compose-preview.md, reflow.md
- notify.md, folder-completion.md, startup.md
- mouse.txt, focus.txt, help-tabs.txt
- panel/*.txt, sidebar/, pager/

---

## Summary Pages (summary.txt)

### Proposed Commands
| Command | Description |
|---------|-------------|
| `:bind` | Show list of keybindings |
| `:color` | Show list of colours |
| `:help` | Easy access to manual |
| `:messages` | Show old mutt messages |
| `:scripts` | Show loaded config files |
| `:set` | Show list of variables |
| `:version` | Show detailed version info |

### Additional Commands
- `:about` - Brief project description
- `:tips` - Show brief guides
- `:credits` - List of contributors
- `:hooks` - List all hooks (numbered)
- `:patterns` - List pattern modifiers

### Features
- Save page to file
- Open in editor
- Syntax highlighting
- Context-sensitive (account/mailbox aware)

---

## Dialog System Improvements (dialog.txt)

### Window Management
- Status bar shows list of summary pages
- Dialog breadcrumbs in status bar
- Tab-based navigation between dialogs
- Sidebar as overlay option with timeout

### Focus Handling
- Fine-grained focus for context-sensitive help
- Window pointer with unique/qualified names
- Focus stack for push/pop navigation

### Layer System
- Layers own: Helpline, sidebar, index, pager, status
- Change folder/account preserves windows
- Compose gets new complete layer

### Pager Enhancements
- Separate: Text, Markup, Display layers
- Plugin modifiers for colors/styles
- Raw email as read-only mmap'd data

---

## Index/Pager Improvements

### Index Features
- Highlight-search (vim-like)
- Incremental search with visual feedback
- Automatic filtering as you type
- Tag-pattern for browser

### Pager Features
- Bare mode (cf weechat) for copy/paste
- Toggle-quoted with proper level handling
- Syntax highlighting plugins
- Folding rules (vim-like)

### Status Bar
- Hide when empty
- Dividing line between index and pager
- Dynamic help bar with custom bindings

---

## Color System Enhancements

### Color Commands
- `color toggle object [pattern]` - temporarily change visibility
- Apply color to columns in index_format
- Partial matches like status color

### Color Features
- Truecolor with simple/palette fallback
- Linked colors for depth fallback
- Quoted colors merged correctly
- Color notifications for regex changes

### Theme Support
- `~/.config/neomutt/theme.rc` file/symlink
- Separate theme and keybindings configs
- Multiple color settings (increasing depth)

---

## Mouse Support (mouse.txt)

### Proposed Features
- Click to select in index
- Click to open in sidebar
- Click to navigate in pager
- Drag to resize panels
- Scroll wheel support

---

## Compose Enhancements

### Window Structure
- Envelope (custom window)
- Attachment bar (simple bar)
- Attachment view (menu)
- Compose bar (custom)
- Preview window

### Features
- Move focus between fields
- Dynamic field sizing
- Abbreviation for long lists
- Tab for backgrounded compose sessions

---

## Sidebar Enhancements

### Account-Aware Sidebar
- `sidebar_account=X,Y,Z` for filtering
- `sidebar_collapsed=X,Y,Z` for collapsing
- Account tabs (dash-to-dock style)
- Merge inbox into account line with $spoolfile

### Display Options
- `sb_display_account` list
- Sort by account, then mailbox
- Visibility toggle per account

---

## Help System

### Help Tabs
- Tabbed help pages
- Context-sensitive (F1/F2)
- Online web help option

### Dynamic Help Bar
- `bind/macro ... show_in_help` flag
- Customizable entries
- Clear all and recreate option

---

## Browser Improvements

### Features
- Scan subdirs for {cur,new,tmp} highlighting
- Space handling in path completion
- Transfer browser location to command prompt
- Option to show maildir subdirs

---

## Alias System

### Unified Groups and Tags
- Group patterns match tags
- Group commands extended
- Syntax: `<mail>@group` to send to group

### Alias Dialog
- Pager/preview for details
- Group indication field
- Sort and limit by group

---

## Miscellaneous Features

### Conceal Mode
- `MT_COLOR_CONCEAL` for invisible content
- Use for signatures, headers, toggle-quoted
- `set conceal_level = [0123]`
- `set conceal_char = '†'`

### History Improvements
- Pattern matching in history (`:set sidebar<UP>`)
- ARRAY-based history search

### Terminal Features
- Window focus in/out detection
- Terminal title script
- Segfault backtrace to file

### Address Book
- Index_format expando for alias short-name
- Colors based on address patterns
- "in-address-book", "in-address-group-X"

---

## Future Ideas

1. **Widescreen Layout**: Index and Pager side-by-side
2. **Multiple Email View**: Side-by-side email comparison
3. **Browser Tabs**: Implementation via tabbed-help-page first
4. **Dynamic Format Strings**: Generic format string editor (cf htop)
5. **Plugin System**: Lua-based plugins for colors, folding, etc.
