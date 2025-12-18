# Workshop Presentation Guide

## 📊 Using the Presentation

This presentation is created with **Marp** (Markdown Presentation Ecosystem).

## 🚀 Viewing Options

### Option 1: VS Code Extension (Recommended)

1. Install Marp extension in VS Code:
   - Open Extensions (Cmd+Shift+X)
   - Search for "Marp for VS Code"
   - Install by `marp-team.marp-vscode`

2. Open `PRESENTATION.md`
3. Click the preview icon (top right) or press `Cmd+K V`
4. Present with `Cmd+Shift+V`

### Option 2: Export to HTML/PDF

Using VS Code Marp extension:
- Open `PRESENTATION.md`
- Open Command Palette (Cmd+Shift+P)
- Type: "Marp: Export Slide Deck"
- Choose format: HTML, PDF, or PPTX

Using CLI:
```bash
# Install Marp CLI
npm install -g @marp-team/marp-cli

# Export to HTML
marp PRESENTATION.md -o presentation.html

# Export to PDF
marp PRESENTATION.md -o presentation.pdf

# Export to PPTX
marp PRESENTATION.md -o presentation.pptx
```

### Option 3: GitHub Pages

1. Export to HTML:
   ```bash
   marp PRESENTATION.md -o docs/index.html
   ```

2. Push to GitHub
3. Enable GitHub Pages in repository settings
4. Access at: `https://[username].github.io/[repo-name]/`

### Option 4: View Raw Markdown

Simply open `PRESENTATION.md` in any Markdown viewer - it's readable as-is!

## 📁 Presentation Structure

- **Total Slides**: ~40 slides
- **Duration**: ~45-60 minutes
- **Sections**:
  1. Introduction (5 slides)
  2. Project Overview (4 slides)
  3. Environment Setup (3 slides)
  4. Phase 1-6 Deep Dive (12 slides)
  5. Best Practices (5 slides)
  6. Getting Started (8 slides)
  7. Additional Resources (3 slides)

## 🎨 Customization

### Theme
Current theme: `default`

Available themes:
- `default`
- `gaia`
- `uncover`

Change in frontmatter:
```yaml
---
theme: gaia
---
```

### Colors
Modify in frontmatter:
```yaml
backgroundColor: #fff
```

### Header/Footer
```yaml
header: 'Your Header'
footer: 'Your Footer'
```

## 🔧 Presenter Mode

In Marp VS Code extension:
- Press `P` during presentation for presenter notes
- Use arrow keys to navigate
- Press `F` for fullscreen

## 📱 Mobile/Tablet Viewing

HTML export is responsive and works on mobile devices.

## 🎯 Tips for Presenting

1. **Practice**: Go through slides before workshop
2. **Timing**: ~1-2 minutes per slide average
3. **Interaction**: Pause for questions after each phase
4. **Hands-on**: Switch to Jenkins UI for live demos
5. **Pacing**: Adjust based on audience experience level

## 🔄 Updating the Presentation

Edit `PRESENTATION.md` directly - it's just Markdown!

### Slide Separators
```markdown
---
```

### Title Slides
```markdown
<!-- _class: lead -->
# Your Title
```

### Speaker Notes
```markdown
<!-- This is a speaker note, won't appear on slide -->
```

## 📤 Sharing

### For Workshop Participants
- Commit to GitHub
- Share HTML export link
- Print as PDF handouts

### Interactive Version
Host on GitHub Pages or any web server.

## ✅ Pre-Workshop Checklist

- [ ] Test presentation in VS Code
- [ ] Export to PDF backup
- [ ] Verify all links work
- [ ] Test on presentation screen
- [ ] Prepare demo environment
- [ ] Have Jenkins running
- [ ] Test all phases beforehand

## 🆘 Troubleshooting

### Marp Extension Not Working
1. Check extension is installed
2. Reload VS Code window
3. Check file extension is `.md`

### Export Fails
1. Ensure Marp CLI is installed
2. Check write permissions
3. Try different export format

### Styling Issues
1. Verify frontmatter syntax
2. Check theme name spelling
3. Try default theme first

## 📖 Additional Resources

- [Marp Official Docs](https://marpit.marp.app/)
- [Marp CLI](https://github.com/marp-team/marp-cli)
- [Marp VS Code Extension](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode)
- [Markdown Guide](https://www.markdownguide.org/)

---

**Ready to present?** Open `PRESENTATION.md` in VS Code with Marp extension! 🎉
