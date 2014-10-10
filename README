MTurk API
=========

> SetUp :

```sh
   npm install
   node services/setup.js (Bootstrap Database)
   node app
```

This will start the localhost server at Port 3000.

>Postman

 * Install Postman REST Client Chrome Extension.
 * Import **postman.json** from root folder to Postman
 * Before making API Calls, you will need to login and get Access Tokens for all the Users.
 * Make login requests for three users (**User, Admin and Global Admin**) and copy the **Access Tokens** for all the three users.
 * Login Requests Name in the Postman Collection are,
**OAuth2 Login User, OAuth2 Login Admin User, OAuth2 Login Global Admin User**.
 * First make these three requests before making any API Call, as you will need to send the Access Token with all the API Calls.
 * In Postman requests JSON, before making request, each time replace the following

```sh 
"Bearer <user_token>" with "Bearer <access_token for the user@topcoder.com>"
"Bearer <admin_user_token>" with "Bearer <access_token for the admin@topcoder.com>"
"Bearer <global_admin_user_access_token>" with "Bearer <access_token for the global_admin@topcoder.com>"
```
and hit the request.
 * For user@topcoder.com = User Role
 , admin@topcoder.com = Admin Role and
   global_admin@topcoder.com = Global Admin Role
 * If you get, **"Access Token is expired"** message in any API Call, make the Login Request for that user again, and replace the new access token with the expired token.
 * Example Authorization Header,
```sh
Authorization: Bearer 191855af4737217d1c77dbe64ac4fbbc262500da
```


