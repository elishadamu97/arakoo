---
layout: default
---

# **Building an incredible UX as a one-person startup**

**`Saturday, May 28, 2022`**

<hr>

[Assetbots](https://assetbots.com/) was one of the first early Replicache customers. Their CEO, [Chad Burggraf](https://twitter.com/chadburggraf), graciously agreed to do a Q&A with us their use of Replicache, and how it has helped them.

Below is the edited interview, with <code>Replicache (Aaron) in purple</code> and **Assetbots (Chad) in black**.

## Hey Chad! Thanks for agreeing to talk with us. Can you tell us what Assetbots does?

Hi Aaron! Assetbots is a tool that helps businesses keep track of their valuable equipment — think computers, tools, or vehicles. You can maintain records of everything you have, and what happens to it over time. For example, who it’s assigned to, when it’s repaired, and how it depreciates. Kind of like a CMS but for stuff instead of people.

## Do you have a demo we can share?

Yep!

<iframe width="708" height="423" src="https://www.youtube.com/embed/u8DhCtmshsE" title="Assetbots Demo #1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## <span style="color:rebeccapurple">What’s the key benefit of Replicache for Assetbots?</span>

**I’m able to deliver an incredible user experience as a one-person operation**. Here’s a quote that I just received yesterday that wouldn’t have been possible without Replicache: *“This looks genuinely amazing … like a mix of Notion and Docusaurus.”*

### <span style="color:rebeccapurple">Heh. This is feedback we have gotten repeatedly and was somewhat unexpected. We thought of Replicache initially as "Serious Engineering". But we have many small customers, just 1-3 person shops, using Replicache and delighted with the quality of experience they can deliver with little effort.</span>

Right. During demos, my customers love the offline capabilities. But when users are driving the product themselves, they fall in love with the way it feels. Nothing in my space comes close to matching the feel and speed Assetbots offers. **There is no way I could have made as good of a product by myself without Replicache.**

### <span style="color:rebeccapurple">How did you find Replicache? What made you decide to try it?</span>

I was thinking about working offline because I noticed the lack of offline support was a recurring complaint about competitor products. I’ve never built an offline-capable web application, but I had experience doing it for mobile and remembered how difficult it was, even for a medium-sized and capable development team.


My first pass used [Lovefield](https://google.github.io/lovefield/), which is a SQL abstraction layer on top of IndexedDB. This worked okay for my offline use-case, but I never really fell in love with the architecture that started to surface in my app.


I stumbled on Replicache, and realized that I should be getting real-time updates (a.k.a., multiplayer) for free if I’m going to build for offline anyway. That’s because once you commit to an offline architecture, you’re going to be merging changes in one way or another, and distributing those changes among clients is the easy part. The Replicache design fit my mental model, so I went with it.

### <span style="color:rebeccapurple">Can you describe a bit how Assetbots is built? How does it make use of Replicache? Why did you decide to build Assetbots this way?</span>

Assetbots is mostly a Single Page Application using React on the client and .NET 6 on the server. Like a lot of “spreadsheet” SaaS applications, it’s basically a nice UI on a set of lists.


On the client, Replicache acts as my source of truth. In other words, it replaces Redux. I have a React context provider that sits near the root of my app tree, and from that you can get an object that gives you access to the global data store. It ends up feeling like a cross between React Query and Redux, for those familiar with React apps. That’s because Replicache is async, so data access and manipulation needs to be turned into reactive state rather than done directly.


An interesting side effect of accessing all your data async is that you can do things that would be hard in a traditional Redux context. For example, I provide full text search on most of our data. Moving full text indexing and the Replicache data store into a Web Worker doesn’t change the API surface for my components at all, which is cool.

 **<span style="color:rebeccapurple">Doing full text search client-side was a very cool hack, and another unexpected surprise from Assetbots. We are working on making this easier, but it's amazing you were able to do it without any first-class support from us.</span>**


Yeah, the fact that Replicache works in Workers is key here. I use the [Fuse.js](https://fusejs.io/) library and store its indexes in IndexedDB (separate from Replicache), but use APIs from Replicache to keep the text index in sync with primary data.


On the server, Assetbots is a standard .NET/SQLServer app. For real-time updates, I’m using SignalR, which is the standard in the .NET world. Assetbots is built this way because my stack already existed from a previous endeavor, and I just kept building Assetbots on top of it. Replicache required a relatively tiny amount of architectural work for the benefits it brought.

 **<span style="color:rebeccapurple">It was really nice to see how easily you integrated Replicache with a .NET/SQLServer backend, since this is not at all something we tested with.</span>**


 **<span style="color:rebeccapurple">Since you started, we got a lot of requests for a server "starter pack" to make it easier to try Replicache. Our new [Todo Quickstart](https://doc.replicache.dev/) includes a generic Replicache server written in TypeScript/Next.js/Postgres, and many customers just use that.</span>**


 **<span style="color:rebeccapurple">I'm curious if you would ever move to something opinionated/built for Replicache on the backend, or whether you're happy with what you've got.</span>**

Yes. Not for Assetbots specifically, but definitely for a greenfield product. Of course, I'd have to explore the details, but assuming there was some sort of Cloudflare Worker / AWS Lambda type functionality included then it would be a no-brainer.

### **<span style="color:rebeccapurple">How have you found the experience of building with Replicache so far? Tell us the good 😊 and the bad 😬.</span>**

It’s been great! I’m typically very conservative with my technology choices. I tend to wait for the “winner” to emerge in a space before wading in. I remember going out for oysters with my wife and talking to her about this technology I found that might enable me to build an experience that is typically only delivered by best-in-class apps with tons of resources. We talked about the risks but quickly realized that my excitement and enthusiasm were going to make the decision for us.

Of course, there are some growing pains. I’ve had to run a fork periodically to work around browser issues (thanks, Safari). Async is not a first-class citizen in React right now, so there is more boilerplate around error handling and loading states than there will be when Suspense for data fetching is standardized.

### **<span style="color:rebeccapurple">What types of projects do you feel are good matches for Replicache? What types are not?</span>**

I can picture ways to make most of the projects I've built in my career work with Replicache. But the best matches are projects where interactivity is important, and where the data set can be easily partitioned. So, collaborative editing, games, etc.


Assetbots is a traditional SaaS with a very relational data model. This kind of app requires a little more thought but is clearly do-able, as long as the dataset can be partitioned in such a way that it can be synced to the client in reasonable <code>(<100MB)</code> chunks.

I think I would struggle with how to implement Replicache with very large data sets (for example, if Assetbots had a customer that wanted to track 1M assets), or graph-like data models for e.g., a social network. Of course, you could solve those problems, but the benefits might not be worth it. One of the key benefits of Replicache is having all your data available on the client. This alone unlocks so much that developers should think about architecting their data model to make it feasible.

### **<span style="color:rebeccapurple">Thank you very much for your time, Chad, and for these kind words. Any closing questions or thoughts?</span>**

Thanks for building a great tool! As with any tech I rely on, my main concerns moving forward revolve around roadmap, stability and pricing. So as much clarity as often as possible in those areas would be great.


 **<span style="color:rebeccapurple">Totally fair. As of a few weeks ago, Replicache is now in [General Availability](https://replicache.dev/blog/replicache-general-availability) which means we no longer consider it beta software and stand behind its quality and stability.</span>**


 **<span style="color:rebeccapurple">We have also published [final pricing](https://replicache.dev/#pricing) and licensing details. We do not yet have a public roadmap but will work on that.</span>**


**<span style="color:rebeccapurple">Thanks again. It has been super fun working with you on Assetbots, and we're very proud to be helping you deliver such a great UX.</span>**