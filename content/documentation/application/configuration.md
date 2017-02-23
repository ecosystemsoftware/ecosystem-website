+++
date = "2017-02-19T10:41:36+01:00"
title = "Configuration with config.json"

+++

The Ecosystem server, as well as some of the utility commands, like `init`, requires configuration via a *config.json* file in the root project folder.  The first thing you will do in most EcoSystem installations is create this file with the command `ecosystem`.

### Available Configuration Parameters


| Attribute                | Function                                 | Default                         |
| ------------------------ | ---------------------------------------- | ------------------------------- |
| pgSuperUser              | Username of database superuser with which to connect for initial setup | postgres                        |
| pgDBName                 | Name of the Postgres DB to connect to    | testdb                          |
| pgPort                   | Server port for the Postgres connection  | 5432                            |
| pgServer                 | Database server to connect to            | localhost                       |
| pgDisableSSL             | Disable SSL mode on DB connection        | FALSE                           |
| apiPort                  | Port to serve the EcoSystem API on       | 3000                            |
| websitePort              | Port to serve the generated website on   | 3001                            |
| adminPanelPort           | Port to serve the EcoSystem admin panel on | 3002                            |
| adminPanelServeDirectory | Local folder from which to serve the admin panel. For development, use the base directory 'ecosystem-admin'. For production use 'ecosystem-admin/build/bundled' or 'build/unbundled' | ecosystem-admin/build/unbundled |
| publicSiteSlug           | The URL base slug for the public website | site                            |
| privateSiteSlug          | The URL base slug for the private HTML api (authenticated) | private                         |
| smtpHost                 | SMTP server for outgoing emails          |                                 |
| smtpPort                 | SMTP port for outgoing emails            |                                 |
| smtpUserName             | SMTP authentication username             |                                 |
| smtpFrom                 | 'From' address for outgoing emails       |                                 |
| emailFrom                | 'From' name for outgoing emails          |                                 |
| jwtRealm                 | Realm parameter for JWT authentication tokens | yourappname                     |
| adminPrimaryColor        | Primary colour in the admin panel colour scheme | #00c4a7                         |
| adminSecondaryColor      | Secondary colour in the admin panel colour scheme | #7EC9A2                         |
| adminTextColor           | Text colour in the admin panel colour scheme | black                           |
| adminErrorColor          | Error colour in the admin panel colour scheme | Red                             |
| adminTitle               | General title for the admin panel        | Admin Panel                     |
| adminLogoFile            | Name of the logo file for display in the admin panel | logo.png                        |
| adminLogoBundle          | The logo is served from the folder bundles/[adminLogoBundle]/public/images | master                          |
| bundlesInstalled         | An automatically maintained list of bundles installed.  If you use `ecosystem install` and `ecosystem uninstall` commands, you shouldn't need to touch this. |                                 |
| host                     | The server name or IP address            | localhost                       |
| protocol                 | Serve with http or https                 | http                            |
