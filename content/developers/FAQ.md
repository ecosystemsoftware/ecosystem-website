+++
date = "2017-03-03T09:57:13+01:00"
title = "FAQ"
+++

This is a collection of general questions that I often get asked about EcoSystem

## Is EcoSystem similar to [PostgREST](http://postgrest.com)?

It's very similar in principle, and I took a huge amount of inspiration from PostgREST in the design and architecture of EcoSystem.  In many ways, EcoSystem is the Go version of PostgREST (although you don't need to know Go - or Haskell in PostgREST's case - to use either of them).  If I knew Haskell, I probably would have used PostgREST as a basis for EcoSystem!  

I would say the main difference between the projects is that I wanted to bake a little more functionality into EcoSystem core - basically auxilliary services like authorization, authentication, HTML web serving, image resizing, static site generation, messaging etc.  Some of these "services" will be in the EcoSystem core, whilst others will be packages that can be included in a custom build.

Also, the eventual idea is for EcoSystem to serve an auto-generating administration dashboard, since this is a requirement of almost every project that would consider using EcoSystem.