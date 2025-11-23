# Tasks by Difficulty Level

This document organizes development tasks by difficulty level, making it easy for contributors to find appropriate work. Whether you're new to NeoMutt or an experienced developer, there's something here for you.

## Source Files
- `brain-dump/easy.txt` - Simple tasks (180+ lines)
- `brain-dump/medium.txt` - Moderate tasks (50+ lines)
- `brain-dump/hard.txt` - Complex tasks (35+ lines)
- `brain-dump/trivial.txt` - Quick fixes (350+ lines)
- `brain-dump/mentoring.txt` - Contributor guidance

---

## How to Use This Guide

### For New Contributors

1. **Start with Trivial or Easy tasks** - These are designed to familiarize you with the codebase
2. **Read existing code first** - Understand the patterns before making changes
3. **Ask questions** - The community is friendly and helpful
4. **Complete 2-3 Easy tasks** before attempting Medium ones

### For Experienced Contributors

- **Medium tasks** require understanding of multiple subsystems
- **Hard tasks** are architectural changes that may take weeks
- Consider mentoring new contributors on Easy tasks

---

## Trivial Tasks

*Quick wins that can be completed in under an hour. Perfect for your first contribution.*

### Documentation Fixes

| Task | Description |
|------|-------------|
| URL linkification | Make URLs in docs/config clickable |
| Drop mono examples | Remove `mono` command from docs examples |
| Update screenshots | Refresh gfx repo with compose preview window |
| Printf format docs | Document printf format ordering for translators |

### Code Comments

| Task | Description |
|------|-------------|
| Coverity comments | Add `/* coverity[check_return] */` to suppress false positives |
| Empty loop markers | Mark empty `for` loops with `/* nothing */` |
| Remove \n from messages | 58 one-liner messages ending in `\n` need cleanup |

### Small Refactors

| Task | Description |
|------|-------------|
| Rename line → row | Menu functions use "line" but should use "row" |
| Filter deprecated | Remove synonyms and deprecated from `get_elem_list()` |
| Check dummy functions | Verify which `dummy() {}` functions in tests are still needed |

---

## Easy Tasks

*Straightforward tasks that teach you about the codebase. Should take a few hours to a day.*

### Documentation Tasks

| Task | Description | Skills |
|------|-------------|--------|
| Quick guides | Write guides for format strings, tagging, alias labels | Writing |
| DYK tips | Add "Did You Know" tips content | Writing |
| Translation updates | Update translations on web page | i18n |
| Doxygen | Generate code docs for web /code directory | Doxygen |

### Code Cleanup

| Task | Description | Skills |
|------|-------------|--------|
| Build warnings | Clean ccache and test config warnings | C, Build |
| Gettext strings | Improve translatable string consistency | i18n |
| Vim syntax | Auto-generate vim syntax file for muttrc | Vim |
| Imbalanced ifdefs | Find and fix imbalanced `#ifdef` blocks | C |

### Small Features

| Task | Description | Skills |
|------|-------------|--------|
| `neomutt -DD` | Show defaults with annotations | C |
| Hide empty bars | Don't display status/pager bar when format is empty | C |
| Limit-to-tag | Function to limit index to tagged messages | C |
| Postpone 3-way | Change postpone question to Save/Delete/Cancel | C |

### Type Safety

| Task | Description | Skills |
|------|-------------|--------|
| buflen → size_t | Change `int buflen` to `size_t buflen` | C |
| Concat path merge | Merge `mutt_file_concat_path` functions | C |
| Replace mbtowc | Use `mbrtowc()` instead of `mbtowc()` | C |

### Testing

| Task | Description | Skills |
|------|-------------|--------|
| Distro testing | Test on a distribution, report issues | Testing |
| Coverage list | List functions without 100% test coverage | Testing |
| Cleanup tmp | Change file tests to clean up tmp/ afterward | C |

### API Improvements

| Task | Description | Skills |
|------|-------------|--------|
| mxapi contract | Add contract documentation to mxapi | C, Docs |
| mbox_is_empty | Add `mbox_is_empty()` API function | C |
| bsearch commands | Convert `mutt_command_get()` to use `bsearch()` | C |

---

## Medium Tasks

*Require understanding of multiple subsystems. May take several days.*

### Features

