+++
date = "2017-02-19T10:40:58+01:00"
title = "Command line commands, arguments and flags"

+++

To run EcoSystem:

```
$ ecosystem [command] [argument] [flags]
```

Type `ecosystem --help` for a list of commands and available flags.

### Available Commands and Flags


| Command     | Argument              | Flags                                    |                                           |
| ----------- | ------------ | ---------------------------------------- | ---------------------------------------- |
| All         |              | -p, —pgpw                                | **Optional:** Postgres super user password, if required |
| `init`      | `db`         |                                          | Performs the DB initialisation for built-in tables, roles and permissions |
| `init`      | `folders`    |                                          | Creates the EcoSystem folder structure   |
| `install`   | `[bundle]`   | —demodata; -r, —reinstall                | Install named EcoSystem bundle.  Bundle folder must be downloaded/cloned into /bundles first. **Optional:** Include demo data with the bundle install. **Optional:** Uninstall the bundle before installing |
| `uninstall` | `[bundle]`   |                                          | Uninstall named EcoSystem bundle.  Will not delete the bundle folder from /bundles |
| `new`       | `bundle`     |                                          | Creates the folder structure for a new EcoSystem bundle |
| `new`       | `configfile` |                                          | Creates a config.json with all default attributes |
| `new`       | `user`       | —admin                                   | Creates a new user. **Optional:** make user with admin permissions |
| `serve`     |              | -d, —demomode; -a, —noadminpanel; -w, —nowebsite; -s, —secret; —smtppw | Starts the EcoSystem server. **Optional:** Run the server in demo mode, allowing users to log in with magic code '123456', rather than having to request a code. **Optional:** Use flags to disable the admin panel and/or website. **Required:** Encryption secret for JWT signing.  Remember to use the same secret every time you start the server, or JWTs previously issued will not be valid. |

Refer to the step-by-step [Getting Started Guide](/developers/getting-started), for an overview of the correct order in which to run these commands when starting a new project.

### Detailed Explanation of Commands

`ecosystem init` on its own runs the complete initialisation procedure, which consists of `ecosystem init folders` and `ecosystem init db`.  You can run each of those commands inividually if required.

`ecosystem init db` runs a series of SQL queries against the specified database in order to set up EcoSystem's built-in tables, roles and permissions.  In summary:

1.  Create the built-in *admin* role and grant default permissions so that *admin* will have access to everything created going forward .
2.  Install the UUID extension.
3.  Create the built-in *users* table which is used for authentication and authorisation.
4.  Create a function/trigger so that each new user is assigned as UUID.
5.  Create the built-in *web_categories* table.
6.  Creates built-in roles *anon*, *web* and *server*.
7.  Grant permissions to allow the role *server* to switch to other roles.
8.  Grant permissions to allow the role *server* to lookup users in the *user* table.
9.  Grant permissions to allow the role *web* to lookup categories in the *web_categories* table.

`ecosystem init folders` creates the basic project folder structure.  In summary:

*/img* for resized images to be stored

*/bundles* for bundles

*/ecosystem-admin* for the administration panel

`ecosystem install [bundle]` installs the bundle located at */bundles/[bundle]*.  You must clone or download the bundle (or create your own) before running this command.  The command searches for a folder in */bundles* containing the bundle files and throws an error if no folder with that name is found.

Installation means simply running the *install.sql* file from the bundle folder.  Read more about bundles to understand how they are strcutured, and what exactly happens when a bundle is installed.  If you specify the flag `--demodata`, the bundle's *demodata.sql* file is run.  The flag `--reinstall, -r` will attempt to uninstall the bundle before installing (see below).

`ecosystem uninstall [bundle]` uninstalls the bundle by removing the bundles's schema from the database.

`ecosystem new configfile` creates a config.json with all of the possible attributes - use it as starter template rather than creating your own from scratch.

`ecosystem new user [email@address]` is a quick way to add users without having to go to the database.  Give the user admin privileges with `--admin`

`ecosystem new bundle [bundle]` creates a folder in */bundles* with the required subfolder structure for an EcoSystem bundle.  Use this as a starter point when creating your own bundles.

`ecosystem serve` starts the ecosystem server.  You must supply a *secret*, which is a string used for encryption within the application.  Specify the *secret* with the `-s` flag, for example `-s=mysecret`.  The server will not start if you ommit the secret.

By dafualt, all three ecosystem servers will be activated (API, web and admin panel).  Use the flags `-a` and `-w` to disable serving of the admin panel and/or website respectively.  The flag `-d` is extremely useful for development and demo purposes - it bypasses the passwordless authentication system and allows registered users to login with the fixed code '123456'.


