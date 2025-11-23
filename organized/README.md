# NeoMutt Brain Dump - Organized Ideas

This folder contains comprehensive, categorized summaries of all development ideas, architectural designs, feature proposals, and technical notes from the NeoMutt brain dump repository. These documents distill years of development thinking into accessible, well-organized reference material.

## How to Use This Documentation

Each category file provides:
- **Context and background** for why certain decisions were made
- **Technical details** with code examples and diagrams where relevant
- **Implementation notes** for developers working on these features
- **Cross-references** to related source files in the `brain-dump/` folder

## Categories

### Core Architecture
| Document | Description |
|----------|-------------|
| [architecture.md](architecture.md) | The foundational system design of NeoMutt, including the Model-View-Controller pattern, account/mailbox/email hierarchy, window management, event handling, and notification systems. Essential reading for understanding how NeoMutt is structured. |

### Development
| Document | Description |
|----------|-------------|
| [features.md](features.md) | Comprehensive collection of feature requests, UI/UX enhancement proposals, and user-facing improvements. Covers summary pages, dialog systems, color enhancements, mouse support, and compose improvements. |
| [code-quality.md](code-quality.md) | Technical debt, refactoring opportunities, and code improvement tasks. Organized from trivial fixes to major architectural changes, with detailed guidance on implementation approaches. |
| [task-difficulty.md](task-difficulty.md) | Tasks organized by difficulty level (easy, medium, hard) for contributor onboarding. Includes mentoring notes and guidelines for new developers. |

### Technical Reference
| Document | Description |
|----------|-------------|
| [apis-libraries.md](apis-libraries.md) | API specifications for the mailbox API (mxapi), completion system, menu framework, path handling, and core libraries. Includes design principles and best practices. |
| [email-protocols.md](email-protocols.md) | Backend implementations for IMAP, NNTP, Notmuch, POP, and Maildir. Covers connection handling, caching strategies, and protocol-specific features. |
| [scripting.md](scripting.md) | Lua scripting integration for testing, configuration, email processing, and UI customization. Includes API documentation and example use cases. |

### Infrastructure
| Document | Description |
|----------|-------------|
| [developer-tools.md](developer-tools.md) | Build system configuration, CI/CD pipelines, debugging tools, and development workflow. Covers testing infrastructure and release processes. |
| [documentation.md](documentation.md) | Documentation standards, translation systems, community engagement, and project management. Includes guidelines for contributors and maintainers. |

## Source Material

These summaries are synthesized from over 120 files in the `brain-dump/` folder, including:

- **Architecture notes**: `mvc.txt`, `account.txt`, `config.txt`, `windows.txt`, `notifications.txt`
- **Feature proposals**: `compose-preview.md`, `reflow.md`, `notify.md`, `dialog.txt`
- **Task lists**: `easy.txt`, `medium.txt`, `hard.txt`, `trivial.txt`
- **UI/UX designs**: `dialog.txt`, `panel/*.txt`, `sidebar/`, `pager/`
- **API specifications**: `api/*.txt`
- **Scripting**: `lua/*.txt`
- **Notebooks**: `notebook.1/`, `notebook.2/` (chronological development notes)

## Contributing

When working with this documentation:

1. **For new ideas**: Add them to the appropriate category file, or create a new category if needed
2. **For implementation**: Cross-reference the original source files in `brain-dump/` for full context
3. **For updates**: Keep summaries in sync with any changes to the source material
