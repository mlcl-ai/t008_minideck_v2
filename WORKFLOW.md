# Workflow: Tracking Slide Edits in Git

## Current Setup

The slides can be edited in two ways:
1. **Editor** (`editor.html`) - Edit slides with live preview, auto-saves to localStorage
2. **Export** - Export localStorage to `slides.json` for git tracking

## Workflow

### 1. Edit Slides
Open `editor.html` and edit any slide content. Changes auto-save to browser localStorage.

### 2. Export to Git-Tracked File
Click the "💾 Export to slides.json" button in the editor. This downloads `slides.json` with your current edits.

### 3. Commit to Git
```bash
# Save the downloaded slides.json to the repo directory
mv ~/Downloads/slides.json .

# Commit the changes
git add slides.json
git commit -m "Update slide content: [describe changes]"
git push
```

### 4. Loading Priority
The presentation (`index.html`) loads content in this order:
1. **slides.json** (if exists) - Git-tracked version
2. **localStorage** - Browser-specific edits
3. **Default** - Original hardcoded content

## Benefits

- ✅ Version control: Every change is tracked in git
- ✅ Collaboration: Team members can see and revert changes
- ✅ History: `git log` shows all slide versions
- ✅ Branches: Test different versions in git branches
- ✅ Rollback: `git checkout <commit> -- slides.json` to restore old versions

## Example Git History

```bash
git log --oneline slides.json
# 3a2b1c Update slide 7: Add clinical pilot details
# 2f4e5d Update slide 3: Refine cambrian moment messaging  
# 1c3d2e Initial slide export
```