| Task | Description | Complexity |
|------|-------------|------------|
| Index color patterns | Add more optional patterns for index coloring | Medium |
| Query append | Change query title to show "X + Y + Z" | Medium |
| Alias preview | Add pager/preview for alias details | Medium |

### Refactoring

| Task | Description | Complexity |
|------|-------------|------------|
| History → ARRAY | Convert history search to use ARRAY | Medium |
| Eliminate libmutt config | Remove config dependencies from libmutt | Medium |
| STAILQ conversions | Convert smimekey/pgpkey to STAILQ | Medium |
| mbox_path_probe | Refactor (does fgetc, then fread) | Medium |

### Infrastructure

| Task | Description | Complexity |
|------|-------------|------------|
| Log version on startup | Log `neomutt -v` output to debug file | Medium |
| Usage autowrap | Wrap `usage()` output at `$COLUMNS` | Medium |
| OUT-params audit | Find all OUT-params and ensure variables are set | Medium |

### Documentation

| Task | Description | Complexity |
|------|-------------|------------|
| Expando docs | Review and document expando long-text | Medium |
| %{name} docs | Document %{name} expandos | Medium |

---

## Hard Tasks

*Significant architectural work. May take weeks. Requires deep understanding.*

### Architecture Changes

| Task | Description | Impact |
|------|-------------|--------|
| Panel default colors | Add separate default colors for each panel | High |
| ci_send_message cleanup | Split/remove backend-specific code | High |
| Account-specific config | Full implementation of account scoping | Critical |
| Browser rewrite | Complete rewrite of file browser | High |

### Major Features

| Task | Description | Impact |
|------|-------------|--------|
| New help system | Implement `$help_format_string` | High |
| Account-aware sidebar | Make sidebar understand account hierarchy | High |
| HCache → maildir | Integrate header/body cache with maildir | Critical |
| Offline support | Inline hcache for offline operation | Critical |

### Refactoring

| Task | Description | Impact |
|------|-------------|--------|
| NNTP auto-subscribe | Refactor like IMAP auto-subscription | Medium |
| Config operators | Implement `+=`, `-=` for config API | Medium |
| M->path refactor | Change from array to pointer | Medium |
| Eliminate AllMailboxes | Remove global list (depends on sidebar) | High |

### Code Structure

| Task | Description | Impact |
|------|-------------|--------|
| mutt_wstr_trunc | Rewrite for legibility (rename vars first) | Medium |
| Switch → table | Convert large switches to `[opcode, fn()]` | High |
| Keymap timeouts | Add timeout structs + callbacks | High |

---

## Mentoring Program

### How Mentoring Works

The NeoMutt project welcomes new contributors through a mentoring program:

1. **Find a task** - Pick something from Easy or Trivial
2. **Announce interest** - Comment on the issue or email the list
3. **Get guidance** - A mentor will help you get started
4. **Submit PR** - Make your changes and submit for review
5. **Iterate** - Address feedback until merged

### Task Tracking

Tasks are marked with their status:
- **New** - Available for someone to take
- **Interested** - Someone has expressed interest
- **In Progress** - Actively being worked on
- **Needs Help** - Contributor is stuck
- **Done** - Completed and merged

### Communication Channels

- **Mailing list**: neomutt-devel@neomutt.org
- **IRC**: #neomutt on irc.libera.chat
- **GitHub**: Issues and Pull Requests
- **Email**: Direct mentor contact

### Recognition

Contributors are recognized through:
- Credits in release notes
- Listing in AUTHORS file
- Shout-outs on mailing list
- `neomutt -vv` contributor list

---

## Task Creation Guidelines

When creating new tasks:

### For Easy Tasks
- Should be completable by someone unfamiliar with codebase
- Include specific file/function locations
- Link to related documentation
- Estimate: 1-4 hours

### For Medium Tasks
- Requires understanding of 2-3 subsystems
- Provide context on why the change is needed
- List dependencies on other work
- Estimate: 1-5 days

### For Hard Tasks
- Include design discussion or RFC
- Break into smaller milestones if possible
- Identify potential blockers
- Estimate: 1-4 weeks

### Extra Credit

Many tasks have "extra credit" extensions:
- Find more examples of the same issue
- Create tests for the fix
- Update documentation
- Help another contributor with a related task
