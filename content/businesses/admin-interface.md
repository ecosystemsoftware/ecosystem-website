+++
date = "2017-01-25T17:05:00+01:00"
title = "The EcoSystem Administration Interface"

+++

## The final layer of an EcoSystem installation is the web-based administration panel application which sits on top of the JSON API made available by the EcoSystem server.

On the surface, administration panels for ERP software look intricate, complex and highly individual, but in fact 99% of all possible requirements can be summarized by a set of standard components, as long as those components are highly configurable.

Just like the server, the administration panel in EcoSystem takes a generic approach.  Since it knows nothing about the data structures you are going to need to manipulate, it actually doesn't show anything to start with.  Your developer quickly creates 'views' on your data using simple configuration files and industry-standard JSON.  It's the equivalent of saying "Give me a sortable, keyword searchable list of cars showing the make, model, mileage and an image and give me buttons to quickly mark whether cars are out-of-action or not".  Again, cars, bookings, services are all the same to EcoSystem - it just needs to know which list the data is coming from.
