---
notes: |
  The default blueprint has only two packages, the subfolder that matches the name of the surrounding folder is your actual addon. and if you look in there you can see that it matches what I showed you a few seconds ago. this is the bit that gets compiled and pushed to npm when you publish. this other folder named “test app” is… well… a test app “joy” 

  for anyone that is familar with classic addons you mght think of this as your dummy app, just a whole lot more standard. there are a whole bunch of hoops that ember-cli needs to jump through to get addons to work properly with their dummy apps and most people don’t care about this, unti lsomething breaks or you spend 12 months building a tool that tries to hook into this process 

  when you run npm start in this setup it will start two processes, one to build your addon in the background and another to run your test app. once both of these processes complete you should have a very standard ember app running that depends on your addon. and it’s on our trusty port 4200 so things can feel very familliar.

---

# The structure

![enhance](/monorepo-visual.png)
