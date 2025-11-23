# Scripting & Extensibility

Lua scripting integration and extensibility features.

## Sources
- lua/*.txt (lua.txt, config.txt, objects.txt, pre-compile.md)
- lua/logging.txt, lua/mute.txt

---

## Lua Integration Overview

### Current Status
- Basic Lua integration available
- `lua` and `lua-source` commands
- Limited documentation

### Goals
- Testing framework
- Configuration scripting
- Email processing
- UI customization

---

## Lua as Testing Framework

### Required Exposures
- Index functions
- Sidebar functions
- Variables
- Macros
- Function calls
- Send keystrokes
- Get error codes

### Testing Features
- `save_results()` function
- Travis/CI integration
- Public test mailboxes

---

## Lua Scripting Features

### Configuration
- Inline Lua in muttrc
- Loops and conditionals
- Variable expansion

### Embedded Syntax Options
```
<lua> ... </lua>   # Tagged sections
<?lua if (): ?>   # PHP-style conditionals
lua-execute        # Direct execution
```

### Output to NeoMutt
- Override print function
- Special printf-like function
- Direct muttrc output

---

## Lua Coloring

### Pager Coloring
For each line:
1. Call Lua script
2. Pass copy of envelope + line number
3. Returns: success, stop, hide line

### Callbacks
- `set_colour(object)` - Set color for object
- `set_colour(range)` - Set color for range
- `change_line(new_text)` - Replace line
- `insert_line(new_text)` - Insert new line
- `delete_line(num_lines)` - Delete lines

### Test Rig
```bash
cat mbox | test-rig config.lua
# ANSI output
```

### Flags for Highlighter
- Header, body, continuation line
- Quoted (user regex)
- Signature, weeded

---

## Lua Feature Modules

### lua-index
- Index colouring
- Folding support
- Custom sorting

### lua-pager
- Email colouring
- Syntax highlighting
- Markdown support
- Diff-so-fancy style

### lua-status
- Custom status bar formatting
- Dynamic updates

### lua-sidebar
- Counter manipulation
- Drafts folder: new count = total count

### lua-new-mail
- Mark, tag, move mail
- Filter scripts
- `(un)new_mail_filter lua [lua...]`

### lua-terminal-title
- Generate title from mailbox/email info

### lua-cron
- Queue scripts for later execution

### lua-hooks
- `XXX-hook = lua:file.lua`
- Existing mutt hooks via Lua

---

## Lua API Functions

### Styling Callbacks
```c
// C functions
hprintf(HANDLE, "text")
push_attr(HANDLE, C_FG_RED)
push_attr(HANDLE, A_REVERSE)
pop_attr(HANDLE)

// Shared definitions
C_FG_RED, C_FG_BLUE, A_UNDERLINE, A_REVERSE
```

### Example Usage
```lua
hprintf(HANDLE, "hello ")
push_attr(HANDLE, C_FG_RED)
push_attr(HANDLE, A_REVERSE)
hprintf(HANDLE, "world")
pop_attr(HANDLE)
pop_attr(HANDLE)
```

---

## Lua State Management

### Script Manager
```c
lua_open(name)   // Returns handle
lua_close(handle)
```

### Security
- Separate states per area (sandboxing)
- Prevent scripts from accessing passwords
- Different variables/functions per state

### State Lifecycle
- One state preserves info between calls
- States NOT shared between areas
- Primitive sandboxing

---

## Lua Variables System

### Interrogation Mode
1. Script called with `INTERROGATE_VARIABLES=1`
2. Script sets vars, returns `I_SET_VARS`
3. Use vars to control future calls

---

## Pre-compiled Lua

### Benefits
- Faster startup
- Smaller distribution
- Syntax validation at build time

### C Integration
- Search for pre-compiled Lua first
- Fallback to source files

---

## Lua Development Guide

### Masterclass Commits
1. Add variable
2. Add function
3. Add autoconf

### Commit Examples
- Call from C
- Check return value
- Call C from Lua
- Register C function
- Pass data to Lua
- Receive data from Lua

### Generic Stack Popper
- n entries
- Check type
- Get value

---

## Embedded Colors

### In-text Colors
```
<#RGBA>text<#default>
```

### Language Highlighting
```
`C
code
`
```
Single backtick to end.

---

## Future Scripting Ideas

### lua-alias
- `is_alias()` for highlighting addresses

### lua-folding
- Vim-like folding
- Linewise processing

### lua-sort
- qsort with Lua logic

### lua-mark-old
- Enter/leave timestamps
- Mark read, old, or nothing

### Duplicate Deletion Script
- Save outgoing to mailing list
- Delete "sent" copy when "public" arrives

---

## Documentation Needs

### Missing Documentation
- Feature page with examples
- API reference
- Tutorial for beginners

### Example Code
- Include in commit messages
- Provide sample scripts
- Show common use cases

---

## Export Functions

### For Compatibility
- `mutt_regex_match` exported
- Version info to Lua state

### NeoMutt Info
- Mutt version
- Date
- Patches
- Build options
