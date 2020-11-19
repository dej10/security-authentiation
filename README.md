## Security & Authentication

Varying levels of security used: 
``` bash
* Level 1 - Username and Password
* Level 2 - Encryption
* Environmental Variables
* Level 3 - Hashing Passwords with md5
* Level 4 - Hashing and Salting Passwords with bcrypt
* Level 5 - Using PassportJS to add cookies and sessions
* Level 6 - Google OAuth2.0
```

Each security and authentication level is commited under its name.
Checkout the commits for the diffs


## Installation

Packages Used

``` javascript

require('dotenv').config();
const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require("mongoose");
const session = require("express-session");
const passport = require("passport");
const passportLocalMongoose  = require("passport-local-mongoose");
const GoogleStrategy = require("passport-google-oauth20").Strategy;
const findOrCreate = require("mongoose-findorcreate");


```

## 
