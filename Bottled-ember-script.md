	Hey folks
	
I feel like I know everyone here in the room but I’ll still introduce myself real quick.

My name is Chris Manson, I have 2 dogs, 2 kids, and somehow find time to keep doing open source work. One of they ways that I find that time is that I work for an awesome company that gives me 20% to work on any community projects… Mainmatter. If that’s a surprise for anyone that knows me, no I haven’t changed jobs recently, instead it’s my job that’s changed names. What used to be called simplabs is now called Mainmatter and I hopefully have updated all my socials to reflect that but let me know if you spot anything that I haven’t.

Speeking of socials, you can tweet me at real_ate at any time about what I’m talking about today. I’m pretty passionate about this stuff so I’ll likely respond! Now that we have introductions out of the way, let’s see what I’m actually talking about today. I have a little side project that I mentioned in my last EmberFest talk last year. Look at me there, I look so young and well rested 😭 but last year I had the idea that it might be useful to have whole ember app as a single dependency so that an Empress repo could just have markdown in it and essentially that single dependency, rather than being “surrounded” by a host Ember app with all random files in it.

You’ll be glad to hear that I continued that experement and had it finished months ago and release to the world to use for ages 

<screenshot of tweet>

hold on what’s the timestamp on that tweet? zoom in. enhance. ah ok that was just 6 days ago! whoops! I’ll be telling some of the story why this was so difficult today but a lot of it will need to wait for the closing dinner. I need a drink or two to talk about some of the things that I’ve seen.

Ok so I can imagine that some of you will be thinking, “sure this is great for Empress, but I don’t use empress so why does this apply to me”. Well This little bottled ember trick can be quite useful to test your V2 addons 

but what is a V2 addon I hear you ask… I guess I’m going to have to bit of an intro to that before I can show you why bottled-ember is so awesome… but to answer that question I need to take you back to a time long ago and tell you a bit more about the “classic addon” structure

<gif of the intro to aladin> 

I was a bit torn on how deep of an expanation I should go into “what is an Ember addon”. In reality everyone here or anyone watching the recording has probably uses ember addons regularly, and we probably have quite a few addon authors in the room too. but for the sake of completeness I’ll go a little bit into the basics.

An ember addon is like a special npm package that automatically integrates with your ember app. It can either provide some stuff that your app can make use of, like some components in the case of ember paper or any of the other component libraris out there

slide with a few different libs

or it could provide you with some helpers like the incredibly popular ember-truth-thelpers addon. bacically they can provide you anything that you could write in your own app.

then there is another class of addons that can *do stuff*. That may seem like a wide category, but it is currently as wide as you can imagine. Since I am a big JAM stack guy one of the “do stuff” addons that I use regularly is prember. This runs at the end of your build and builds a static copy of your pages (at least the ones that you have told it about). As you can imagine a category as wide as “do stuff” can be very very large, but you can take a look at the “build system” section on Ember Observer if you want to see some examples

There are even some addons that can provideo you with actions or generators that hook into the ember command itself. When you run `ember serve` or `ember build` those are built in commands, but you can have a whole host of commands that are provided by addons. This ability of ember-addons is not very widely used, but one of the more famous ones is ember-cli deploy that provides the new `ember deploy` command. 

Each addon can also provide a bunch of generators that may be sepecific to their use cased. there is a default generator that can run when the addon is first installed `with ember install` and then you can provide either specific new generators, like empress-blog providing a generator for posts, authors, or tags so files are setup in the right way. or you could have a specific generator that overrides one of the default ones like maybe your addon changes what a “component” should look like, so you provide a replacement.


All of these addon features have something in common. they are all magical in the way that they work, and depending on what camp you’re in that is either a good thing or a bad thing. I’m personally a massive fan of all this magic but I’m aware and I agree with a lot of the critisisms that can be landed at them. The two big ones I always see people talking about are “provider discoverability” and “build time”. For discoverability let’s take this example template 

