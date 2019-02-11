# e-mission devapp (adapted from PhoneGap Developer App)

> Work on the emission UI without setting up the native build environment

## Overview

The e-mission developer app (adapted from the [PhoneGap Developer
App](https://github.com/phonegap/phonegap-app-developer)) is a testing utility
for web developers and designers using the e-mission platform. After installing
the e-mission devapp on your emulator or physical device (android) or emulator
(iOS), you can connect to your e-mission phone app live deploy server. This
allows you to:
1. view and test the UI instantly,
2. without compiling any native code, and
2. reload the UI automatically on every saved change

For more information, see the CLI instructions in the [Developer App Reference Guide][3] on the PhoneGap Docs. e-mission-dev-app has not been tested with the PhoneGap desktop app. If you get it to work, please submit a pull request with instructions :)

## Issues
Since this repository is part of a larger project, all issues are tracked [in the central docs repository](https://github.com/e-mission/e-mission-docs/issues). If you have a question, [as suggested by the open source guide](https://opensource.guide/how-to-contribute/#communicating-effectively), please file an issue instead of sending an email. Since issues are public, other contributors can try to answer the question and benefit from the answer.

## Download
Unlike the PhoneGap Developer App, the e-mission devapp is NOT in the stores.
You will need to manually download the latest release.
https://github.com/e-mission/e-mission-devapp/releases

It turns out that the PhoneGap Developer App has also been removed from iTunes,
so there doesn't appear to be much advantage to putting the app in the stores.
(https://twitter.com/phonegap/status/982022400085712896)

## Installing

1. Install the development enviroment
    - iOS: xcode + [ios-sim](https://github.com/ios-control/ios-sim#node-js)
    - android: android studio

2. Launch the simulator, either:
    - from the development environment UI (use xcode or android studio help), or 
    - from the command line
        - iOS: `$ ios-sim start`
        - android: `$ $ANDROID_HOME/platform-tools/emulator/emulator -list-avds | head -n 1 | xargs $ANDROID_HOME/platform-tools/emulator/emulator -avd`

3. Drag and drop the binary into the emulator
    - iOS: em-devapp.app
    - android: em-devapp.apk
    
4. Launch the installed app and wait for it to go through the "Loading" and "Hi!" screens until it comes to the "Server address" screen

4. Start the server in a clone derived from the [e-mission-phone](https://github.com/e-mission/e-mission-phone) repository

1. Fill in one of the URL(s) that the server is listening to as the "Server address"

1. "Connect" and enjoy tinkering with the UI

## Installing on a real phone

If you want to test on a real phone instead of the simulator, use these options instead of steps 2 and 3 above. The other steps are the same.
- android: install the apk to your phone (search "install apk on phone"). Obvious choices include either downloading the apk directly from the releases page, or by installing via `adb`.
- iOS: installing apps onto a real device requires an Apple Developer license. Since the app has to be signed by keys associated with your developer license before installation, we can't publish `.ipa` files as part of the release. So you have to build and install the devapp from scratch.

    ```shellsession
    $ git clone https://github.com/e-mission/e-mission-devapp.git
    $ cd e-mission-devapp/
    $ npm install
    $ npm run phonegap -- run ios
    ```

**Note:** Sometimes, the last command (`$ npm run phonegap -- run ios`) fails because of errors while cloning plugins (`Failed to restore plugin "..." from config.xml.`). A workaround is at https://github.com/e-mission/e-mission-docs/blob/master/docs/overview/high_level_faq.md#i-get-an-error-while-adding-plugins

## Everything below this line is from the standard phonegap app README
---

The CLI parts of this documentation should work, the desktop app support will not.

## Documentation

*em-devapp note*: This is the part you need to read to figure troubleshoot app *usage*.

- [Developer App Reference Guide](http://docs.phonegap.com/references/developer-app/)
- [FAQ/Troubleshooting](http://docs.phonegap.com/references/developer-app/troubleshoot-faq/)
- [Unable to Connect or Download from Server](http://docs.phonegap.com/references/developer-app/troubleshoot-faq/#phonegap-developer-app-is-unable-to-download-from-the-server)

## Development

*em-devapp note*: This part is only useful if you want to *modify* the devapp.
devapp itself.

### Setup

The repository includes everything required to compile the app. You can get
setup by:

```shellsession
$ git clone https://github.com/phonegap/phonegap-app-developer.git
$ cd phonegap-app-developer/
$ npm install
```

### Compile and Run

The app bundles the `phonegap` npm module as a dependency. This means that
there is no need for a global installation of `phonegap` or `cordova`.
By editing the `package.json`, you can specify the exact version of `phonegap`
to compile the project.

Since a global install of the `phonegap` module is not required, we use
`npm run` scripts to compile and run the application. This allows the app's
`platforms/` and `plugins/` directories to be removed and rebuilt each time,
ensuring a consistent build using the correct PhoneGap, Cordova, platform,
and plugin versions.

The command structure is:
```shellsession
$ npm run phonegap -- <command> [args]
```

For example, you can check the version of `phonegap`:
```shellsession
$ npm run phonegap -- --version
```

You can compile and run iOS or Android:
```shellsession
$ npm run phonegap -- run ios
$ npm run phonegap -- run android
```

For developers wishing to use the platform SDKs (Xcode, Eclipse, Visual Studio),
please build once with the CLI to correctly populate the platform assets:
```shellsession
$ npm run phonegap -- build <platform>
```

Due to a Windows npm bug, the `--` does not work. Therefore we have created
run script that will build Windows Phone 8, so it can run in Visual Studio.
```shellsession
$ npm run phonegap-wp8
```

### Running the Tests

There are two types of test targets: the local app and served app.

#### Test the Local Application

The local application is the PhoneGap Developer App. To test the local
functionality, we build the application with mobile-spec. This allows us to
ensure that each plugin was correctly installed. Eventually, we would like
to add functional tests for the app logic.

You can run the local tests with:
```shellsession
$ npm run phonegap -- run android --test
$ npm run phonegap -- run ios --test
```

Again for Windows Phone, we have a run script that will build the app so it can run in
Visual Studio.
```shellsession
$ npm run phonegap-wp8-test
```

#### Test the Served Application

The served application is the app served by the CLI. To test the server
functionality, we serve an instance of mobile-spec. We can then use the manual
and automatic tests to ensure that the server provided the correct version of
Cordova, the plugins, and the navigation logic.

You can run the served tests with:
```shellsession
$ cd tests/
$ phonegap serve
# now connect with the latest build of the PG App
```

### Updating the Tests

It's as easy as a copy and paste.

1. Copy all content of `cordova-mobile-spec`
2. Paste the content into `tests/www`

On your first run of the tests, you may see some modified files. This is expected
because the test runner invoked by `--test` modifies certain files to support
our app's configuration.

### Adding Custom Plugins

If you're a developer interested in creating your own custom build of the
PhoneGap Developer App, then this section is for you!

Since the PhoneGap Developer app bundles `phonegap` as a npm dependency and
treats the `platforms/` and `plugins/` as artifacts, you'll find that it is very
easy to add your own plugins and alter other aspects of the app.

You may visit the [PhoneGap docs](http://docs.phonegap.com/references/developer-app/custom-build/ios/) for a more in-depth guide or follow the steps below.

First, [Setup](#user-content-setup) the project on your local system.

Second, edit the `config.xml` to add a custom plugin, change a preference, or
configure the app's name. You should also change the app's id to your own
unique app bundle id:

```xml
<?xml version='1.0' encoding='utf-8'?>
<widget id="org.mycompany.phonegap.app" version="1.6.2" xmlns="http://www.w3.org/ns/widgets" xmlns:gap="http://phonegap.com/ns/1.0">
```

Third, follow the [Compile and Run](#user-content-compile-and-run) section to
get the app onto your device.

If you run into any problems, feel free to [submit an issue](https://github.com/phonegap/phonegap-app-developer/issues).

### Releases

Releases are also generated using the npm scripts:
```shellsession
$ npm run release-android
$ npm run release-ios
$ npm run release-wp
```

In order to properly code-sign Android and iOS, you must setup the signing key.
This is accomplished by cloning the signing key repository and sym-linking each
platform directory with the PhoneGap Developer App repository:
```shellsession
# in your development directory
$ git clone https://github.com/phonegap/phonegap-app-developer-keys.git

$ cd path/to/phonegap-app-developer/
$ ln -s /path/to/phonegap-app-developer-keys/keys/ios resources/signing/ios
$ ln -s /path/to/phonegap-app-developer-keys/keys/android resources/signing/android
```

### Contributing, Commits, and Tags

See the [CONTRIBUTING.md][6] file for details.

[1]: https://play.google.com/store/apps/details?id=com.adobe.phonegap.app
[3]: http://docs.phonegap.com/references/developer-app/
[4]: http://github.com/phonegap/connect-phonegap
[5]: http://github.com/phonegap/phonegap-cli
[6]: https://github.com/phonegap/phonegap-app-developer/blob/master/CONTRIBUTING.md
[7]: http://www.windowsphone.com/en-us/store/app/phonegap-developer/5c6a2d1e-4fad-4bf8-aaf7-71380cc84fe3
[travis-ci-img]: https://travis-ci.org/phonegap/phonegap-app-developer.svg?branch=master
[travis-ci-url]: http://travis-ci.org/phonegap/phonegap-app-developer
[bithound-img]: https://www.bithound.io/github/phonegap/phonegap-app-developer/badges/score.svg
[bithound-url]: https://www.bithound.io/github/phonegap/phonegap-app-developer

