+++
date = "2017-02-05T10:18:46+01:00"
title = "What ecosystem gives you"

+++

If you followed the step-by-step [Getting Started Guide](/developers/getting-started), you should now have a working EcoSystem installation. Here's a quick rundown of what you now have in place:

### localhost:3000 - JSON API##

This is the basis of every EcoSystem implementation.  The built-in admin panel uses it, and you can use it to build any kind of API-backed application.  All requests to the API need to go through authorisation, so if you'd like to send some test requests, you'll need to start by requesting a JWT from **/login** - just include a JSON body in the request like this:

```
{
    "username": [your@email.com]
    "password": "123456"
}
```

grab the JWT that comes back and include it with your requests in an authorization header: `Authorization: Bearer xxxxxx...` - you'll have full admin permissions since you set yourself up as an admin user earlier.

### localhost:3001 - Website ###

This is optional, but if you only need a traditional server-generated HTML website, then EcoSystem gives you that out-of-the-box.  Head over to *localhost:3001* to the see the website that was generated with the simple templates in the bundle.  See how the URL path is basically just */SCHEMA/TABLE/RECORD*? That's the beauty of EcoSystem - it's simple!  Golang teplates are incredibly easy to use, so putting together a functional website is quick.

There's more in the box not covered by this short tutorial.  As well as full HTML pages, the web server can also deliver HTML partials and use the authorisation system, so you can easily use AJAX to swap in templated data specific to your current browser - allowing you to build fully dynamic server-generated pages, like carts and wishlist.

Of course, there's nothing to stop you using *both* the HTML API and JSON API to create hybrid sites using combination of server and client side rendering...

And finally, EcoSystem includes a growing collection of utility functions for easier HTML generation, like an automatic menu builder.

### localhost:3002 - Admin Panel ###

Every business needs an administration panel to control its data.  We're not talking about a visual website builder here, we're talking about a proper database visualisation tool to allows administrators to interact with custom data and logic.  Since EcoSystem knows nothing about your application, the admin panel is fully configurable with simple JSON - tell it what you want to see and do, and the admin panel is generated for you.  The easy way to understand how it works is by looking at the configuration files in the `bundle/admin-panel` folder - it's fairly self-explanatory.

Head over to *localhost:3002/admin*, log in with your email address and the password '123456' to get a feel for the admin panel.