---
Title: "Objects on hexagonal rails"
layout: post
date: 2014-01-03 12:15
headerImage: false
tag:
- rails
- design patterns
blog: true
author: tomcartwright
description: Rails conventions can lead to hard-to-maintain apps but launching into a full OO approach has implications. How do you find a balance?
---

Six months ago I sent an email to the London Ruby User Group mailing list to canvas opinions on hexagonal rails and object on rails techniques. We were starting a new product at keepmebooked and it seemed a good chance to employ some of the OO techniques that have been all the talk of late [^1]. As rails ages, so have the projects that built using it. Once they were brand new, agile developed websites; now they are giant behemoths sitting in maintenance mode. Many have found these large projects hard to maintain and have blamed the rails conventions for this. Numerous articles, books and presentations have been sprung up outlining patterns that could circumvent the problems. I was keen to employ some of these techniques but was aware that there would be a price to pay for any gains.

I received many responses from people who have started to develop their applications in a more OO-centric style but no-one was able to say that they had completely mastered an approach. Several themes emerged; employing these techniques would cost more up front in boiler plate code. Others were concerned that going to off-piste would involve making a decision about everything at every stage and that a coherent approach would be hard to get at. The on-board time for new engineers would be longer as I would not be able to say ‘follow the rails way’ and everyone understand what that meant. 

With these thoughts in mind, I decided on a set of principles to guide the development of the app:

1. To begin with, the app will be developed in the traditional rails way. Logic will live in mainly skinny controllers and fat models.
2. When complex logic starts bloating the models, it should be refactored into service objects.
3. Service objects will have one responsibility and a small well-defined interface. We are not talking about shunting logic into modules in a /concerns folder and including them willy nilly.

If good architecture is the ability to draw defined lines, [^2] this approach may seem like taking a large crayon and just colouring everything in. The single responsibility principle (SRP) is not being adhered to as models are allowed to have many responsibilities so the boundaries that should encapsulate the business logic are fuzzy. This slows down the development of extensions to existing functionality as the first step in tackling a user story is often to create some services from the existing logic. But the advantage of this approach is that there is a defined first step that is not so off the beaten rails way as to be confusing to new (to the project) developers. The workflow for a story becomes:

1. Find logic in models that implements the functionality you are extending.
2. Determine if you are going to have to add to it significantly.
3. If you are - create a set of collaborating objects to perform that functionality.
4. Having recreated old functionality, extend it with ease in your newly created service.

### It was easier than it sounded (but we cheated slightly)

These principles have been easier to enforce than you might imagine for one reason: The rails app is only an API. I originally envisaged the app being a bog standard rails one but for reasons, we decided to use AngularJS as the client-side library that handles all the front-end rendering and behaviour. This has made writing the rails app decidedly easier as view objects (forms, view helpers etc) were not handled. JSON requests come in and JSON responses are spat out. It’s very easy. Of course we have not lost all the complexity of handling views and the HTML. We have simply moved them to the front-end. But we have been happy with our choice. For starters, our rails app has been a joy to develop!

### The importance of integration tests

Because the refactoring process involves creating new objects not just including modules, tests that cover the behaviour of the models will begin to break. If we had only unit tests covering the behaviour of models, recreating that logic in a service layer would have been hard. With a good set of good integration tests (in our case request specs) that covered the behaviour of the app from the API endpoint perspective, we have had confidence during refactoring.

### In conclusion

This rails app has been great to develop. There are are some disadvantages. For one, whenever you are changing or extending functionality of service objects, you have to remind yourself of the structure of the code. This invariably involves opening a folder worth of files, picking an entry point and then following the flow of the code until you remember what everything does. But once you do remember, you are instilled by so much confidence in the role of each object and it's straight forward to determine where the new functionality should go. In another post I will explain how we structure our service objects and a bit about the refactoring process.

[^1]: [Hexagonal rails](http://www.youtube.com/watch?v=CGN4RFkhH2M) by Matt Wynne and [Objects on Rails](http://objectsonrails.com/) by Avdi Grimm both muse on using standard Ruby objects to proxy to rails ones to remove dependencies. 

[^2]: [Uncle Bob Martin](http://www.youtube.com/watch?v=WpkDN78P884) -  Architecture the Lost years ~ 45mins


