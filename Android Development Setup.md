## [Android Development Setup](id:tableOfContents)<a name="tableOfContents"></a>

[Android SDK](#androidsdk)

[Gradle](#gradle)

[Java JDK](#javajdk)

[.bash_login](#bashlogin)

[Android Studio](#androidstudio)

[Genymotion](#genymotion)

***
### [Android SDK](id:androidsdk)<a name="androidsdk"></a> 
Installing the [Standalone Android SDK](https://developer.android.com/sdk/index.html#Other) to a known directory lets you add the binaries (such as adb) to your path.
Download it and install it to your ~/android-sdk directory.

[back to top](#tableOfContents)

### [Gradle](id:gradle)<a name="gradle"></a> 
Installing [Gradle](https://gradle.org/downloads/) to a known directory lets you execute gradle from a terminal.
Download it and install it to your ~/gradle directory.

Speed up Gradle command line execution (doesn't seem to affect Android Studio unfortunately) by enabling the Gradle daemon process that stays in memory by adding this to your ~/.gradle/gradle.properties file; parallel project execution also helps if your project has a few subprojects:

	org.gradle.daemon=true
	org.gradle.parallel=true

[back to top](#tableOfContents)

### [Java JDK](id:javajdk)<a name="javajdk"></a> 
Download and install the latest [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
because the Android development tools depend on it.

[back to top](#tableOfContents)

### [.bash_login](id:bashlogin)<a name="bashlogin"></a> 
After installing all those pieces, you can put this into your ~/.bash_login file so everything can be run from a terminal:

	export ANDROID_HOME=~/android-sdk
	export PATH=$PATH:~/android-sdk/tools:~/android-sdk/platform-tools:~/gradle/bin:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:~/bin:.
	export JAVA_HOME=`/usr/libexec/java_home`

[back to top](#tableOfContents)

### [Android Studio](id:androidstudio)<a name="androidstudio"></a> 
Download and install the latest [Android Studio](https://developer.android.com/sdk/index.html)
because the Android development tools depend on it.

After you have installed Android Studio, download the [code style JAR file](https://github.com/Raizlabs/Raizlabs-Android-Style/blob/master/rz-android-java-code-style.jar) and then load it into Android Studio by doing File/ImportSettings.

To speed up the build process by 15 seconds, go into Android Studio's preferences and set Gradle to offline mode after you have compiled a project once (you'll have to disable offline mode if you want to update your Gradle project dependencies).

Using a RAM disk didn't speed up build times by more than a few seconds and the possibility of data loss doesn't seem worth it.

[back to top](#tableOfContents)

### [Genymotion](id:Genymotion)<a name="Genymotion"></a> 

[Genymotion](https://www.genymotion.com/#!/) is slightly faster than Android's AVD emulators though the x86 HAXM 
ones have gotten a lot faster.  To support push notifications and the APIs that Google has stuck into Play Services,
you have to install [Google Apps](https://gist.github.com/wbroek/9321145) by dragging the appropriate zip file into
Genymotion and then updating it a bunch of times so it no longer crashes.

[back to top](#tableOfContents)


