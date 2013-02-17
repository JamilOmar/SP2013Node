# SharePoint 2013 apps with node.js

This is sort of a proof-of-concept for creating node.js apps with SharePoint 2013, using web sockets and everything. 
The strategy for SharePoint authentication is mostly taken from here, but it has been 
modified a bit. Mostly I stripped away stuff that I did not need for my own understanding.
https://github.com/QuePort/passport-sharepoint

The basic idea is that you deploy an appmanifest so SharePoint pointing to a website running this node.js webapplication.
SharePoint will go fetch encrypted refresh-tokens from ACS which the client will then post to this web app.
This app, knowing the app secret, will decode the encrypted token and then go to ACS to get the auth token.
Then, it can go to SharePoint to get user data etc. And it does a lot of stuff to connect the authenticated https sessions to web sockets.
I realize there is a lot of documentation to do. But it's a proof of concept and it sort of works.

## TODO
todo... a lot of stuff
Stub out the authorization server and decouple all the things

## Keys
I can't put the secrets in here, so I have not uploaded any real keys. You should make your own cookieSecret and get your own clientID and clientSecretfrom SharePoint when you deploy the app. You can put them in the keys file, like shown below, or if you use nodejitsu to run your app, like I do, you can use environment variables.

a keys.js file should contain secret keys for app and cookie-signing
```javascript
    var clientID = "17b74aca-d3b1-39a5-b11a-892043384b3a";
    var clientSecret = "Lbac16P1mX+amQjmfP2psKMahsfnTbsJKzv19jLi4b0=";
    var cookieSecret = "thisisfreakinghardtoguess";
    module.exports = {"clientID" : clientID,
                      "clientSecret" : clientSecret,
                      "cookieSecret" : cookieSecret};
```
when using nodejitsu, like I do, you can run
```shell
    jitsu env set clientID "17b74aca-d3b1-39a5-b11a-892043384b3a"
    jitsu env set clientSecret "Lbac16P1mX+amQjmfP2psKMahsfnTbsJKzv19jLi4b0=";
    jitsu env set cookieSecret "thisisfreakinghardtoguess";
```

![OAuth 2.0 dance](http://www.gliffy.com/pubdoc/4318056/M.png)
