# Developer Tools & Infrastructure

Build system, CI/CD, debugging tools, and development workflow.

## Sources
- build-all.txt, travis.txt, travis-assistant.txt
- debug.md, curses-debug.txt
- mentoring.txt, challenges.txt
- release.txt, deploy.txt, distro.txt

---

## Build System (build-all.txt)

### Common Build Options
```
--enable-debug --enable-flock --enable-gpgme --enable-imap
--enable-pop --enable-smtp --with-gnutls --with-gss
--with-sasl --with-ssl --with-tokyocabinet
```

### Feature Evolution
| Version | Key Features |
|---------|-------------|
| mutt-1.5.23 | --enable-hcache |
| mutt-1.7.0 | --enable-hcache --enable-sidebar |
| neomutt-20160530 | --enable-compressed --enable-nntp --enable-notmuch |
| neomutt-20170414 | Added --enable-lua |

### Header Cache Backends
- BDB, GDBM, Kyoto Cabinet, LMDB, QDBM, Tokyo Cabinet

---

## CI/CD (travis.txt)

### Travis Configuration
- Multi-environment builds
- Feature flag combinations
- Valgrind, ASAN, UBSAN, Coverity testing
- Sample config validation

### GitHub Actions
- Weekly build with ubsan
- Weekly build with -D_FORTIFY_SOURCE
- Auto-sync with GitLab/etc
- Release commit count

### Testing Infrastructure
- neomutt-test-configs.sh
- Lua as testing framework
- Public access to test mailboxes
- save_results() for lua-testing

---

## Debugging (debug.md)

### Debug Levels
- 6th debug level for notifications
- Log all processes
- Change to debug(5) before merge

### Debug Files
- .neomuttdebug* files
- Log "neomutt -v" output on startup
- Backtrace on segfault

### Curses Debugging
- curses-debug.txt for ncurses issues
- Terminal window focus detection
- Repaint debugging

---

## Mentoring Program (mentoring.txt)

### Wiki Structure
- Get Involved page
- Spread the word
- mutt-newbies (cf kernel-newbies)
- Public mentoring emails

### Task Management
- Mark tasks: interested, working, needs help, new, finished
- Add category to issues
- Link to enhancements/bugfixes

### Code Tidy Guidelines
- Whitespace and indenting
- Commenting
- Introduce BOOL type
- Static functions
- Variable naming conventions

### Documentation Priorities
- Update docs
- Make user friendly
- Translate (split up)
- OPS: tidy into sentence case

### Sidebar Development
- Fix refresh
- Background colour
- Whitelist with +dir notation
- Add unwhitelist

---

## Version Management

### version.c Improvements
```c
{ string, bool },
{ "SIDEBAR", USE_SIDEBAR },
```
- Alpha sorted
- Queryable via ifdef/ifndef
- Optional 3rd arg: configure flag

### Print Options
- Print +/- USE_X mixed
- Print +/- USE_X separate
- Print only +USE_X
- Print only -USE_X
- Wrap at 80 chars

---

## Testing Strategy

### Automatic Testing
- Travis/GitHub Actions
- Valgrind for memory
- ASAN for address sanitizer
- UBSAN for undefined behavior
- Coverity for static analysis

### Manual Testing
- Test distros
- Screenshot collection
- User feedback

### Test Types
- Unit tests
- Integration tests
- Config validation
- Sample mailbox tests

---

## Development Workflow

### Version Control
- ifdef patch => ifndef command
- "finish" command for return
- Check for #include guards

### Code Review
- Review large functions
- Check pointer validity
- Validate string configs

### Release Process
- Commit count per release
- Update copyright dates
- Summary to ML + twitter
- Recalc top ten contributors

---

## Tools Integration

### Editor Support
- LanguageClient-neovim
- lc.vim for FreeBSD
- vim-cpp -> vim-neomutt-source

### Analysis Tools
- Include What You Use (IWYU)
- Coccinelle semantic patching
- Code-scanning warnings

### Documentation Tools
- Doxygen
- transform-links.vim
- deprecated.patch
- Website themes (sphinx, doxygen-awesome-css)

---

## Distro Support

### Distro Helper
- Instructions for install/build/test
- Check installs
- Test problems
- One+ person per distro

### Packaging
- COPR builds
- Repology badge
- Installation instructions

---

## Challenge System (challenges.txt)

### Community Challenges
- Tied to mentoring
- Progressive difficulty
- Recognition system

### Types
- Code challenges
- Documentation challenges
- Testing challenges
- Translation challenges