template using Ember paper components

There are all a bunch of ember paper cmponents being used together, and actually there are a few helpers and modifiers provided by other addons. the issue we have here is that both people and cmoputers can’t really tell where these components came from. We can kinda teach the computers by getting people to install ember spcecific plugins into their editors but a human either has to guess what addon is providing a specific component or the community has come up with some tricks to help in the search, like all of the ember paper components are prefixided with “Paper” so you can kinda guess where things are coming from.

on the build time side of things, because of the way that a classic addon can pretty much hook into any part of the ember build process it’s not hard to imagine a situation where a rogue addon can be adding a significant build penalty. I know I’ve writtend addons that accidentially make thousands of requests to the localhost server while you’re developing effectively ddosing yourself. 

so what’s the solution to these two problems? well again it’s Ed falkner to the rescue! by this stage you all must have heard about embroider. I don’t know all that much about it but the highest level summary is that it’s a next generation build system for ember apps that makes things a little bit less “magic”. part of the problems that we’ve had in ember is that we’ve been doing our own thing our own way that doesn’t fit in with the rest of the community’s tools. Embroider is an attempt to change that by better integrating with the wider community. one of the biggest things that are affected by this new ethos is the humble addon. The V2 addon spec dramatically simplifies the complexity of an ember-addon by defining a standard pre-built output that an addon needs to be published to npm as. that means that if all your provider addons are converted to this V2 format you just don’t need to buld them any more as part of your build step! 

There is one problem hidden in what I’ve just said though. if you addon is in the “do stuff” category then you cant have it automatically hook into any build process any more. You can tell the developer consuming your addon how to get it to hook in, but it can’t do this automatically any more. I’ve had a few discussion with Ed about this and there is a potential way forward. Hopefully I’m back here next year telling you about “build addons”, not least because my whole Empress project desperately needs them!

Anyway let’s get back to what a V2 addon can do, and stop talking about what it can’t.


The structure of the addon is also much more standard so when you’re importing something from an addon that’s also in the format that is standard for npm so other tools can just figure it out! all very good, but there is another catch. let’s take a look at the v2 addon’s structure as npm would see it:

there is nothing in here! 

Let’s also take a look at the source that this was generated from. Ok this is a bit better, tehre are at least some files in here. but there are a lot fewer of the nicities that we have come to expect from ember addons, there is no test dummy app that you can just run to have a demo of your app. and there are no tests that you can use that dummy app to exercise the components in your addon. 

this is why the current draft or proposed blueprint doesn’t look like this folder strcutre. I’ve just simplified it to be exactly what you need to deploy your npm package. no the currently proposed blueprint look smore like this

so that is a whole lot of stuff, let me take you on the tour to simplify it a bit. the trick to understanding this screenshot is to know that the blueprint is using workspaces. you heard me right folks, the new proposed blueprint is a monorepo. now anyone that knows me knows that I’m not a fan of monorepos, but don’t worry I’ll get to that.

let’s just look what we have here first. The default blueprint has only two packages, the subfolder that matches the name of the surrounding folder is your actual addon. and if you look in there you can see that it matches what I showed you a few seconds ago. this is the bit that gets compiled and pushed to npm when you publish. this other folder named “test app” is… well… a test app “joy” 

for anyone that is familar with classic addons you mght think of this as your dummy app, just a whole lot more standard. there are a whole bunch of hoops that ember-cli needs to jump through to get addons to work properly with their dummy apps and most people don’t care about this, unti lsomething breaks or you spend 12 months building a tool that tries to hook into this process 

when you run npm start in this setup it will start two processes, one to build your addon in the background and another to run your test app. once all this console stuff gets out of the way you should have a very standard ember app running that depends on your addon. and it’s on our trusty port 4200 so things can feel very familliar.

> timecheck 13 mins

