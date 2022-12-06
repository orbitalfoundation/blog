---
layout: post
title:  Why XR?
category: Philosophy
tags: [xr]
image: city-1.jpg
date: 2022 Dec 1st 2021
---

# The complexity of app development

Applications as we know them today are ferried to us along a "pipeline" where each part of the pipeline is a transformation of the application. An application starts with a concept, an idea, a glint in a designers eye - and ends up with a running piece of code delivered to our compute platform - either as an interactive experience or as an independent agent doing work on our behalf.
The pipeline as it is phrased today is needlessly heavy. There is no reason why everybody and anybody shouldn't be able to create apps on demand, on a moments notice, and share them with friends. The no-code revolution is a part of this, but also there's a more fundamental restructuring that is needed for applications, components and runtime containers.

# Fetching apps

We all typically download apps within the context of a desktop operating system such as Windows, or on our phones using iOS, or even on the web (by visiting sites with rich applications such as Figma, Miro or Makepad).

All of these ecosystems are typified by concepts such as self-contained apps consisting of pre-compiled binaries that are fetched via monolithic downloads. Apps are often fetched from app stores which define security and safety policies on behalf of the user.
The "mindset" of an app is that it is a "heavy" thing that is fetched "infrequently". To some degree the web does hide this from consumers - but for the web app developer this is very much true. Even web apps go through a heavy (not instantaneous) compilation stage to become packaged into a final deliverable that is then shipped to a client.

# App Data Silos

Apps today connect to private data silos where they fetch, manipulate and interact with your data. These silos are almost always closed data, with the user interface coercively dominating, and the data often if not always hidden behind the app user interface.

Consumer apps today almost never ever operate over a shared data model. File-systems have become somewhat vestigial from the original design intent - they have become a kind of mind-palace to reference data objects that will be viewed by one specific app.

There are many good reasons for this aside from just moat building around a business concern. It is hard to keep data safe, and it is hard to allow third party software agents into a corporate sandbox - everybody uses different languages, databases, ways of organizing their data, optimizing access to data and so on. To solve this problem, to allow applications to visit each others data more easily, there are two serious challenges - first is to create a computational sandbox where other peoples code can even run, and more seriously even, formally defining app permissions in such a way that apps don't crush local memory resources, or cpu resources or access resources they are not supposed to.

We do see with Google Firebase some attempt to formalize data access but agents running inside a cloud compute container effectively have administrative level access to databases in that container. We can also see Cloudflare workers and WASM and WASI as attempts to formalize security models here to allow remote untrusted execution to be run locally safely.

# Loading apps

Once an app is fetched from a remote server then typically it is "run". It is loaded into memory, any late binding libraries or resources are resolved, and execution is passed to an entry point.
Applications universally run within some kind of application container, be it merely a Google Chrome Tab running Javascript or WASM, an open-source UNIX based thread-manager and OS such as Ubuntu running native assembler code or a proprietary operating system such as iOS or MacOS running code both in assembler and in emulation. All of these application containers provide absolutely critical core services and built-in libraries such as networking, file-system access and device access such as input controls.

Application containers all also enforce security over apps to varying degrees. Third party apps are prevented from accessing parts of the file-system they do not own (such as /home/user/bitcoin_private_keys) and from saturating all of the disk space, the CPU or memory.
There's really nothing wrong with any of the above - the problem here isn't generally the fetching and running of apps per-se but rather that this is all fragmented.
Theres a real opportunity here to more often use a late binding mechanism for loading and referencing third party libraries, modules and services. A lot of apps are defined by makefiles or cargo crates that are all effectively manifests defining resources that could be bound later but instead produce monolithic precompiled blobs.

Many app ecosystems do of course allow dynamic link libraries, but the argument I'm making is that a late binding paradigm should be the default - not an exception. All applications should be primarily just glue code with the bulk of the app itself being declared by reference in a manifests that declare which libraries to load.

Another issue, more specific to the web, is that applications are conceived of as things that run once and then exit. The web doesn't have a model of background computation. This means you cannot have agents that continue to act on your behalf while you are not on a web page. Clearly this can be done in the cloud - but the argument here is that there could be a single seamless computational environment where the agent moves to the best place to run (as defined by the developer). If it needs to run persistently and the client app container is not persistent, then it can be moved to a better place such as a cloud app container.

If we think of apps this way then apps in fact become lighter concepts - they become what we would more colloquially refer to as "agents" - a concept of execution that we think of as durable, persistent and background - rather than necessarily foreground. In this model the "view" is what we see in web apps today - the client side web app is today only a view on background state.

# App compilation and packaging

Writing applications for traditional ecosystems typically consists of programming in highly specialized programming languages such as C, C++, Swift, Rust or Javascript. Source code written by developers is compiled (or uses webpack or some other post-processing tool) and a manifest of some kind is used to define statically linked third party libraries using Cargo Crates or NPM modules or Apt or Pkg or other library distribution schemes to generate a static blob that represents the application, service or utility in question. On the web the manifest is the index.html file. In npm it is packages.json. In a C program it is the makefile. In Rust it is the cargo crates file. Some application containers such as Deno work hard to hide the link stage but there is still a monolithic intent.

The issue here is that applications today are conceived of as heavy, statically bound, written by experts and updated infrequently blobs of code. The mindset around app 'composition' is one of labor - of a serious endeavor. This is a fallacy. There's nothing intrinsically heavy about wiring together a bunch of libraries into an executable - it is just that our "pipeline" for concept to completion is one that places compilation early rather than late.
The opportunity here is to move the final compilation phase or late binding phase to the target application container. This allows apps to become smaller, lighter and to be updated more easily and frequently. More specifically if apps are decomposed into pieces then updating one piece of an app (one module) doesn't necessarily mean tearing down the whole app and reloading it - or even stopping an app instance that is already running.

