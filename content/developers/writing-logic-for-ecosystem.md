+++
date = "2017-02-09T11:13:52+01:00"
title = "Writing logic for EcoSystem"

+++

## EcoSystem implements a thin application layer between the database and the outside world via the JSON and HTML apis.  This approach is well suited to business applications rooted in relational data, action-defined logic, and well structured data display and feed requirements. ##

In order to effectively build applications on the EcoSystem platform, you must have a clear understanding of how to implement logic at the database level, how EcoSystem handles concepts like users and permissions, and how this relates to the rest of your application.  This tutorial/guide will take you through a worked example step-by-step.

For this guide we will create the database layer for a simple list making application that will provide website users the functionality to browse pages of items, add them to a list and remove them from a list.  This functionality is a typical business requirement and covers a wide range of scenarios, such as a cart in an online store, a wishlist or task list.  Here we'll keep it generic.

### Step 1: The basic items table ###

Let's create a table to store our list of items, it might look something like this:

```
CREATE TABLE items (
	id serial primary key,
	name text,
	description text,
	image text,
	keywords text[],
	slug text,
	price numeric(18,2)
);
```

**There are three important fields here that will unlock built-in EcoSystem functionality:**

**Image:** In theory, you could refer to your image file with any field name you wish and just include the correct tag in the web template, but the EcoSystem admin panel looks for an 'image' field or 'images' array for displaying line thumbnails, so we use the field name 'image'.

**Keywords:** This array unlocks basic website categorization functionality.  If you'd like to auto generate categories, then include this field and tag your records with keywords.

**Slug:** For an item to have its own drilldown page in the server generated HTML site, you must include a slug field so a URL can be built.

### Step 2:  Allow 'items' to display on the website ###

EcoSystem uses role-switching to enforce permissions.  Public HTML pages do not pass through the authorization system and are always assigned the role 'web', so lets go ahead and assign permissions with `GRANT SELECT on items to web`

### Step 3: The selections table

We'll now create a table to store the items a user has selected.  We might call this 'wishlist' or 'cart', depending on the specific application, but here were going to keep it generic and go with a logic-based name: 'itemsxuser' (which I pronounce "items per user").  I'm deliberately avoiding using underscores in the table name because they have a specific meaning in EcoSystem, which is covered elsewhere.  Underscores in field names are fine though, and make for very readable fields.

Now, to make things interesting, let's specify a very realistic business logic requirement - that the price of the item added to the list *must* be hard-encoded into the database record as the item is added to the list.  Why don't we just cross reference the price from the items table? Because it could change at a later date, and we want to lock down the price as soon as it is added to the list.  Of course, it doesn't have to work like this, but it's a sensible way to do it.  So, the 'itemsxuser' table looks like:

```
CREATE TABLE itemsxuser (
	id serial primary key,	
	item_id integer references items(Id),
	user_id uuid,
	price numeric(18,2)
);
```

We're keeping it simple - just a sequential key to reference the line, a reference to the user who added the item, a reference to the item and the hard-coded price as discussed above.

*At this point we should digress for a minute and go back over EcoSystem's user model.  EcoSystem assigns unique identifiers for new users and delivers them to the user's browser via a secure JWT where it is stored in local storage.  Since the user cannot decode the JWT locally, they cannot see their own user id.  When the user's browser makes calls to the EcoSystem server, with the JWT in the Authorization header, the JWT is decoded by the server, the user id retrieved and then included in SQL queries to the database by setting a variable called 'my.user_id'.  This allows us to use the user id in our database logic and thus apply the right logic to the right user - in this case, assigning a selection to a specific user in our 'itemsxuser' table.  Note that we don't necessarily have to hold any details about the user in our 'user' table or any other table; this only comes into play once we want to allow login and assign different roles to different users - but that's for another day.  For the time being, our users will always be 'anon' because that's the default role EcoSystem assigns.*

### Step 4: User permissions and the selection table ###

Lets start by giving 'anon' users access to the selections table with `GRANT SELECT, INSERT, MODIFY, DELETE on itemsxuser to anon`.  We'll also need to give them access to the sequencer with `GRANT USAGE ON SEQUENCE itemsxuser_id_seq to anon`

But you've probably already spotted the fatal flaw in this logic: users should only be able to read, insert and modify under their own user id, right?  No problem, Postgres allows this easily through its [row security policies (or row-level security)](https://www.postgresql.org/docs/9.5/static/ddl-rowsecurity.html):

```
ALTER TABLE itemsxuser ENABLE ROW LEVEL SECURITY;
CREATE POLICY restrict_itemselections ON itemsxuser
	USING (current_setting('my.user_id')::uuid IN (user_id));
```

Activating row-level security with `ALTER TABLE x ENABLE ROW LEVEL SECURITY` applies a blanket policy to restrict access to all rows of this table to every role unless there is a policy in place that specifies who is allowed to see what.

Note that the built-in 'admin' role in EcoSystem bypasses row-level security, so you don't have to waste time creating multiple policies for the 'admin' role.

The `CREATE POLICY` statement we applied says that for users with the 'anon' role, only let them see, manipulate or delete their own records.  So far so good.

But there is another problem: since users don't have access to their own 'user_id', *they can't include it in the call to the API* to insert, for example, so how do we know which user_id to attach to the new row? Again, we use database logic.

Even if users did have access to their user id and we made the full POST call using XHR with the 'user_id' as an attribute in JSON body, we could not, of course, trust the user not interfere with the API call and try to add or delete rows from someone else's list (although note that we are using UUIDs so this would be impossible to all intents and purposes).  We need to specify that for 'anon' users, we should automatically use their user id, as decoded from the JWT, when we make an insert, and that's just a simple function and a trigger on INSERT operations:

```
CREATE FUNCTION itemsxuers_init() RETURNS trigger AS $$
BEGIN
    -- Run the user id restriction
    IF current_user = 'anon' THEN
        NEW.user_id = current_setting('my.user_id')::uuid;
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER itemsxuser_init
    BEFORE INSERT ON itemsxuser FOR EACH ROW EXECUTE PROCEDURE itemsxuser_init();
```

### Step 5: My List ###

Now we have the logic set up to only and always allow an anonymous user to see and manipulate his 'own list', any calls that that user makes via the API to the 'itemsxuser' table will only return his/her own items as a matter of default - the row-level-security will not allow it any other way.  So, we have our choice of using the JSON api or the private HTML api (which DOES go through auth) to return a list of selected items to our user, either as JSON which we render client-side, or as templated HTML rendered server-side.

### Summary ###

We used EcoSystem to create the database layer for a simple list-making app.  With EcoSystem's built-in server and admin panel, it would take only a few minutes more to have a live web application running and an administrative backend.

We implemented our solution with all data specifications and some simple logic operating at the database layer, allowing the rest of the stack to be generic.

In terms of data we specified two tables: one to hold item records, and the other to hold records of selections of items by users.

In terms of logic, we:

- Specified that the items list could be shown on the public HTML api (website).
- Specified that the price should be hard coded in the selection record at the time of adding to the list.
- Specified that users could make selections, see already made selections and remove selections - but all restricted to their own user id.
- Specified that anonymously logged in users, when making a selection, would always have the user id in the selection defaulted to their own.

Using the above simple principles, combined with views for calculations and joining data, we can rapidly develop an API for interacting with our data with a clean separation between the data/logic layer at the base and the rest of the stack.


