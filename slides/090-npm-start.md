---
notes: |
  so here we’re starting the ember app with serve and that’s all we need to do. I've added a custom output path here though because we need to leave dist in place because that's where our addon's npm package will get compiled to
  
  When you run this I currently have it outputting a status of each of the steps that it’s doing
---

# npm start script

```json
"scripts": {
  "start:app": "bottled-ember serve --output-path=bottled-dist"
}
```
