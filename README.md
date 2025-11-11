# t008_minideck_v2

15-slide pitch deck for Gradient.

## Quick Start

- **Present**: Open `index.html` 
- **Edit**: Open `editor.html`

## Editing Workflow

1. Open `editor.html` in browser
2. Click any text to edit (auto-saves to localStorage)
3. When ready to commit: Open browser console → Run `exportSlides()`
4. Paste output into `slides.json`
5. Commit: `git add slides.json && git commit -m "Update slides" && git push`
6. Use `git log slides.json` to see history
7. Revert: `git checkout <commit-hash> slides.json`

## Files

- `index.html` - Presentation view (loads from slides.json → localStorage)
- `editor.html` - Edit mode (saves to localStorage)
- `slides.json` - Git-tracked source of truth (title + subtitle only)

## Links

- **GitHub**: https://github.com/mlcl-ai/t008_minideck_v2
- **Notion Task**: https://www.notion.so/minideck_v2-2a8cdd12fc5e819ba597e04f5f3a3169
- **Owner**: Ben Holmes

