# 📊 Viewing the Workshop Presentation

## 🌐 Online Viewing (GitHub Pages)

**Live Presentation**: https://epam-msdp.github.io/CICD-workshop-day1/

The presentation is automatically deployed to GitHub Pages when `PRESENTATION.md` is updated.

### How it works:
1. Edit `PRESENTATION.md`
2. Push to `presentation` or `main` branch
3. GitHub Actions automatically converts to HTML
4. Deployed to GitHub Pages
5. View in browser with full slide functionality

## 🎯 Quick Links

- **Presentation**: [View Slides](https://epam-msdp.github.io/CICD-workshop-day1/)
- **Source**: [PRESENTATION.md](PRESENTATION.md)
- **Setup Guide**: [PRESENTATION-README.md](PRESENTATION-README.md)

## 🛠️ Local Development

### VS Code with Marp Extension
1. Install "Marp for VS Code" extension
2. Open `PRESENTATION.md`
3. Press `Cmd+K V` for preview
4. Press `Cmd+Shift+V` for presentation mode

### Export to PDF
```bash
npm install -g @marp-team/marp-cli
marp PRESENTATION.md -o presentation.pdf
```

## 📱 Navigation (Online Presentation)

- **Next Slide**: Arrow Right, Space, Click
- **Previous Slide**: Arrow Left
- **First Slide**: Home
- **Last Slide**: End
- **Fullscreen**: F
- **Overview**: O (if supported)

## 🔄 Updating the Presentation

1. Edit `PRESENTATION.md`
2. Commit and push:
   ```bash
   git add PRESENTATION.md
   git commit -m "Update presentation"
   git push
   ```
3. Wait 1-2 minutes for GitHub Actions
4. Refresh the GitHub Pages URL

## 🎨 Customization

Edit frontmatter in `PRESENTATION.md`:

```yaml
---
marp: true
theme: default  # or gaia, uncover
backgroundColor: #fff
---
```

## 📖 Full Documentation

See [PRESENTATION-README.md](PRESENTATION-README.md) for complete guide.

---

**🚀 Start Presenting**: [Open Slides](https://epam-msdp.github.io/CICD-workshop-day1/)
