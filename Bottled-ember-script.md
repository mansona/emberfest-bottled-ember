

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














