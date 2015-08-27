# Prototyping Android Apps quickly, using Cordova

## Pre-Requisites
- The Android SDK and an Android Emulator (or use a device)
- Google Chrome
- Install Node and the Node Package Manager (NPM)

  *Recommend method*: Install [HomeBrew](http://brew.sh/) (http://brew.sh/) and then install node:
  
  `ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
  
  `brew install node`
  
  Optionally, you could install directly from http://www.nodejs.org, but it's harder to uninstall after using this
- Install the TACO & Bower tools using the command `npm install -g taco-cli bower`
- Launch the Android emulator now, to save time during the lab
- To follow along: `git clone http://github.com/jmatthiesen/CordovaAndroidProto-Tutorial` 

## Step 1
This step creates a simple Cordova project targeting Android. From a terminal window, run:

1. `taco create sampleApp` # Create a simple Cordova app
1. `cd sampleApp` # From now on, work in the app directory
1. `taco platform add android --save` # Add the Android platform and save it in config.xml 
1. `taco run` # To run the project and deploy to device/emulator
1. Open the sampleApp folder in your favorite editor

To see the end result, you can run `git checkout Step1` on the sample repo.

### Things to note
- Look into the platforms folder for the familiar AndroidManifest.xml
- Look at the MainActivity.java and CordovaActivity.java to see how webView is initialized
- Look at ConfigParser to see how config.xml is used to define the URL that the Webview displays

##Step 2
Now let's look at a project setup with Material design.

1. `git checkout Step2`
1. `bower install` # Downloads libraries used by the app
1. `taco run`
1. Open `main.html` to see the HTML for this page
1. Open `main.js` to see the logic for the page, this is using Ionic and AngularJS
1. In `main.js` $stateProvider is used to configure state transitions/routes between pages of the app.
1. Let's add one item to the UI real quick, open `samplePage.html`
1. Inside of the `<ion-content>` item, add the following code:
  
  ````HTML
  <div class="card">
    <div class="item item-divider">
      <h2>I'm a demo page!</h2>
    </div>
    <div class="item item-text-wrap">
      This is a basic Card with some text.
    </div>
    <div class="item item-divider">
      I'm a Footer in a Card!
    </div>
  </div>
 ````

### Things to note
- You're looking at a "kitchen sink app using the Ionic framework with a Material Design library applied
- Bower is a package manager for the web, that helps you find and install popular web libraries.
- Ionic adds controls and an app framework on top of Cordova, it uses AngularJS
- Material Design lite is the library we're using with Ionic
- There's a "hooks" folder that defines custom build hooks, which simplify this template by combining multiple JS libraries into one file referenced by the app
- To add a card to the UI, we simply used a CSS class. Ionic has many more components like this, documented at http://ionicframework.com/docs/.

##Step 2a
Now we'll look at a fast refresh/debug/design workflow

1. In the Terminal, Ctrl-C to end the current Cordova session
3. `taco run`
4. After the app launches, open Google Chrome
5. Open `chrome://inspect`
6. Select the inspect link for the application you have running
7. In the dev tools that open, use the magnifying glass icon and click on tap the word "controlls" in your app
8. In the dev tools, double click the highlighted HTML and replace "controlls" with "controls"
9. Click around using the magnifying glass, navigating in the HTML. Change some colors and observe your changes in the UI.
10. Now, in the terminal Ctrl-C again
2. `taco plugin add cordova-plugin-browsersync` # Installs a "live reload" plugin
1. `taco run -- --live-reload --ignore=lib/**/*.*` # Run with live reload enabled, ignoring library files
11. In the source code for the app, open `sample.tpl.html`. Fix the typo in "Controlls" and name it "Controls"
12. Save your changes
13. In the emulator/device, notice that the typo fix auto-loads.

### Things to note
- The Chrome dev tools help you remotely debug a Cordova application/web site
- You can use these tools to quickly experiment with your UI
- Live reload tooling helps you bring the fast web dev workflow to mobile apps, letting you make changes and push them to the app immediately without building.
- With BrowserSync you could also debug multiple devices at the same time and synchronize changes immediately

##Step 3
Building a full fledged app prototype

1. `git checkout Step3`
1. `taco run`

### Things to note
- The app uses a Material design theme
- Click the top left icon and you'll see a navigation side menu
- Viewing the Edit Profile screen, you can see input fields. Clicking a field, you can see field label animations modeled after Material Design
- Clicking to Scan a QR code, you currently just see a placeholder screen

##Step 4
Using a plugin and native device capabilities. Let's add a qrCode scanner.

1. In your browser, open http://plugins.cordova.io/npm/index.html and search for "barcode"
1. Take a quick look at the docs for the phonegap-plugin-barcodescanner, copy and paste the full example for using the plugin
1. Close any `taco run` sessions
1. Run `taco plugin add phonegap-plugin-barcodescanner` # Adds a the barcode plugin that provides QR Scan functionality
1. Open `scanQr.js`
1. In the controller() function, defining the ScanQrCtrl, paste in the following source from the Cordova docs

  ```JavaScript
  cordova.plugins.barcodeScanner.scan(
      function (result) {
          alert("We got a barcode\n" +
                "Result: " + result.text + "\n" +
                "Format: " + result.format + "\n" +
                "Cancelled: " + result.cancelled);
      }, 
      function (error) {
          alert("Scanning failed: " + error);
      }
   );
  ```
1. Run `taco run`
1. In the app, load the Scan QR screen. The camera will open and the plugin will look for a bar code.
1. To see a full running version of the app. Get the final version from `git checkout Step4`

### Things to note
- Plugins enable a Cordova app to talk to native device capabilities
- If you use the full version of the demo app, you can save contact information with your own QR code and scan that QR code using another version of the app

## Step 5
Deploying the app privately, to get end user feedback. For this we'll use the HockeyApp service.

1. Go to http://www.hockeyapp.net
1. Click on Sign Up to create a new (free) account
1. A confirmation email will come to you, follow the directions in that email
1. Once registration is confirmed, follow instructions on the site to install the HockeyApp app on your device.
