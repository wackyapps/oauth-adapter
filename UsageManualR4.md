# Introduction #

This short introduction will explain how to use the OAuthAdapter.

Download the oauth\_adapter.js file according to your version of Titanium Mobile SDK and iPhone SDK:
  * Mobile SDK 1.3.3 and iPhone SDK 4.0: [oauth\_adapter.js r16](http://oauth-adapter.googlecode.com/svn-history/r16/trunk/oauth_adapter.js)
  * Mobile SDK 1.3.0 and iPhone SDK 3: [oauth\_adapter.js r4](http://oauth-adapter.googlecode.com/svn-history/r4/trunk/oauth_adapter.js)

You need to download the following two libraries to a lib subfolder (below the Resource folder) in your project:
  * [oauth.js](http://oauth.googlecode.com/svn/code/javascript/oauth.js)
  * [sha1.js](http://oauth.googlecode.com/svn/code/javascript/sha1.js)

Then download the [oauth\_adapter.js](http://oauth-adapter.googlecode.com/svn/trunk/oauth_adapter.js) in any folder you like and include it via Ti.include('oauth\_adapter.js').

# Details #

```
var oAuthAdapter = new OAuthAdapter(
	'<your-consumer-secret>',
	'<your-consumer-key>',
	'HMAC-SHA1');


// load the access token for the service (if previously saved)
oAuthAdapter.loadAccessToken('twitter');

oAuthAdapter.send('https://api.twitter.com/1/statuses/update.json', [['status', 'hey @ziodave, I successfully tested the #oauth adapter with #twitter and @appcelerator #titanium!']], 'Twitter', 'Published.', 'Not published.');

// if the client is not authorized, ask for authorization. the previous tweet will be sent automatically after authorization
if (oAuthAdapter.isAuthorized() == false)
 {
    // this function will be called as soon as the application is authorized
    var receivePin = function() {
        // get the access token with the provided pin/oauth_verifier
        oAuthAdapter.getAccessToken('http://twitter.com/oauth/access_token');
        // save the access token
        oAuthAdapter.saveAccessToken('twitter');
    };

    // show the authorization UI and call back the receive PIN function
    oAuthAdapter.showAuthorizeUI('http://twitter.com/oauth/authorize?oauth_token=' +
        oAuthAdapter.getRequestToken('http://twitter.com/oauth/request_token', [['oauth_callback', 'oob']]),
        receivePin, PinFinder.twitter);
}

```