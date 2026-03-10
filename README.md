# WASMA
- [WASMA](https://github.com/joernih/WASMA)
- [REPL](https://webr.r-wasm.org/latest/)
  - webr::install("dplyr", repos = "https://joernih.github.io/WASMA/")
  - webr::install("WASMP", repos = "https://joernih.github.io/WASMA/")
- [Shiny](https://shinylive.io/r/examples/)



[WASMAP](https://r-wasm.github.io/rwasm/)







.github/workflows/deploy.yml


The error occurs because **one of your GitHub Actions dependencies internally uses the deprecated `actions/upload-artifact@v3`**. Your workflow downloads `actions/upload-pages-artifact@v2`, which still calls v3 under the hood.

## Quick Fix
Update these actions to their latest versions in `.github/workflows/wasm-build.yml`:

```yaml
    - name: Upload WASM repo  # Replace upload-artifact@v4 if present
      uses: actions/upload-artifact@v4  # Already correct
    
    - name: Deploy to GitHub Pages  # CRITICAL: Update this
      uses: peaceiris/actions-gh-pages@v4  # Keep this
    
    # But the real culprit is likely:
    - uses: actions/upload-pages-artifact@v3  # <- CHANGE TO v3 OR v4
```

**Most likely fix**: Change any `actions/upload-pages-artifact@v2` → `@v3` (or `@v4` if available).

## Check Your Workflow
Your log shows these being downloaded:
```
actions/upload-pages-artifact@v2  # <- This uses v3 internally
actions/deploy-pages@v2
```

**Replace with**:
```yaml
- name: Upload pages artifact
  uses: actions/upload-pages-artifact@v3  # Updated
  
- name: Deploy to GitHub Pages  
  uses: actions/deploy-pages@v4  # Latest
```

## Verify & Test
1. `git grep -n "upload.*artifact\|pages.*artifact" .github/workflows/` to find all instances
2. Update **all** to `@v3` or `@v4` 
3. Commit/push → workflow should pass

This is a common issue since Jan 30, 2025 deprecation—many actions need pinning to non-v3 versions. [github](https://github.com/orgs/community/discussions/152695)