# Competing Concerns in writing code

Programming could be made more accessible to novices if apps were more granular and separated 'glue' from 'core code'.

Application programmers tend to merges multiple distinct almost unrelated domains because the tools often have only a single programming language for multiple concerns. We often see attempts to "rewrite SQL in a rust like way" for example to bridge over these issues.
One domain is the actual definition of procedural behavior over time, the specialized logic and behavior that makes an app novel. This may very well always be challenging - and may always require expertise - depending on how good our new AI tools become.

Another domain is the simpler "wiring" of heavy pieces together. We see with Unreal Blueprints some attempt to separate high level concerns from low level concerns so that say a game designer can wire together high level behaviors out of a 'game grammar' that has been composed by a lower level programmer in response to design needs.

We do see substantial evolution and velocity around thinking here. For example Entity Component Systems have become popular over the last twenty years. One could imagine being able to expose at a high level a series of "nouns" (the data or objects in an app) to a series of "verbs" (the actions to take on those data objects). And that novice programmers could effectively compose sentences at a high level out of these pieces.

# Single View Paradigm

Applications today also presume that they have control over an entire user view.
The mindset of traditional apps is one of a single page or screen that the application can largely paint whatever it wants onto.

However future interfaces, specifically Augmented Reality interfaces, are going to be a single view where multiple apps have to work together to overlay data onto that view. This becomes more like an audio sequencing engine where emitters have to express priorities in the right way so as to be able to occupy scarce user real-estate. Also because the view may itself consists of external 3d objects (the real world) it becomes important to express the placement of virtual annotations semantically - such as "pin this note at eye level to the nearest wall" rather than absolutely. It's best to in fact express user interface layout at a high level abstractly with a grammar such as some kind of 3d html rather than to over-qualify desired layouts.

# Distribution Issues

Distributing applications is probably the most serious challenge for creative app development and sharing by novices.

Distribution involves a series of increasingly complex hoops - from key signing, to creating content artwork, applying to app stores for permission to publish the app, and jumping through whatever hoops or quality controls that the private corporation has defined.
Apps that are multiplayer also tend to require bringing down their entire server and upgrading it and restarting it.

And during a development process even shipping large app builds around to a whole team can be incredibly time consuming. Even the process of moving large blobs of code around is itself a barrier to sharing and creativity. This is especially true of game engines such as Unreal where there's a tendency to simply start throwing everything, code, graphics and the kitchen sink into the repository while the compilation time takes longer and longer.

The entire legacy pattern of monolithic apps deserves a fresh look.

# Blockers

We additionally see these other blockers for lightweight app composition:

1. platform specific -> applications (other than web apps) seem overly tied to implementation and hardware - for example an app and its dependencies can be described using npm modules, or rust crates, or a c makefile, and then associated code in a grammar such as javascript, rust or even c... but most of what an app does is simply talk to a big pile of libraries, which are largely standardized - and the actual glue logic or scripting is often less critical; but the idea of the app cannot be hoisted out of the specific toolchain...

2. poor scenario definitions -> applications don't seem to have very formal ways of describing permissions, this makes it very hard to run an app over the wire... rust/wasm are making great strides here of course

3. needless complexity -> it's difficult for novices to use the current grammars required to define rich apps - it should be possible to define an app leveraging third party libs and a bit of scripting glue - and this should be about as difficult as defining a static website in html
lack of persistence -> what's also missing is a way for the average consumer to describe an application or even just a single-purpose agent at a high level and publish that service to a persistent and durable network environment where that service can keep running and interacting with other agents. Most models of computation today for consumers have a concept of "downloading" and "installing" and then "running while being observed" and then "exiting". There just doesn't seem to be a way for consumers to think about software agents that can easily run durably in the background until stopped.

# Rethinking these patterns

There are issues all up and down this design pipeline. Often users want to glue together very simple apps with very little custom behavior. It should be possible to wire together and share simple apps out of pieces without having to do much at all.

If these issues were fixed then there's a class of lightweight simple app that doesn't exist yet that could exist - something like programming in Roblox. In fact the only places where pressure exists to rethink traditional application design issues are in the massively multiplayer online worlds space where it simply isn't possible to stop a game, or tear down the entire server and restart it whenever a new feature is desired.

As well while the Apple App Store, or the Steam store, or the Epic Store or the Android Store does protect users from bad actors, they also accidentally create an almost total barrier to trivially create and distribute a skit, a small piece of interactive humor, or anything transitory, ephemeral and temporary. There are entire classes of trivial apps that simply do not exist.
The Web does remains a free and open frontier here where anybody can create and share an app but even here it could be easier. Notably it isn't possible to run persistent agents in the background on the web, and it isn't possible for pieces of code written by one user to easily interact with pieces of code written by another user to produce emergent effects.

# A different way

An ideal application framework would be one that allows users to describe applications lightly in a way similar to the way we describe index.html documents today; as a manifest with dependencies - and to have a next generation client side "web browser" read that manifest, fetches the dependencies and builds and runs the app - and to allow those apps to continue to run in the background persistently if the user wishes.

Apps could be as simple as a single  'agent' - a small piece of code that may or may not have dependencies, that can move from machine to machine as needed, and that can leverage a compute environment that allows them to specify and resolve security policies and dependencies.

These can continue to exist within conventional powers that we have over apps - an ability to start and stop them, to inspect policies, to publish and share them. But new powers emerge such as the ability for apps to work together and produce emergent effects. Overall if we can reduce the cost of writing code we should be able to increase the power of humans to use code in everyday speech. 