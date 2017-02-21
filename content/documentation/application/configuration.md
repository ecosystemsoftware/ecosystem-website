+++
date = "2017-02-19T10:41:36+01:00"
title = "Configuration with config.json"

+++

The Ecosystem server, as well as some of the utility commands, like `init`, requires configuration via a *config.json* file in the root project folder.  The first thing you will do in most EcoSystem installations is create this file with the command `ecosystem new configfile`.

### Available Configuration Parameters


| Attribute                | Function                                 | Default         |
| ------------------------ | ---------------------------------------- | --------------- |
| PgSuperUser              | Username of database superuser with which to connect for initial setup | Postgres        |
| PgDBName                 | Name of the Postgres DB to connect to    | ecosystem       |
| PgPort                   | Server port for the Postgres connection  | 5432            |
| PgServer                 | Database server to connect to            | localhost       |
| PgDisableSSL             | Disable SSL mode on DB connection        | FALSE           |
| ApiPort                  | Port to serve the EcoSystem API on       | 3000            |
| WebsitePort              | Port to serve the generated website on   | 3001            |
| AdminPanelPort           | Port to serve the EcoSystem admin panel on | 3002            |
| AdminPanelServeDirectory | Local folder from which to serve the admin panel. For development, use the base directory 'ecosystem-admin'. For production use 'build/bundled' or 'build/unbundled' | ecosystem-admin |
| PublicSiteSlug           | The URL base slug for the public website | site            |
| PrivateSiteSlug          | The URL base slug for the private HTML api (authenticated) | private         |
| SmtpHost                 | SMTP server for outgoing emails          |                 |
| SmtpPort                 | SMTP port for outgoing emails            |                 |
| SmtpUserName             | SMTP authentication username             |                 |
| SmtpFrom                 | 'From' address for outgoing emails       |                 |
| EmailFrom                | 'From' name for outgoing emails          |                 |
| JWTRealm                 | Realm parameter for JWT authentication tokens | yourappname     |