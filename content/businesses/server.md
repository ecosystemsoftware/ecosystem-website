+++
date = "2017-01-25T17:04:00+01:00"
title = "The web/API server in EcoSystem"

+++

## EcoSystem sits on top of your custom database and uses a generic approach to give you three important constructs with minimal set-up effort

1.  A JSON API on which you can build any number of websites, web apps, native apps or integrations.
2. A simple, easy-to-template HTML website.
3. An administration panel that can be customised precisely to your needs.

These are the ingredients of all modern, custom ERP/connected-web business systems.  EcoSystem avoids reinventing the wheel in each case by delegating all the complex logic to the database, as we have already discussed, allowing the API, web server and admin panel to be generic and templatable.

### What does this mean in practice?

Let's say you run a taxi company with 50 cars.  In your database, you create a list of cars with all the requisite information like make, model, colour etc.  Since this is a taxi company, you'd probably also maintain a list of drivers, customers and completed trips.  You might program the database to calculate total mileage of the cars by tallying up trips, and if you paid drivers by the hour, you might also sum up trip durations and automatically calculate payroll.  All done easily in the database in SQL, and cleanly separated from the higher layers of the system.

**Now the EcoSystem server steps in and gives you a website.**  The list 'cars' you created would be available at mytaxicompany.com/cars (if you wanted it to be. Of course) and your developer would create a template so customers could browse the cars you have available.  You might also want to show drivers' profiles at mytaxicompany.com/drivers and let customers log in to see their completed trips.

Again, the EcoSystem server doesn't care whether you're showing cars, hotel rooms or employees' profiles - if it's in the database, it can show it, if that's what you want.

The EcoSystem server is basically a very thin, generic layer that translates your database schema into a website (HTML) according to the templates you establish, and to JSON in the form of a restful API.  The restful API would make available the endpoint 'cars' so that (authorized) users could add, edit and delete cars.  But if you had a list called 'rooms' instead, the API server would make available the endpoint 'rooms'.
