<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#path-of-exile-api-implementation-guide">About The Project</a>
    </li>
    <li>
      <a href="#description">Description</a>
    </li>
    <li>
        <a href="#what-can-i-do-with-path-of-exiles-api">Usage</a>
    </li>
    <li>
        <a href="#getting-started">Getting Started</a>
        <ul>
            <li>
                <a href="#registering-your-application-or-product">Registering Your Application or Product</a>
            </li>
        </ul>
        <ul>
            <li>
                <a href="#utilizing-api-requests">Utilizing API Requests</a>
            </li>
        </ul>
    </li>
     <li>
      <a href="#authorization">Authorization</a>
        <ul>
            <li>
                <a href="#grants">Grants & Scopes</a>
            </li>
        </ul>
    </li>
    <li>
      <a href="#authors">Authors</a>
    </li>
    <li>
      <a href="#version-history">Version History</a>
    </li>
    <li>
      <a href="#acknowledgments">Acknowledgments</a>
    </li>
  </ol>
</details>

<br></br>

<!-- ABOUT THE PROJECT -->
# Path of Exile API Implementation Guide

Path of Exile is a free online-only Action RPG developed by Grinding Gear Games. Many components of Path of Exile, such as in-game trading and character information, require interaction and communication between different applications. 
In order to fascilitate this connection, we are able to utilize APIs that allow the sharing of information and data between various applications and platforms.

<!-- DESCRIPTION -->
## Description
#
This guide serves as a compact tutorial for the implementation of Path of Exile's API. The source of the information provided in this guide can be found on [Path of Exile' Developer Docs](https://www.pathofexile.com/developer/docs/index) website. This is a personal project used to display my technical writing skills. This project is purely for personal training/practicing purposes and is <u>NOT</u> affiliated with Grinding Gear Games. All credits go to the original source of information.
<br></br>
<!-- USAGE -->
## What Can I Do With Path of Exile's API? 
#
Path of Exile's API allows developers to retrieve in-game data such as League data, PVP Match Data, Account & Character Data, Account Stash Data, and more. 

It is important to note that the access to the the data is primarily a snapshot of the data currently stored on the game servers.
<br></br>

<!-- GETTING STARTED -->
## Getting Started
#
In order to be able to begin utilizing Path of Exile's API, Grinding Gear Games requires all developers to register their application or product and be granted authorization to their existing Path of Exile account. You can create a free Path of Exile account by visiting [Path of Exile's Official Website](https://www.pathofexile.com/).
<br></br>

<!-- REGISTERING YOUR APPLICATION OR PRODUCT -->
### Registering Your Application or Product

To begin your registration, all developers must make a registration request by e-mailing oauth@grindinggear.com with their application or product details and requirements. The contents of the request must include the following:

* Developer's PoE Account Name
* Developer's Application/Product Name
* The [Scopes](#scopes) that they are interested in
* A secure Redirect URI if they plan to use the [`authorization_code`](#authorization) grant type
<br></br>

<!-- UTILIZING API REQUESTS -->
### Utilizing API Requests

The initializiation of of API requests requires: 
* Request Method - `GET`, `POST`, `DELETE`, .. 
* Request URI
  * Server Endopoint - `https://api.pathofexile.com`
  * Query Parameters - `/league`, `/pvp-match`, `/profile`, .. <i><br>For full documentation on all of the available query parameters for Path of Exile's API, visit: [Path of Exile' Developer Docs - API Reference](https://www.pathofexile.com/developer/docs/reference)</i>
* Request Header
* OAth Authorization Token (Included in the Request header)
 
A simple GET request that is trying to retreive the data for a Stash in a specific League would look something like this:
```
GET https://api.pathofexile.com/stash/<league>/<stash_id>[/<substash_id>]
```

> **_NOTE:_** It should be noted that any application that interacts with Path of Exile's API must set an identifible User Agent header prefixed using the following format:

```
User-Agent: OAuth {$clientId}/{$version} (contact: {$contact}) ...
```

Example:

```
User-Agent: OAuth mypoeapp/1.0.0 (contact: mypoeapp@gmail.com) StrictMode
```

<!-- AUTHORIZATION & SCOPES -->
## Authorization
#
Many of the available developer APIs require OAuth 2 framework authorization.<br>
After a developer has successfully registered their application or product, they will be able to gain access to their credentials by logging-in on [Path of Exile's Official Website](https://www.pathofexile.com/), and selecting the "Manage" tab displayed on their application or product.
>**_NOTE:_** The first time a developer visits the page, they would need to generate and store their `client_secret`. In addition, Grinding Gear Games require all developers to use the `state` parameter which should be unique for each URL generated for the authorization code grant flow.

<!-- GRANTS -->
### Grants
There are three types of grants available for developers:
* Authorization Code Grant 
  * Used when developer's application requires access to act on another user's behalf<br>
    * Example request:

```
https://www.pathofexile.com/oauth/authorize
    ?client_id=example
    &response_type=code
    &scope=account:profile
    &state=10ceb8104963e91e47a95f4138448ecf
    &redirect_uri=https://example.com
    &prompt=consent
```

* Client Credentials Grant
  * Used to access services unrelated to an individual account. Does not have a set expiration time.
    * Example request:

```
POST https://www.pathofexile.com/oauth/token
...
Content-Type: application/x-www-form-urlencoded

 client_id=example
&client_secret=verysecret
&grant_type=client_credentials
&scope=service:psapi
```
* Refresh Token Grant
  * Used to generate a new access token without requesting the user's consent again.
    *  Example request:

```
POST https://www.pathofexile.com/oauth/token
...
Content-Type: application/x-www-form-urlencoded

 client_id=example
&client_secret=verysecret
&grant_type=refresh_token
&refresh_token=17abaa74e599192f7650a4b89b6e9dfef2ff68cd
```



<!-- SCOPES -->
### Scopes
There are three types of grants available for developers

<details open><summary>Available Scopes</summary>

* `account:profile` 
  * Allows access to the account's basic profile information.
* `account:stashes` 
  * Allows viewing the account's stashes and items.
* `account:characters`  
  * Allows viewing the account's characters and inventories.
* `account:league_accounts`  
  * Allows viewing the account's allocated atlas passives.
* `account:item_filter`  
  * Allows managing the account's item filters.
* `account:leagues`  
  * Allows fetching leagues.
* `account:leagues:ladder` 
  * Allows fetching league ladders.
* `account:pvp_matches` 
  * Allows fetching PvP matches.
* `account:pvp_matches:ladder` 
  * Allows fetching PvP match ladders.
* `account:psapi` 
  * Allows access to the Public Stash API.
</details>

<!-- AUTHORS -->
## Authors

[@pgom2](https://github.com/pgom2)

<!-- VERSION HISTORY -->
## Version History

* 0.1
    * Initial Release

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

Inspiration, code snippets, etc. gathered from:
* [Path of Exile' Developer Docs](https://www.pathofexile.com/developer/docs/index)

