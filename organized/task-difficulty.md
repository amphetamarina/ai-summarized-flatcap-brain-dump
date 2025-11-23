# Tasks by Difficulty Level

Tasks organized by difficulty for contributor onboarding and project planning.

## Sources
- easy.txt, medium.txt, hard.txt, trivial.txt
- challenges.txt, mentoring.txt

---

## Easy Tasks

*Simple, non-contentious, quick to merge. Keep a minimum of 5 easy coding tasks available.*

### Documentation
- Quick guides for format strings, tagging, alias labels
- Document tags in alias, mail to user ML
- Add to "Did You Know" tips
- Translations to web page
- Doxygen to web /code dir

### Code Cleanup
- Clean build warnings (ccache, neomutt-test-configs.sh)
- Gettext strings cleanup
- `"` -> `'` in muttrc files
- Autogen vim syntax file
- Find and fix imbalanced #ifdefs

### Small Features
- `neomutt -DD` (defaults with annotations)
- Hide status/pager bar when empty
- Limit-to-tag function
- User prompting: `:ask VARIABLE "prompt"`
- Postpone question to three-way (Save, Delete, Cancel)

### Refactoring
- Change `int buflen` to `size_t buflen`
- Merge mutt_file_concat_path functions
- Replace mbtowc() with mbrtowc()
- Refactor parse_hooks to use flags
- Eliminate static variables (starting with libraries)

### Testing
- Test a distro, give feedback
- Screenshots
- List functions without 100% test coverage
- Change file tests to clean up tmp/

### Infrastructure
- Travis build scripts
- Distro helper (install, build, test instructions)
- On segfault, write backtrace to file

### Config System
- Disabled config support
- Docs of all config types
- Validators for format strings
- Validators for pop/imap/smtp/nntp authenticators

### API Improvements
- Add contract to mxapi
- Add API mbox_is_empty()
- Convert mutt_command_get() to use bsearch()
- set_focus() return old focus

---

## Medium Tasks

*Moderate difficulty, requires understanding of codebase.*

### Features
- More index color patterns (optional patterns)
- Query append (change title to "X + Y + Z")
- Alias pager/preview for details

### Refactoring
- Convert history search to use ARRAY
- Eliminate config from libmutt
- STAILQ for smimekey/pgpkey
- MuttIndexWindow workaround
- Refactor mbox_path_probe

### Infrastructure
- Log "neomutt -v" output on startup
- Usage() autowrap at $COLUMNS
- Find all OUT-params and ensure variables are set

### Documentation
- Translations printf format ordering doc
- Expando long-text review
- Docs for %{name} expandos

---

## Hard Tasks

*Complex refactoring and architectural work.*

### Architecture
- Add separate 'default' colours for each panel
- Split up/remove BE-specific code in ci_send_message()
- Colours/attrs: parse config, summary, implement primitives
- Account-specific config
- Browser rewrite

### Major Features
- New help system with $help_format_string
- Sidebar account-aware
- HCache/BCache -> maildir integration
- Inline hcache, offline support

### Refactoring
- NNTP refactor (auto subscription like IMAP)
- Config +=, -=, etc API
- Change M->path from [] to *
- Eliminate AllMailboxes
- Separate keybindings from curses & IMAP

### Code Structure
- Rewrite mutt_wstr_trunc() (rename variables, refactor)
- Change large switch statements to [opcode, fn()] table
- Add timeout structs + callbacks for keymap.c

---

## Trivial Tasks

*Small fixes that can be done quickly.*

### Comments and Warnings
- Add coverity comments to avoid warnings
- Comment out deprecated commands
- Mark empty for loops with `/* nothing */`

### Naming
- Rename line to row in Menu
- Rename variables in mutt_wstr_trunc()

### Small Fixes
- Not true comment in mutt/file.c about O_NOFOLLOW
- Drop 'i' binding for quit/exit in pager
- Filter synonyms and deprecated from get_elem_list()

### Testing
- Check which dummy() functions are still needed
- Test build with ubsan
- Weekly GitHub action for testing

### Documentation
- Update docs/config URLs to be linkified
- Drop mono command from docs examples
- Update gfx repo compose screenshots

---

## Contributor Guidelines

### For New Contributors
1. Start with Easy tasks
2. Read existing code before modifying
3. Test your changes
4. Follow coding standards
5. Ask for help when needed

### Task Progression
- Easy -> Medium -> Hard
- Complete at least 2 Easy tasks before Medium
- Complete at least 2 Medium tasks before Hard

### Recognition
- Praise on dev-ml for H,M task completion
- Monthly email to devel ML
- Update contributor credits

---

## Mentoring Notes

### Wiki - Mentoring Page
- Get Involved section
- Spread the word
- mutt-newbies (cf kernel-newbies)
- Make public: all mentoring emails

### Task Marking
- Someone's interested
- Someone's working on it (needs help?)
- New task
- Recently finished

### Extra Points
- Task (n+1): find more examples to fix
- Link new easy tasks to old ones
- Ask preferred communication method

### Newbie Notes
- PR vs branch differences
- Can't co-work on a PR
- [ci skip], #123 docs
