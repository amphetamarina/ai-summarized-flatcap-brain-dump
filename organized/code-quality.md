# Code Quality & Refactoring

Code cleanup, refactoring tasks, and technical improvements.

## Sources
- trivial.txt, hard.txt, iwyu.txt, coccinelle.txt
- plus-equals.txt, function-triple.txt

---

## Trivial Improvements (trivial.txt)

### Coverity/Static Analysis
- `/* coverity[check_return] */`
- `/* coverity[suspicious_sizeof] */`
- Address "Poorly documented large function" warnings

### Code Cleanup
- Remove #ifdef for #defines and enums
- Convert #define num ranges to enum
- Eliminate static variables in functions
- Find and fix imbalanced #ifdefs
- Remove duplicate regex functions

### Type Safety
- Change `int buflen` to `size_t buflen`
- Use `intptr_t` vs `long` in config
- Shrink DT_LONG to BIGNUM
- Create typedefs for library headers

### String Handling
- Replace `mbtowc()` with `mbrtowc()`
- Rewrite `mutt_wstr_trunc()` for legibility
- Merge concat path functions
- Clean/dirty strwidth()/wcwidth() uses

### Buffer Conversions
- Convert `km_expand_key()` to use Buffer
- Unify `mutt_addrlist_write()` and `format_address_header()`

### Memory Management
- Use ARRAY for history search
- Convert `struct MhSequences` to `MhSeqArray`
- Find Linked List Nodes to replace with ARRAY

---

## Hard Refactoring Tasks (hard.txt)

### Architecture Changes
- Add separate 'default' colours for each panel
- Split up/remove BE-specific code in `ci_send_message()`
- Separate keybindings from curses & IMAP
- Eliminate AllMailboxes (depends on A-aware sidebar)

### Major Rewrites
- Browser rewrite
- NNTP refactor (auto subscription like IMAP)
- Change M->path from array to pointer
- Config `+=`, `-=` API

### System Improvements
- Account-specific config
- New help system with `$help_format_string`
- Sidebar account-aware
- HCache/BCache -> maildir integration
- Inline hcache, offline support

### Code Structure
- Change large switch statements to [opcode, fn()] table
- Add timeout structs + callbacks for keymap.c
- Eliminate IMAP dependencies from keymap

---

## Code Patterns

### Function Organization
- Reorder functions to reduce forward declarations
- Forward declare structs in headers to reduce includes
- Static functions first in files
- Sort definitions (vars, functions)

### Naming Conventions
- Rename statics to MixedCase
- Change `mh_data()` to `get_mh_data()`
- Standard naming: adata, mdata, edata
- Functions: `BE_DATA_{free,new,get}`

### Error Handling
- Reduce config error messages to error codes
- Add validators for authenticators
- Validate format strings when set
- `$tmpdir` validator (create sample temp file)

---

## Technical Debt

### Config System
- Filter synonyms and deprecated from get_elem_list()
- Separate DT_TYPE from DT_FLAGS
- Eliminate DTYPE macro
- Config: LCOV_EXCL_LINE -> assert

### Path Handling
- Split PATH & MAILBOX types
- DT_PATH flags: FILE_ONLY, DIR_ONLY, NO_SYMLINK, MUST_EXIST
- Validator check for readable/reachable

### Documentation
- OUT-params flagged in doxy header
- Docs of all config types + flag subtypes
- Update and move config design docs to web

---

## Code Standards

### Formatting
- Mark empty for loops with `/* nothing */`
- Use `while (1)` rather than FOREVER
- Check all pointers
- Check all string config vars are NONULL'd

### Macros
- Use `{ X; Y; Z; } while(0)` for multi-statement macros
- Unify `#if/#ifdef HAVE_X/USE_X`

### Comments
- Translatable comments
- Remove `\n` from 58 one-liner messages
- Comment to avoid warnings

---

## Testing Improvements

### Coverage
- List all functions without 100% test coverage
- List all tests that would be required
- Coverage for core: command, config_cache, neomutt, mailbox

### Test Infrastructure
- Change file tests to clean up tmp/ afterwards
- Replicate LOCALES_HACK in tests
- Test CLI params of "" (empty string)
- Weekly build action with ubsan

### Validation
- `neomutt -D?` options (validate muttrc without running)
- Only display CHANGED values
- Show default values/keybindings

---

## Build Improvements

### Compiler Flags
- `-D_FORTIFY_SOURCE=1` or `2`
- Fix strict-aliasing warnings
- SUPPRESS_UNUSED_WARNING macro
- SUPPRESS_UNUSED_RESULT_WARNING macro

### Dependencies
- Eliminate config from libmutt
- Move `mutt_ch_convert_nonmime_string()` dependencies
- Move `mutt_file_mkstemp_full()` `$tmpdir` dependency

---

## Menu Refactoring

### Structure
```
Dialog -> Container -> WinMenu
                         |
                         V (wdata)
                       Menu
                         | (mdata)
                         V
                       Priv Data { Rows[], state info }
```

### Features
- Move is_{matches,tagged,deleted,visible} into Menu-specific struct
- MenuView { state_info, void *payload }
- Combine MenuView and CustomView for fewer allocs

### Naming
- Rename `line` to `row` in Menu
- Fix Menu.search() naming too
