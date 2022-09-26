---
notes: |
  essentailly we need to add dependencies to our bottled ember app. I’m going to add them to a new script here on the project. `start:docs` 

  we need to do it this way for these dependencies because we don’t want every dev dependency of our addon to bleed into the bottled ember app (this is how we did it in classic addons) and we can’t add these particular dependencies to the `dependencies` block on the package.json because they are not relevant to consumers of our addon. 

  we also want to add `-—no-overlay` to the command line so it doesn’t include the test or the application temlate we added a second ago. and we’re going to add this last thing, link docs. This will allow us to write our markdown in the docs folder and for it to be in the right place in the bottled ember app. We’re also adding an extra cache name so that it doesn’t blow away our other bottled ember app that we’re using for the test app. 

  and lastly we add -p 0 to allow it to pick a random port and we can run our docs at the same time as the test app
---

# Field Guide


```json
"scripts": {
  "start:docs": "bottled-ember serve 
    --output-path=bottled-docs 
    --deps=field-guide,field-guide-default-template,ember-cli-fastboot,prember 
    --links=docs 
    --cache-name=docs 
    -—no-overlay 
    -p 0"
}
```
