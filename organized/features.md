# Features & UI/UX Enhancements

This document catalogs proposed features, UI improvements, and user experience enhancements for NeoMutt. These range from small quality-of-life improvements to major new capabilities.

## Source Files
- `brain-dump/dialog.txt` - Dialog system designs (750+ lines)
- `brain-dump/summary.txt` - Summary page proposals (350+ lines)
- `brain-dump/compose-preview.md` - Compose preview feature
- `brain-dump/mouse.txt` - Mouse support (250+ lines)
- `brain-dump/focus.txt`, `brain-dump/help-tabs.txt`
- `brain-dump/panel/*.txt`, `brain-dump/sidebar/`, `brain-dump/pager/`

---

## Summary Pages

One of the most significant proposed feature sets is a collection of "summary pages" - introspection commands that let users see the current state of NeoMutt's configuration and bindings.

### The Vision

Users often struggle to understand what NeoMutt is doing under the hood. These summary pages would provide transparency through simple commands typed at the `:` prompt:

### Proposed Commands

| Command | Purpose | Details |
|---------|---------|---------|
| `:bind` | Show keybindings | Display all keybindings and macros, grouped by context (Index, Pager, etc.). With "all" option, show everything that *can* be bound. |
| `:color` | Show colors | List all objects that have been colored, shown as valid `color` commands with preview. With "all" option, show everything that *can* be colored. |
| `:help` | Access manual | Show the text version of the manual, generated at build time. Could be expanded into a generic `display-file` function. |
| `:messages` | Show message history | Display all messages and errors with timestamps. Limit to ~100 entries. "clear" option empties the buffer. |
| `:scripts` | Show loaded configs | List all configuration files loaded at startup or via `source`. Red-flag any that had errors. |
| `:set` | Show variables | Display changed variables. With "all" option, show every variable. Highlight changes from defaults. |
| `:version` | Show version info | Detailed version information like `neomutt -v`, including patches and compilation flags. |

### Additional Ideas

- `:about` - Brief description of the NeoMutt project
- `:tips` - Enable "tips" mode showing brief guides
- `:credits` - List of contributors
- `:hooks` - List all hooks (numbered) with matching folders
- `:patterns` - List all pattern modifiers
- `:changelog` - Read changelog from file

### Implementation Notes

- Opening a summary page should close any existing summary/help page
- Status bar shows list of available summary pages when viewing one
- Support saving page content to file (strip colors)
- Support opening in external editor
- Context-aware: `:set` should depend on current account/mailbox

---

## Dialog System Improvements

The dialog system manages multi-screen workflows like Compose, Browser, and Help. Proposed improvements would make it more flexible and user-friendly.

### Layer Architecture

Dialogs are organized in layers. Each layer owns a complete set of windows:

```
Layer: Index/Pager
├── HelpLine
├── Sidebar
├── Index
├── IndexBar
├── Pager (optional)
└── PagerBar (optional)

Layer: Compose
├── HelpLine
├── Envelope
├── AttachmentBar
├── AttachmentView
└── ComposeBar
```

### Sidebar as Overlay

An innovative idea is making the sidebar an overlay that appears on demand:

- `<sidebar-show>` makes it visible with a timeout
- After timeout (or after any action), sidebar hides automatically
- `<sidebar-hide>` for manual dismissal
- Perfect for users who want space but occasional folder access

### Dialog Breadcrumbs

Status bar could show navigation breadcrumbs:
```
Index → Compose → Attach?
```
Only visible when navigating deeper than the main screen.

### Tab-Based Navigation

Future vision includes browser-like tabs:
- Each tab could hold a different account or view
- Backgrounded compose sessions shown in tabs
- `Alt-1`, `Alt-2`, etc. for quick switching

---

## Index Improvements

The Index is where users spend most of their time. These improvements would make it more powerful.

### Vim-Style Search

Implement search features inspired by Vim:

- **Highlight-search**: Matching emails highlighted after search
- **Incremental search**: Results update as you type
- **Visual feedback**: See matches before pressing Enter
- **Automatic filtering**: Optional mode where typing filters in real-time

### Enhanced Patterns

Currently, patterns only apply to certain fields. Proposed expansion:

- Add patterns (optional) to all index fields
- Tag-pattern for the browser (useful for compose-attach)
- More index color patterns with optional conditions

### Search by Message-ID

The `~i REGEX` pattern for matching message IDs is problematic because message IDs often contain regex special characters. Proposed solution:

```
~i REGEX      # Current behavior: text is regex
~i <TEXT>     # New: text is literal (angle brackets = literal mode)
```

---

## Pager Improvements

The Pager displays email content. These features would enhance readability and usability.

### Bare Mode

Inspired by Weechat, a "bare mode" for clean copy-paste:

- Wrapping, markers (`+`), and scrollbars interfere with selecting text
- `<bare-mode>` (suggested: Ctrl-B) drops into basic pager using `endwin()`
- No wrapping, just dumps email to screen
- Let terminal handle page up/down
- `q` to exit back to normal mode
- Optionally use ANSI sequences for clickable URLs
- Eliminates need for external tools like urlview/urlscan

### Toggle-Quoted Fixes

Current `<toggle-quoted>` has issues with `$toggle_quoted_show_levels`:

- If there are multiple quoting styles (`>` and `|`), only the first is handled
- Should recognize all quoting styles, not just the first encountered

