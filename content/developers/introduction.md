+++
date = "2017-02-01T10:07:17+01:00"
title = "Introduction"

+++

EcoSystem is a platform that allows you to quickly develop completely custom data-driven websites, web-applications and backend systems.  EcoSystem doesn't assume anything about the data structure or logic of your business because you code it directly and freely at the database layer using the power of PostgreSQL.  EcoSystem then augments your database layer with a generic web and JSON API server written in Go and a generic backend administration panel created with Polymer, giving you a *bridge* to your data and logic with minimal boilerplate.


The EcoSystem platform consists of two parts - [an HTML/JSON server application written in Go](https://github.com/ecosystemsoftware/ecosystem) and a [configurable admin panel created with Polymer](https://github.com/ecosystemsoftware/ecosystem-admin).

The EcoSystem application is actually a command-line tool that allows you to easily initialise, configure and launch the EcoSystem server, as well as perform operations on **bundles** (more on those later).  If you are familiar with Go, you can also easily build a custom version of the server by including the packages (cmd, ecosql, handlers and utilities) as dependencies in your project. If you'd like to do that, check out the [Custom EcoSystem Server](https://github.com/ecosystemsoftware/ecosystem-server-custom) repository for some instructions and a template to work from.

 The EcoSystem server also serves the [EcoSystem admin panel application](https://github.com/ecosystemsoftware/ecosystem-admin) for you, but you need to clone/download into your project directory first.
