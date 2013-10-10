---
layout: post
title:  "Reusing Google OAuth2 Access Tokens in Perl"
date:   2013-10-11 06:04:10
categories: perl oauth2
---

Recently at [work] I was asked to move the statistics generated to assist with capacity planning from an internal wiki to a more accessible Google Sites page. To update the embedded spreadsheets and corresponding charts I wanted to drive refreshing the spreadsheets from the preexisting perl scripts that pulled the data from various sources.

There's a nice perl module called [Net::Google::Spreadsheets][net-google-spreadsheets] which gets you up and running quickly, but the problems start when you want to 

a) use OAuth2 so your password isn't in a script viewable by other system engineers. 
b) have the ability to authentication token without user interaction by requesting a refresh token which is updated as necessary if the previous one has expired.

Wherever I looked all I could find were people in the [same situation][same-situ]. There didn't seem to be any documentation on how to create a token that could be promoted to a refresh token and use that at a later point in time (handling whether it expired or not) to authenticate and access the spreadsheets.

Poring over what little documentation there was and reading through the source code of the modules I was finally able to work out how to give access to my application then continue to have access without further interaction from me to update the spreadsheets. It seems so simple now but hindsight is always 20/20.

Authorise your application with the following. 
{% gist 6738162 %}
You'll need to go to the url provided to accept the access, then the session will be saved in the current directory.

To use the saved session with the token we generated earlier here you can use this.
{% gist 6738247 %}

[work]: http://monash.edu/esolutions/
[net-google-spreadsheets]: http://search.cpan.org/~danjou/Net-Google-Spreadsheets-0.1501/
[same-situ]: http://xkcd.com/979/
