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

### `Packages Used`

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

## Cookies & Sessions

### `Cookies and sessions implentation using PassportJS `

```javascript

const app = express();

app.use(session({
  secret: process.env.ENC_SECRET,
  resave: false,
  saveUninitialized: false
}));

app.use(passport.initialize());
app.use(passport.session());

 passport.serializeUser(function(user, done) {
      done(null, user);
    });
     
    passport.deserializeUser(function(user, done) {
      done(null, user);
    });
    
```


## Google OAuth2.0

### `Google OAuth Strategy, this strategy hashes and salts passwords when signing in without Google OAuth`

``` javascript

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret:process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: "http://localhost:3000/auth/google/secrets",
    userProfileURL: "https://www.googleapis.com/oauth2/v3/userinfo"
  },
      function (accessToken, refreshToken, profile, cb) {
        User.findOrCreate(
          { username: profile.id },
          {
            provider: "google",
            email: profile._json.email
          },
          function (err, user) {
              return cb(err, user);
          }
        );
    }
));


```

Authentication Requests

```javascript
app.get('/auth/google',
  passport.authenticate('google', { scope: ['profile'] }));

app.get('/auth/google/callback', 
  passport.authenticate('google', { failureRedirect: '/login' }),
  function(req, res) {
    res.redirect('/');
  });

```


## Sign In , Sign Up & Log Out

### `Sign Up`
```javascript
.post(function(req, res) {

	const username = req.body.username;
    const password = req.body.password;

	User.register({username: username}, password, function(err, user) {
          if (err) {
            console.log(err);
            res.redirect("/register");
          } else {
            passport.authenticate("local")(req, res, function() {
              User.updateOne(
                { _id: user._id },
                { $set: { provider: "local", email: username } },
                function() {
                  res.redirect("/secrets");
                }
              );
            });
          }
	});

	
});

```

### `Sign In`

``` javascript
.post(function(req, res) {

	const user = new User({
		username: req.body.username,
		password: req.body.password
	});

	req.login(user, function (err) {
		if (err) {
			console.log(err);
		} else {
			passport.authenticate("local")(req, res, function () {
				res.redirect("/secrets");
				});
		}
		});
	

	});





```

### `Log Out`



```javascript

app.get("/logout", function(req, res){
  req.logout();
  res.redirect("/");
});



```
