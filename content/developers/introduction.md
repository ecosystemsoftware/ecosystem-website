+++
date = "2017-02-01T10:07:17+01:00"
title = "Introduction"

+++

EcoSystem is a platform that allows you to quickly develop custom, data-driven backends.  It's a great alternative to 'backend-as-a-service' when you need the power of a custom server driven by a powerful relational databse system.

EcoSystem doesn't assume anything about the data structure or logic of your business because you code it directly and freely at the database layer using the power of PostgreSQL.  EcoSystem then augments your database layer with a generic JSON API server and a number of auxiliiary 'services' (like auth, image resizing, HTML page serving) which can be removed or added to with custom builds.  It's a *bridge* to your data and logic with minimal boilerplate.


The [EcoSystem application](https://github.com/ecosystemsoftware/ecosystem) is written in Go and (for the time being) needs to be compiled, but you don't need to know Go in order to use EcoSystem.  There is no need for programming at the application layer.

The EcoSystem application is actually a command-line tool that allows you to easily initialise, configure and launch the EcoSystem server, as well as perform operations on 'bundles' (more on those later).  If you are familiar with Go and would like to extend or modify the standard build, you can easily build a custom version of the server by including the ecosystem packages (core, auth, images etc) as dependencies in your project and combining with your own, or community contributed packages. If you'd like to do that, check out the [Custom EcoSystem Server](https://github.com/ecosystemsoftware/ecosystem-server-custom) repository for some instructions and a template to work from.