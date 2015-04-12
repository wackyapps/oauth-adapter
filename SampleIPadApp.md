# Introduction #

This is a sample based on a clean iPad project with Titanium 1.3.0 and iPhone SDK 3.2.


# Details #

Support files (oauth.js, sha1.js and oauth\_adapter.js) are located in the lib subfolder. This is the app.js file:

```

// this sets the background color of the master UIView (when there are no windows/tab groups on it)
Titanium.UI.setBackgroundColor('#000');

// create tab group
var tabGroup = Titanium.UI.createTabGroup();


//
// create base UI tab and root window
//
var win1 = Titanium.UI.createWindow({  
    title:'Tab 1',
    backgroundColor:'#fff'
});
var tab1 = Titanium.UI.createTab({  
    icon:'KS_nav_views.png',
    title:'Tab 1',
    window:win1
});

var label1 = Titanium.UI.createLabel({
	color:'#999',
	text:'I am Window 1',
	font:{fontSize:20,fontFamily:'Helvetica Neue'},
	textAlign:'center',
	width:'auto'
});

win1.add(label1);

//
// create controls tab and root window
//
var win2 = Titanium.UI.createWindow({  
    title:'Tab 2',
    backgroundColor:'#fff'
});
var tab2 = Titanium.UI.createTab({  
    icon:'KS_nav_ui.png',
    title:'Tab 2',
    window:win2
});

var label2 = Titanium.UI.createLabel({
	color:'#999',
	text:'I am Window 2',
	font:{fontSize:20,fontFamily:'Helvetica Neue'},
	textAlign:'center',
	width:'auto'
});

win2.add(label2);



//
//  add tabs
//
tabGroup.addTab(tab1);  
tabGroup.addTab(tab2);  


// open tab group
tabGroup.open();



Ti.include('lib/oauth_adapter.js');
var oAuthAdapter = new OAuthAdapter(
        '<your-secret>',
        '<your-key>',
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