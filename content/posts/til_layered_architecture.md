---
title: "TIL #2: The layered architecture approach in Go projects"
date: 2025-11-13T11:30:03+06:00
showToc: true
TocOpen: false
draft: false
tags: ["TIL"]
---

I started writing Golang code professionally for six months. I am building a push-based monitoring system. The basic idea is that a client will be installed on a server, collect metrics defined by the user, and send the data to the monitoring server.

Numerous Golang project templates can be found if one searches for the architecture. I highly encourage my readers to have a look at them and choose for themselves. The current architecture approach I picked up mainly from the [boot.dev](https://www.boot.dev/) courses.

> I am not affiliated with boot.dev in any way. I was a student of their bootcamp, and I have to admit, it was pretty amazing and learned a lot. When I was enrolling, I get to know from one of their blogposts that Lane (*the brain behind bootdev*) wanted to help his wife to learn web development. And, he was aware of tutorial hell and decided to do something about it. It happens to be turned out a great project! Recently, Lane posted another article addressing the similar scenario that's going on worldwide at the moment: [Vibe coding hell](https://blog.boot.dev/education/vibe-coding-hell/). Shoutout to them!

In the Golang projects of Bootdev, the structure was pretty much like this: all of the application-specific code related to the projects goes under the `internal` directory. The `internal` directory plays a crucial role in Golang’s project structure, enforced by the compiler. Some of them are:

- Restricted imports
- Controlling API exposures.
- Project layout.

Any code/packages that are defined under `internal` can only be imported by other packages (*packages that are already under internal*) or within the same project’s parent directory tree.

As the learners have to follow the given instructions to complete a task, they often write application-specific code under `internal` and defined separate packages based on the responsibilities/scopes.

While developing the web server and defining separate packages based on responsibilities (*handler, service, repository*), and data flows from one side to another, more like top to bottom. From handler to service, from service to repository, and vice versa. I came to know that this software design pattern has a name.

### The Layered architecture

Layered architecture is a software design pattern that divides an application into layers, each with a specific role, to promote separation of concerns. For example, the three main layers in my projects are- handler, service, and repository. Where

- The handler layer contains responsibilities like parsing requests, extracting parameters from the request, returning an HTTP response to the client, and calling services.
- The service layer is all about enforcing business rules. For example, validation of business rules, checking access control, orchestrating (*a word I picked up from music dept* :P ) operations, proper error handling, and calling the repository layer to retrieve and save data.
- The repository layer is solely about interacting with the database.

Following these conventions has a lot of benefits. One can easily pinpoint where to look in the code when debugging (*at the surface level at least*). The newcomers to the project will find it easy to get their feet rolling.

I have introduced more layers into the project, such as

- The policy layer, which helps to decide the authorization of a resource.
- The DTO layer helps to control what data we are receiving and showing to the end user.
- The model layer (*the most important*), which defines the project structure as well as the DB model structure.
- The Log layer, which helps in structured logging.
- The config layer, which helps to define the server configuration.

There are more architectures to explore, and one thing I wanna mention before ending tonight, let’s Go!