# opencv-android
Project that bundles the OpenCV4Android to distribute it as an .aar library to be used via Gradle
without the need to download the OpenCV Manager app on user's device.

# Usage
Add the code below in your root build.gradle file:

    allProjects {
        repositories {
            maven { url "https://jitpack.io" }
        }
    }
    
And in your app/module build.gradle add the implementation command:
    
    dependencies {
        implementation 'com.github.ramonrabello:opencv-android:{latest version}'
    }

Please check the library [latest release](https://www.github.com/ramonrabello/opencv-android/releases) that can be used. 

# Shrinking the release APK size
You will notice that, after the opencv-android lib had been
downloaded, the final size of your project will increase
drastically, up to __73 MB__. This happen because it was embedded with
all OpenCV `.so` files for each ABI architecture (x86, mips, armeabi, ...), as
you can see in the image below after using the APK Analyzer.

![Large APK size](https://github.com/ramonrabello/opencv-android/blob/master/images/large-apk-size.png)

A solution for that is to split APKs for
each ABI architecture. For this, just add the `splits` directive
inside your `build.gradle` of app module:

    splits {
        abi {
            enable true
            reset()
            include 'x86', 'x86_64', 'armeabi', 'armeabi-v7a', 'mips', 'mips64', 'arm64-v8a'
            universalApk false
        }
    }

This will instruct Gradle to build multiple APKs. So, after
split process, you can notice that the APK size
shrunk up to __8 MB__ as shown in the image below.

![Split APK size](https://github.com/ramonrabello/opencv-android/blob/master/images/split-apk-size.png)    

# Initializing OpenCV
Call `OpenCVLoader.initDebug()` to initialize OpenCV 
engine. A best practice is to call it inside your 
`Application`, like the code below (in Kotlin):

    class MyApplication : Application {
        override fun onCreate(){
            super.onCreate()
            OpenCVLoader.initDebug(){
                // put your initialization logic here
            }
        }
    }

Do not forget to add the bind the `<application> android:name` attribute
with your `Application` class. Finally, sync your project and wait to complete the build. 
If all went successfully, now you can begin to use all 
available OpenCV module classes like `Imgproc`, `Mat`, ...

# Disclaimer
This project was developed entirely by my own according
to previous experience in creating CV-based apps. I'm
not directly involved in OpenCV development and
maintenance.
    
# License
Copyright 2018 Ramon Rabello

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
