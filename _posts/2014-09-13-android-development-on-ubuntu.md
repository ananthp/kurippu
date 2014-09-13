---
layout: post
title: "Android Development On Ubuntu"
date:   2014-09-13 18:52:20
categories: dev linux
---


## Setup

Following list provides basic idea of what is required:

1. Oracle JDK
1. [Gradle](http://gradle.org/) - Alternative for Ant
1. [Espresso](https://code.google.com/p/android-test-kit/wiki/Espresso) - Functional testing framework
1. [Robolectric](http://robolectric.org/) - Run unit tests (and even functional and integration tests) on desktop JVM instead of Dalvik
1. [Spoon](http://square.github.io/spoon/) - Run all the device tests simultaneously. Can be combined with espresso. `gradle spoon ...` 

Alternatively, here is the way to get android development VM:

1. Install [vagrant](http://www.vagrantup.com/)
1. Install [android-vm](https://github.com/rickfarmer/android-vm)
### Install Oracle Java (version 7 for Android L, 6 for up to kitkat)

```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
sudo apt-get install oracle-java7-set-default

java -version
```
### android SDK

http://www.unixmen.com/install-android-sdk-ubuntu-14-04/

### emulator

Reference: http://stackoverflow.com/a/12941873/15139

1. Install KVM. https://help.ubuntu.com/community/KVM/Installation
1. Install Intel x86 emulator images from SDK Manager
1. Create AVD with Intel x86 CPU/ABI
1. Run it using `emulator -avd <avd_name> -qemu -m 512 -enable-kvm`

Alternative:

genymotion (http://schier.co/post/install-genymotion-2.0-in-ubuntu) #
Worked fine and takes up less screen space on laptop but  didn't work
very well on the desktop

### Directory structure

```
  src
    |-- main
    |   `-- java
    |       `-- com
    |           `-- example
    |               `-- helloapp
    |                   `-- MainActivity.java
    `-- test
        |-- espresso
        |   `-- MainActivityTest.java
        `-- java
            `-- MainActivityTest.java
```

## .gitignore

```
#built application files
*.apk
*.ap_

# files for the dex VM
*.dex

# Java class files
*.class

# generated files
bin/
gen/

# Local configuration file (sdk path, etc)
local.properties

# Windows thumbnail db
Thumbs.db

# OSX files
.DS_Store

# Eclipse project files
.classpath
.project

# Android Studio
.idea
#.idea/workspace.xml - remove # and delete .idea if it better suit your needs.
.gradle
build/

# for older format

/*/out
/*/*/build
/*/*/production
*.iws
*.ipr
*~
*.swp
```
### Running Espresso Tests with Gradle

```
sudo add-apt-repository ppa:cwchien/gradle
sudo apt-get update
sudo apt-get install gradle # or gradle-1.10
```

Reference: http://askubuntu.com/a/328181/1476

File: `build.gradle`

```
buildscript {
	repositories {
		mavenCentral()
	}

	dependencies {
		classpath 'com.android.tools.build:gradle:0.9.+'
	}
}

apply plugin: 'android'

repositories {
	mavenCentral()
}

android {
	buildToolsVersion "19.0.3"
	compileSdkVersion 19

	dependencies {
		compile fileTree(dir: 'libs', include: ['*.jar'])
		compile 'com.android.support:appcompat-v7:19.+'
		androidTestCompile 'com.jakewharton.espresso:espresso:1.1-r3'
		androidTestCompile ('com.jakewharton.espresso:espresso-support-v4:1.1-r3') {
			exclude group:'com.android.support', module:'support-v4'
		}
	}

	sourceSets {
		main {
			manifest.srcFile 'AndroidManifest.xml'
			res.srcDirs = ['res']
		}
		androidTest {
			setRoot('src/test/java/')
			manifest.srcFile 'AndroidManifest.xml'
		}
	}

	lintOptions {
		abortOnError false
	} 

	defaultConfig {
		minSdkVersion 8
		targetSdkVersion 19
		testInstrumentationRunner "com.google.android.apps.common.testing.testrunner.GoogleInstrumentationTestRunner"
	}

	packagingOptions {
		exclude 'LICENSE.txt'
	}
}
```

Example espresso test

File: `./src/test/espresso/MainActivityTest.java`
```
package com.example.helloapp;

import android.test.ActivityInstrumentationTestCase2;
import android.test.suitebuilder.annotation.LargeTest;
import com.example.helloapp.MainActivity;
import com.example.helloapp.R;

import static com.google.android.apps.common.testing.ui.espresso.Espresso.onView;
import static com.google.android.apps.common.testing.ui.espresso.assertion.ViewAssertions.matches;
import static com.google.android.apps.common.testing.ui.espresso.matcher.ViewMatchers.withId;
import static com.google.android.apps.common.testing.ui.espresso.matcher.ViewMatchers.withText;

@LargeTest
public class MainActivityTest extends ActivityInstrumentationTestCase2<MainActivity> {
	public MainActivityTest() {
		super("com.example.helloapp", MainActivity.class);
	}

	@Override
	protected void setUp() throws Exception {
		super.setUp();
		getActivity();
	}

	public void testCreate() {
		onView(withId(R.id.text_view)).check(matches(withText(R.string.hello)));
	}
}
```
Ensure `local.properties` has correct android sdk path. For example,
`sdk.dir=/opt/android-studio/sdk`

Ensure from Android SDK Manager that the version of Build Tools is
installed as mentioned in `build.gradle` file. (in this example it is
19.0.3)

`gradle build`

If successful,

Start up the emulator `genymotion` or `emulator avd <emulator-name> -qemu -m 512
-enable-kvm`

Now is the time to run 

`gradle connectedAndroidTest`

If there is any error, try `adb logcat`

### Running Robolectric Tests with Gradle

build.gradle includes espresso and robolectric   

File: `build.gradle`
```
buildscript {
	repositories {
		mavenCentral()
	}

	dependencies {
		classpath 'com.android.tools.build:gradle:0.9.+'
    classpath 'org.robolectric.gradle:gradle-android-test-plugin:0.9.4'
	}
}

apply plugin: 'android'
apply plugin: 'android-test'

repositories {
	mavenCentral()
}

android {
	buildToolsVersion "19.0.3"
	compileSdkVersion 19

	dependencies {
		compile fileTree(dir: 'libs', include: ['*.jar'])
		compile 'com.android.support:appcompat-v7:19.+'
		androidTestCompile 'com.jakewharton.espresso:espresso:1.1-r3'
		androidTestCompile ('com.jakewharton.espresso:espresso-support-v4:1.1-r3') {
			exclude group:'com.android.support', module:'support-v4'
		}
	}

	sourceSets {
		main {
			manifest.srcFile 'AndroidManifest.xml'
			res.srcDirs = ['res']
		}

		androidTest {
			setRoot('src/test/java/')
			manifest.srcFile 'AndroidManifest.xml'
		}
	}

	lintOptions {
		abortOnError false
	}

	defaultConfig {
		minSdkVersion 8
		targetSdkVersion 19
		testInstrumentationRunner "com.google.android.apps.common.testing.testrunner.GoogleInstrumentationTestRunner"
	}

	packagingOptions {
		exclude 'LICENSE.txt'
		exclude 'META-INF/LICENSE'
		exclude 'META-INF/LICENSE.txt'
		exclude 'META-INF/NOTICE'
	}
}

androidTest {
  include '**/*Test.class'
  exclude '**/espresso/**/*.class'
}

dependencies {
  repositories {
    mavenCentral()
      maven {
        url 'https://oss.sonatype.org/content/repositories/snapshots/'
      }
  }

  androidTestCompile('junit:junit:4.11') {
    exclude module: 'hamcrest-core'
  }

  androidTestCompile('org.robolectric:robolectric:2.3') {
    exclude module: 'classworlds'
    exclude module: 'maven-artifact'
    exclude module: 'maven-artifact-manager'
    exclude module: 'maven-error-diagnostics'
    exclude module: 'maven-model'
    exclude module: 'maven-plugin-registry'
    exclude module: 'maven-profile'
    exclude module: 'maven-project'
    exclude module: 'maven-settings'
    exclude module: 'nekohtml'
    exclude module: 'plexus-container-default'
    exclude module: 'plexus-interpolation'
    exclude module: 'plexus-utils'
    exclude module: 'wagon-file'
    exclude module: 'wagon-http-lightweight'
    exclude module: 'wagon-http-shared'
    exclude module: 'wagon-provider-api'
  }

  androidTestCompile 'com.squareup:fest-android:1.0.+'
}
```

Sample robolectric test  
File: `./src/test/java/MainActivityTest.java`

```
package com.example.helloapp.gradletest;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.robolectric.Robolectric;
import org.robolectric.annotation.Config;
import org.robolectric.RobolectricTestRunner;
import com.example.helloapp.MainActivity;

import static org.junit.Assert.assertTrue;

@Config (emulateSdk = 18) //Robolectric support API level 18,17, 16, but not 19
@RunWith(RobolectricTestRunner.class)
public class MainActivityTest {
      @Test
      public void testCreate() throws Exception {
        MainActivity activity = Robolectric.buildActivity(MainActivity.class).create().get();
        assertTrue(activity != null);
      }
}
```

Verify build and run robolectric tests with: `gradle build`

Run tests with: `gradle test` or `gradle clean test`


Report is available at the following location. Nice report to view on browser   
`file:///home/ragu/code/android-dryrun/build/test-report/debug/index.html`
