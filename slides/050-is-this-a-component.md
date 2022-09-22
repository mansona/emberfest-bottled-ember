---
notes: |
  There are all a bunch of ember paper cmponents being used together, and actually there are a few helpers and modifiers provided by other addons. the issue we have here is that both people and cmoputers can’t really tell where these components came from. We can kinda teach the computers by getting people to install ember spcecific plugins into their editors but a human either has to guess what addon is providing a specific component or the community has come up with some tricks to help in the search, like all of the ember paper components are prefixided with “Paper” so you can kinda guess where things are coming from.

  on the build time side of things, because of the way that a classic addon can pretty much hook into any part of the ember build process it’s not hard to imagine a situation where a rogue addon can be adding a significant build penalty. I know I’ve writtend addons that accidentially make thousands of requests to the localhost server while you’re developing effectively ddosing yourself. 
---

# So many components 🙈

![all components](/all-components.png)