but personally I’m not a fan of all of this. I already mentioned that my bottled-ember idea was to make it possible to have an empress app without being surrounded by the host ember app. I figured why not do something like that for V2 addons and make it possible to have the new fancy without needing to go so far as a monorepo.

so let’s add bottled-ember together to this previous example before we converted it into a monorepo. the first thing you will need is to depend on `bottled-ember` as a dev dependency. ok now with that installed lets create a start script. Now the way bottled-ember works is that it creates a host app for you “behind the scenes”. The idea is that you don’t need to know anything about this and it should “just work”. We’ll see how far that goes over the coming months when I hope all of you try this out in your various projects.

so here we’re starting the ember app with serve and that’s all we need to do. When you run this I currently have it outputting a status of each of the steps that it’s doing. we can probably turn this off eventually. Also I have a plan to make this much quicker but we’ve already seen that it took me quite a while to get it to work this far!

and that’s all you need to do to get it working. One problem, there is a blank app when you boot it up. So we didn’t provide it with any actual test app templates or anything to do! Let’s start with the simple example, let’s just create a app template that excersises the component in this addon.

where should I create it? well let’s just try where we’re used to trying `app/templates/application.hbs` 

here is my template in that normal folder, let’s try the app again and … it’s working!!! 

The way that this works is that it overlays certain known folders over the default app folder provided by bottled ember. so `app` and `tests` is overloaded on the botteld ember app. 

that also means that we can create ourselves an acceptance test in the place that you might expect and we could run that too. It’s funny because this even feels a bit more convenient than being in a “real” ember app because you just don’t need to define any of the test boiler plate if you don’t need it to differ from what’s provided in the default template. 

so this is one option available to you if you wanted to provide your own ember app and build everyting yourself. but what about the project I showed off last year at Ember Fest? field-guide! 

it’s a pretty good project for documenting your addons. I’m hopefully going to convince anybody that’s currently using ember-cli-addon docs to swich over in the next 12 months, but we’ll see! essentailly we need to add dependencies to our bottled ember app. I’m going to add them to a new script here on the project. `start:docs` 

we need to do it this way for these dependencies because we don’t want every dev dependency of our addon to bleed into the bottled ember app (this is how we did it in classic addons) and we can’t add these particular dependencies to the `dependencies` block on the package.json because they are not relevant to consumers of our addon. 

we also want to add `—no-overlay` to the command line so it doesn’t include the test or the application temlate we added a second ago. and we’re going to add this last thing, link docs. This will allow us to write our markdown in the docs folder and for it to be in the right place in the bottled ember app. We’re also adding an extra cache name so that it doesn’t blow away our other bottled ember app that we’re using for the test app. 

and there we have it. it’s working.

if we add a port to our docs app we can even run it at the same time as our test app using the trick that was on the monorepo blueprint. 

That’s pretty much it for using bottled ember in a V2 addon. I know it’s a bit of a whirlwind tour but I wanted to cover the overview here and I’ll be writing blog posts and maybe even youtube videos about this for the next while. I really dislike monorepos and I’m going to do everything in my power to make the alternative as easy as possible!

I’ll leave you with one thing before I sign off. Let’s go back to that tweet that I put out on Saturday. 

I want to show you the conclusion of this year long quest to make empress projects feel “clean” for people.

Here is a screenshot of the file structure for my blog before saturday. 

and here it is now

As you can see that is a lot better! essentially the only files that we have left are the markdown files that represent real actual content. there is one thing about this setup I don’t like, because this is an empress host app we don’t need to worry about things being in our dependencies blok of our package json. but for some reason I couldn’t get ember-cli to pick those up as addon dependencies if I put them in the package.json but I’m sure I’ll be able to trick it eventually

And that’s it folks! I’ll be putting out a rival V2 blueprint that y’all can try out and let me know what you think and if you’d use it. if anyone is interested in how this works from a deeper perspective just let me know either in person or on twitter and I might do a blog series about it going into more detail.

see you all later and happy coding! 














