# Documentation & Community

Documentation, translations, community engagement, and project management.

## Sources
- dyk.txt, translate.txt, jekyll.txt
- mentoring.txt, credits.txt, contributors.txt
- docs-release-1.0.md

---

## Documentation Types

### User Documentation
- Manual/guide
- Configuration reference
- Feature documentation
- FAQ

### Developer Documentation
- API documentation (Doxygen)
- Architecture docs
- Contributing guide
- Code style guide

### Community Documentation
- Getting started
- Changelog
- Release notes
- Credits

---

## "Did You Know?" Tips (dyk.txt)

### Purpose
- Brief educational content
- Feature discovery
- Tips for power users

### Content Ideas
- Keyboard shortcuts
- Hidden features
- Configuration tips
- Workflow suggestions

### Distribution
- `:tips` command
- Mbox in contrib/
- NNTP (if available)
- Links to web

---

## Translation System (translate.txt)

### Current State
- Gettext-based translations
- Multiple language support

### Improvements Needed
- Split up large documents
- Printf format ordering docs
- Translation workflow guide

### Format Ordering Example
```
msgid "Option %s has an invalid type %d"
msgstr "Le type %2$d de l'option %1$s est invalide"
```

---

## Website (jekyll.txt)

### Components
- Main site (neomutt.org)
- Code documentation (/code)
- Feature pages
- Guide/manual

### Tools
- Jekyll for static site
- Doxygen for code docs
- Sphinx themes option

### Content
- Installation instructions
- Feature documentation
- Development guides
- News/blog posts

---

## README Improvements

### GitHub-Flavored Markdown
```markdown
> [!NOTE]
> Important information

> [!TIP]
> Helpful suggestion

> [!IMPORTANT]
> Critical information

> [!WARNING]
> Potential issues
```

### Installation Badge
```markdown
[![Packaging status](https://repology.org/badge/vertical-allrepos/neomutt.svg?columns=3&exclude_unsupported=1)](https://repology.org/project/neomutt/versions)
```

---

## Community Engagement

### GitHub Discussions
- "I Need Help" category
- "I Have Ideas" category
- "I Want to Help" category

### Pinned Issues
- How to ask for help
- How to join us
- How to thank us (star, donate, IRC)

### Communication Channels
- IRC
- Mailing lists
- GitHub Discussions
- Issue tracker

---

## Contributor Recognition

### Credits System
- Contributors list
- AUTHORS.md
- `neomutt --authors`

### Statistics
- Commit counts by year
- Top contributors
- `neomutt -vv` top ten recalc

### Annual Summary
- Commits: x
- Users: y
- Releases: z
- Summary to ML + social media

---

## Sample Configs

### Modernization
- Underscore config names
- `# vim: syn=neomuttrc`
- Remove deprecated options
- Update examples

### Theme Files
- `~/.config/neomutt/theme.rc`
- Separate keybindings file
- Sample themes collection

---

## Documentation Standards

### Code Documentation
- Doxygen comments
- @extends for data structures
- Clear function descriptions
- Parameter documentation

### User Documentation
- Consistent formatting
- Cross-references
- Examples for each feature
- Troubleshooting sections

---

## Release Documentation

### docs-release-1.0.md
- Feature highlights
- Breaking changes
- Migration guide
- Known issues

### Changelog
- `:changelog` command
- Read from file
- Version history

---

## Help System

### Current Help
- Built-in help pages
- Manual access
- Context-sensitive help (F1)

### Planned Improvements
- Tabbed help pages
- Online web help option
- Search functionality
- Better navigation

---

## Learning Resources

### Primer HOWTO
- How to compose and send email
- How to browse folders
- How to search in folders
- Basic operations guide

### Target Audience
- Read incoming email
- Mark read/unread
- Move to folders (or tag with notmuch)
- List/index directories
- Compose new message

---

## Issue Management

### Issue Templates
- Bug report
- Feature request
- Enhancement redirect to Discussions

### Labels
- Difficulty levels
- Component areas
- Status tracking

### Triage Process
- Initial review
- Categorization
- Assignment
- Follow-up

---

## Repository Management

### Organization
- Archive/delete old repos
- neomutt-old organization option
- Hide repos from Google indexing

### GitHub Settings
- .github README (welcome)
- Issue templates
- Discussion categories

---

## Config Documentation

### URL Linkification
- URLs in docs should be clickable
- Reference links to guide

### Expando Documentation
- Long-text review needed
- %{name} expandos docs
- Architecture documentation

---

## Awesome Lists

### Goal
- Get NeoMutt on every available list
- Track listings on neomutt.org

### Example Lists
- Best console email clients
- Terminal applications
- Productivity tools

---

## Community Guidelines

### AI Usage in Contributions
- Create label for PRs
- Credit AI usage
- Suggested: "Assisted-by: <AI tool name>"

### Code of Conduct
- Welcoming environment
- Constructive feedback
- Inclusive community
