# ReactNativeScanner
To develop a scanner application we need to integrate our app with two library 
  react-native-camera
  react-native-qrcode-scanner
## install react-native-camera
```
 npm install react-native-camera --save
```
## Automatic link library
```
  react-native link react-native-camera
```

## Configuration

First open android/gradle/wrapper/gradle-wrapper.properties  and modify gradle from 2.14.1 to 4.4
```
distributionUrl=https\://services.gradle.org/distributions/gradle-2.14.2-all.zip
```
to 
```
distributionUrl=https\://services.gradle.org/distributions/gradle-4.4-all.zip
```

open file android/build.gradle and replace with the code below

```
buildscript {
    repositories {
        jcenter()
        mavenCentral()
        maven {
            url 'https://maven.google.com'
            name 'Google'
        }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.1.0'
        classpath 'com.google.gms:google-services:3.0.0'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        mavenLocal()
        jcenter()
        maven {
            // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
        maven { url "https://jitpack.io" }
        maven {
            url "https://maven.google.com"
        }
    }
}
subprojects {
    project.configurations.all {
        resolutionStrategy.eachDependency { details ->
            if (details.requested.group == 'com.android.support'
              && !details.requested.name.contains('multidex') ) {
                details.useVersion "26.0.1"
            }
        }
    }
    
    afterEvaluate { 
        project -> if (project.hasProperty("android")) { 
            android { 
                compileSdkVersion 26 
                buildToolsVersion '26.0.1' 
            } 
        } 
    }
}
```
## Install react-native-qrcode-scanner
```
  npm install react-native-qrcode-scanner
```

## Link react-native-qrcode-scanner & react-native-permission
```
  react-native link react-native-qrcode-scanner
  react-native link react-native-permissions
```

# Example 
```
import React, { Component } from 'react';

import {
  AppRegistry,
  StyleSheet,
  Text,
  TouchableOpacity,
  Linking,
} from 'react-native';

import QRCodeScanner from 'react-native-qrcode-scanner';

class ScanScreen extends Component {
  onSuccess(e) {
    Linking
      .openURL(e.data)
      .catch(err => console.error('An error occured', err));
  }

  render() {
    return (
      <QRCodeScanner
        onRead={this.onSuccess.bind(this)}
        topContent={
          <Text style={styles.centerText}>
            Go to <Text style={styles.textBold}>wikipedia.org/wiki/QR_code</Text> on your computer and scan the QR code.
          </Text>
        }
        bottomContent={
          <TouchableOpacity style={styles.buttonTouchable}>
            <Text style={styles.buttonText}>OK. Got it!</Text>
          </TouchableOpacity>
        }
      />
    );
  }
}

const styles = StyleSheet.create({
  centerText: {
    flex: 1,
    fontSize: 18,
    padding: 32,
    color: '#777',
  },
  textBold: {
    fontWeight: '500',
    color: '#000',
  },
  buttonText: {
    fontSize: 21,
    color: 'rgb(0,122,255)',
  },
  buttonTouchable: {
    padding: 16,
  },
});
```

