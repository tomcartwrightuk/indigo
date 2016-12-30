---
Title: "Introducing Entity Minr"
layout: post
date: 2013-11-15 22:15
headerImage: false
tag:
- scala
- nlp
blog: true
author: tomcartwright
description: Entity minr is Scalatra wrapper around the Standford Named Entity Recogniser. It provides a JSON API making it easy to use the entity recognition functions.
---

I have been writing an app that required extracting named entities from articles. The app was written in Ruby and was using the treat gem to tag text and the extract pronouns. This process was slow and used a lot of memory. The ruby-java bridge was used to call out to the Stanford NLP libraries and this occasionally caused crashes. 

So I decided to make a quick web service that could call these libraries directly in the JVM. Scala, which runs on the JVM is a perfect choice for this and the [Scalatra framework](http://www.scalatra.org/) provides a easy, lightweight way to create simple web services. 

The project is useable in it's current form but I need to add various features including concurrency support using [Akka](http://akka.io/) and I might change the response to a bag-of-words (or bag-of-phrases in this case). At the moment it returns an unordered array of tagged phrases that would need a little massaging to be of any use.

Find it on [Github](https://github.com/tomcartwrightuk/entity_minr).