### Syntax Highlighting Plugin System

Allow Lua plugins to define highlighting rules:

```lua
rule "signature" {
  apply_to = folder("[pattern]") or email("[pattern]"),
  begin_pattern = "^-- $",
  end_pattern = "^$",
  max_lines = 4,
  color = { fg = "gray", bg = "default" },
  conceal = true  -- optionally hide matching content
}
```

---

## Color System Enhancements

NeoMutt's color system could be more powerful and flexible.

### Conceal Mode

New `MT_COLOR_CONCEAL` for content that's "not there":

- Use for signatures, weeded headers, toggle-quoted text
- `set conceal_level = [0123]` (like Vim)
- `set conceal_char = '†'` to show placeholder
- Toggle visibility without losing the color definition

### Column Colors

Apply colors to specific columns in index_format:

```
# Color just the subject column green
color index_subject green default
```

### Partial Match Colors

Extend header coloring to support partial matches:

```
# Current: colors entire Subject header
color header green default "^Subject"

# Proposed: color just the matched group
color header green default "^Subject: (.*)" 1
```

### Truecolor Fallback

Support both truecolor and 256-color in the same theme:

```
# Theme file with increasing color depth
color object red green           # 16-color fallback
color object color123 color214   # 256-color
color object #aabbcc #112233     # Truecolor (if supported)
```

Last viable setting wins, allowing one theme file for all terminals.

---

## Compose Improvements

The Compose screen creates outgoing messages. Proposed enhancements:

### Window Structure

```
┌─────────────────────────────────┐
│ Envelope (headers)              │
├─────────────────────────────────┤
│ Attachment Bar (status)         │
├─────────────────────────────────┤
│ Attachment View (list)          │
├─────────────────────────────────┤
│ Compose Bar (actions)           │
└─────────────────────────────────┘
```

### Preview Window

A new window showing rendered email preview:
- See how the email will look to recipients
- Update in real-time as attachments are added
- Toggle visibility with a keybinding

### Attachment Status Bar

New `$attach_status` to configure the AttachBar:
- Default: "Attachments" or "Attachments: %n"
- Allow email expandos: "To: %t, Subject: %s"
- Show cumulative attachment size

### Multiple Compose Sessions

Background compose sessions:
- Start composing, switch back to Index
- Status bar shows backgrounded sessions
- Return to any session from a menu

---

## Sidebar Enhancements

The Sidebar shows folder list and navigation.

### Account-Aware Sidebar

Make sidebar understand account boundaries:

```
# Show only these accounts
set sidebar_account = work,personal

# Collapse these accounts by default
set sidebar_collapsed = lists
```

### Merged Inbox Display

Use $spoolfile to merge inbox into account line:

```
# Without merge:
work
├── inbox  1/6
├── sent
└── drafts

# With merge (spoolfile=inbox):
work           1/6
├── sent
└── drafts
```

### Account Tabs

For users who don't want sidebar space:
- Display account tabs at top
- Click (or key) to switch accounts
- Combine with sidebar for favorites

---

## Mouse Support

Enable mouse interaction throughout NeoMutt.

### Proposed Features

| Area | Action | Result |
|------|--------|--------|
| Index | Click row | Select email |
| Index | Scroll | Page up/down |
| Sidebar | Click folder | Open folder |
| Sidebar | Click account | Expand/collapse |
| Pager | Scroll | Scroll content |
| Pager | Click URL | Open in browser |
| Panel borders | Drag | Resize panels |

### Configuration

```
set mouse = yes
set mouse_click_selects = yes
set mouse_scroll_pages = yes
```

---

## Help System Improvements

### Tabbed Help

Implement tabbed help pages:
- Multiple help topics open simultaneously
- Easy switching between related topics
- Browser-like navigation (back/forward)

### Context-Sensitive Help

Enhanced F1/F2 context help:
- Fine-grained focus tracking
- Window-specific help: "browser-pgp" → "browser" → general
- Option to open online web help instead

### Dynamic Help Bar

The help bar at the bottom could be customizable:

```
bind index <key> <function> show_in_help
```

Users can:
- Clear all default help entries
- Add their own commonly-used bindings
- Prioritize what appears in limited space

---

## Alias System Enhancements

### Unified Groups and Tags

Merge the concepts of address groups and alias tags:

- Group patterns would match tags
- Consistent syntax across all group-related commands
- New syntax: `<mail>@group` to send to entire group

### Alias Preview

When selecting aliases:
- Pager/preview showing full details
- Especially useful for groups
- Show all members with their addresses

---

## Browser Improvements

### Directory Scanning

Option to scan subdirectories for maildir markers:
- Highlight `{cur,new,tmp}` directories as mailboxes
- Make maildir folders more visible in file browser

### Path Completion Fixes

Current issues with spaces in paths:
- `/srv/with space/<tab>` doesn't work intuitively
- Browser location doesn't transfer to command prompt
- Need better integration between browser and input

---

## Format String Editor

Long-term idea: interactive format string customization

- Visual editor for `$index_format`, etc.
- Drag-and-drop columns
- Live preview of changes
- Similar to htop's column configuration

---

## Terminal Integration

### Window Focus Detection

Detect when terminal window gains/loses focus:
- Pause background operations when unfocused
- Resume checking when refocused
- Visual indication of focus state

### Terminal Title

Customizable terminal title:
- Show current folder name
- Show unread count
- Update dynamically as state changes
