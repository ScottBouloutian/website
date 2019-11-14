---
layout: post
title: "Building a Mobile App with Cordova and App.js"
author: scott_bouloutian
categories: blog
excerpt: The plan is to use Apache Cordova to build a hybrid mobile application using App.js framework to fake native app appearance and functionality.
tags: []
image:
  path:
date: 2016-11-27T14:22:41-05:00
modified: 2016-11-27T14:22:41-05:00
comments: true
share: true
---

My greetings, today we will be building and deploying a mobile application on the iOS app store!
For the purposes of this tutorial we will be creating an iOS app named Wanda which tells you the
current date in the [Discordian Calendar][discordian].

## What's the plan Stan?
The plan is to use [Apache Cordova][cordova] to build a hybrid mobile application using
[App.js][app-js] framework to fake native app appearance and functionality. Lastly, Cordova
will be used to open the app in Xcode from where the final app will be tweaked and deployed.

## Cordova
Ok, let's start by installing Apache Cordova as a global npm package.

```
npm install -g cordova
```

And then bootstrapping an empty application with the following.

```
cordova create wanda
cd wanda
cordova platform add ios
npm install -g ios-sim
npm install -g ios-deploy
cordova emulate ios
```

If you have been following along a boilerplate Cordova iOS app will now be running in an
iOS emulator on your Mac.

## App.js
Download App.js from their [website][app-js]. The App.js bundle should contain four files.

1. move `app.min.css` to `wanda/www/css`
2. move `app.min.js` to `wanda/www/js`
3. move `zepto.js` to `wanda/www/js`

Next we will intelligently merge the `index.html` from the App.js bundle with the `www/index.html`
from the boilerplate Cordova application. For the final result check out my GitHub [repo][wanda-repo].

Lastly, open `www/js/index.js` and replace the code in the function `receivedEvent` with:

```
App.controller('home', homeController);
try {
    App.restore();
} catch (err) {
    App.load('home');
}
```

## Creating Wanda
In `index.js` create a function with the signature `homeController(page)`. This will be our App.js
controller for the home page of our app. Along the way we will be adding code to this function as well as adding HTML to our `index.html` file. The app is very simple. All it will do is show the current
date of the Discordian calendar. It will also allow the user to share this date as they choose.

Below you will find the HTML for the app. The class names prefixed by `app-` are from App.js and automatically style the corresponding element to look pseudo-native.

```html
<div class="app-page" data-page="home">
    <div class="app-topbar blue">
        <div class="app-title">Wanda</div>
    </div>
    <div class="app-content">
        <img class="fish-image" src="img/fish.png" />
        <div class="app-section">
            <p id="discordian-text"></p>
        </div>
        <div class="app-section">
            <div id="share-button" class="app-button blue">Share</div>
        </div>
    </div>
</div>
```

Now that the layout is set up we can implement the `homeController`. You can see what my
implementation ended up looking like below. Note that `discordian` is a function which
when passes a `Date` object returns the current Discordian date as a `String`.

```javascript
var text = 'Today is ' + discordian(new Date(Date.now())) + '.';
$(page).find('#discordian-text').text(text);
$(page).find('#share-button').on('click', function () {
    window.plugins.socialsharing.shareWithOptions({
        message: 'share this',
        subject: 'Wanda',
        url: 'https://www.google.com'
    }, noop, noop);
});
```

There is something else interesting happening here as well. You may have noticed the reference
to `window.plugins.socialsharing`. This is a reference to a Cordova plugin which adds native
sharing capabilities to your app. This plugin can be installed with the following command:

```
cordova plugin add cordova-plugin-x-socialsharing
```

## Submitting to the App Store
Before submitting your app to the store, you will most likely want to modify some of the values
in your `config.xml` file. Cordova provides excellent documentation on how to do so and on which
tags are available to you. Once you have done this, build your application and open it in Xcode.

```
cordova build ios
open platforms/ios/Wanda.xcworkspace
```

From Xcode you can now archive and upload your hybrid app in the same way as a native one. Before
submitting your app however, you may want to look through your Xcode project settings to ensure
that properties like your app's name and icons are what you expect them to be. You may want to
make some small tweaks to your Xcode project before deploying.

## Version Control
When adding your Cordova app to version control, you may or may not want to commit up the `platforms`
and `plugins` directories. If you choose not to, the following [gist][cordova-gist] may be helpful to
include with your project. It is used for initializing a freshly cloned Cordova application which does
not include these directories. Note however that if you have modified your app's Xcode project directly,
these changes will not be included if you do not commit up the `platforms` directory.

[app-js]: http://code.kik.com/app/3/index.html
[cordova]: https://cordova.apache.org/
[discordian]: https://en.wikipedia.org/wiki/Discordian_calendar
[wanda-repo]: https://github.com/ScottBouloutian/wanda
[cordova-gist]: https://gist.github.com/jonathandixon/7043297
