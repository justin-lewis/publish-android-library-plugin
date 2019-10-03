# publish-android-library-plugin
To help developers to publish their library with short configuration

## Introduction
This repo contains gradle script which is used for publish android library to bintray
## Usage
*	Step 1: Please publish your library to your [github](https://github.com/) account
*	Step 2: Create your bintray account [here](https://bintray.com/).
*	Step 3: Create your bintray repository to store your library there (In bintray, your library is called package)
*	Step 4:  please add statements bellow to your project build.gradle inside buildscript

```gradle
buildscript {
    dependencies {
        classpath 'com.github.dcendents:android-maven-gradle-plugin:2.1'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.8.4'
    }
}
```

*	Step 5:  inside your library module, please declare variable and replace values with your own information as example bellow  (I use my [SwipeNavigationLayout](https://github.com/justin-lewis/swipenavigationlayout) library as an example for you) :

```gradle
ext {

    bintrayRepo = "CustomView"
    bintrayName = "me.justinlewiskeith.swipenavigationlayout"

    publishedGroupId = 'me.justinlewiskeith.view'
    libraryName = 'swipenavigationlayout'
    artifact = 'swipenavigationlayout'

    libraryDescription = 'A Custom Swipe Navigation Layout is layout which allows swipe to navigation or other action via listener handler'

    siteUrl = 'https://github.com/justin-lewis/swipenavigationlayout'
    gitUrl = 'https://github.com/justin-lewis/swipenavigationlayout'

    libraryVersion = '1.0.0'

    developerId = 'justinlewiskeith'
    developerName = 'Justin Lewis'
    developerEmail = 'lewiskeithjustin@gmail.com'

    licenseName = 'The Apache Software License, Version 2.0'
    licenseUrl = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
    allLicenses = ["Apache-2.0"]
}
```

*	Step 6: add your these statements to your ```local.properties``` file as bellow:
```gradle
bintray.user = your bintray username
bintray.apikey = your bintray API Key
```
By default your local.properties file is included in .gitignore, so don’t worry about your account information.

* Step 7: add this statement to the end of your library module:

```gradle
apply from: 'https://github.com/justin-lewis/publish-android-library-plugin/master/publish_lib_v1.gradle'
```
*	Step 8: let’s build your library and publish it to your bintray account please run this command in your ````terminal```` in your root of the project

```bash
./gradlew clean build install bintrayUpload
```
#### If you have any exception please check it in ``Note``

## How to call your libray in a project after publish to jcenter

To call your library to use in any projects please follow this rule:

```gradle
GROUP_ID:ARTIFACT_ID:VERSION
```
For example, I want to call my [SmartSearchView](https://github.com/Chivorns/SmartSearchView) to use in a project:

* GROUP_ID : ``me.justinlewiskeith.view``
* ARTIFACT_ID : ``swipenavigationlayout``
* VERSION : ``1.0.0``

### So in my application module, I put this statement inside ``dependencies``

```gradle
dependencies {
    // other dependencies
    compile 'me.justinlewiskeith.view:swipenavigationlayout:1.0.0'
}
```

## Note
* To avoid [lintOptions](https://developer.android.com/studio/write/lint.html) error when build your project, please put statement bellow in your modules (All modules):

```gradle
android {
    lintOptions {
        abortOnError false
    }
}
```
*	To avoid generating Javadoc throw any exceptions please add these statement to your project build.gradle inside ```allprojects```:

```gradle
allprojects {
    tasks.withType(Javadoc) {
        options.addStringOption('Xdoclint:none', '-quiet')
        options.addStringOption('encoding', 'UTF-8')
        options.addBooleanOption('Xdoclint:none', true)
    }
}
```
* To check deprecation and unchecked warning message please add this line to check it

```gradle
allprojects {
    gradle.projectsEvaluated {
        tasks.withType(JavaCompile) {
            options.compilerArgs << "-Xlint:deprecation"
            options.compilerArgs << "-Xlint:unchecked"
        }
    }
}
```

