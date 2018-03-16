Android testing samples
===================================

A collection of samples demonstrating different frameworks and techniques for automated testing.

Prerequisites
--------------

- Android SDK v23
- Android Build Tools v23
- Android Support Repository rev17

Getting Started
---------------

These samples use the Gradle build system. To build a project, enter the project directory and use the `./gradlew assemble` command or use "Import Project" in Android Studio.

- Use `./gradlew connectedAndroidTest` to run the tests on a connected emulator or device.
- Use `./gradlew test` to run the unit test on your local host.

There is a top-level `build.gradle` file if you want to build and test all samples from the root directory. This is mostly helpful to build on a CI (Continuous Integration) server.

Android Testing Support Library
---------------
Many of these samples use the ATSL. Visit the [Android Testing Support Library site](https://google.github.io/android-testing-support-library/) for more information.

Experimental Bazel Support
--------------------------

Some of these samples can be built with [Bazel](https://bazel.build) on Linux. These samples contain a `BUILD.bazel` file, which is similar to a `build.gradle` file. The external dependencies are defined in the top level `WORKSPACE` file.

This is __experimental__ while testing features are being developed in Bazel. To run the tests, please use a development build of Bazel built from master:

```
$ git clone https://github.com/bazelbuild/bazel
$ cd bazel
$ bazel build //src:bazel # requires local installation of Bazel, please see https://docs.bazel.build/versions/master/install.html`
$ cp bazel-bin/src/bazel /tmp/bazel
```

Then, to run the tests:

```
$ git clone https://github.com/google/android-testing
$ cd android-testing

# Edit the path to your local SDK at the top of the WORKSPACE file
$ $EDITOR WORKSPACE

# Test everything in a headless mode
$ /tmp/bazel test //... --spawn_strategy=local

# Test a single test, e.g. ui/espresso/BasicSample/BUILD.bazel
$ /tmp/bazel test //ui/espresso/BasicSample:BasicSampleInstrumentationTest --spawn_strategy=local

# Test everything with GUI enabled
$ /tmp/bazel test //... --action_env=DISPLAY=$DISPLAY

# Test with a local device or emulator. Ensure that `adb devices` lists the device.
$ /tmp/bazel test //... --test_output=streamed --test_arg=--device_broker_type=LOCAL_ADB_SERVER

# If multiple devices are connected, add --device_serial_number=$identifier where $identifier is the name of the device in `adb devices`
$ /tmp/bazel test //... --test_output=streamed --test_arg=--device_broker_type=LOCAL_ADB_SERVER --test_arg=--device_serial_number=$identifier
```

For more information, check out the tutorial for [Building an Android App with Bazel](https://docs.bazel.build/versions/master/tutorial/android-app.html), and the list of [Android Rules](https://docs.bazel.build/versions/master/be/android.html) in the Bazel Build Encyclopedia.

Known issues:

* Headless mode is unstable in sandboxed mode due to Xvfb issues, so the flag `--spawn_strategy=local` is required.
* Building of APKs is supported on Linux, Mac and Windows, but testing is only supported on Linux.
* `android_instrumentation_test.target_device` attribute still needs to be specified even if `LOCAL_ADB_SERVER` is used.
* If using a local device or emulator, the APKs are not uninstalled automatically after the test. Use this command to
remove the packages:
    * `adb shell pm list packages com.example.android.testing | cut -d ':' -f 2 | tr -d '\r' | xargs -L1 -t adb uninstall`

Please file Bazel related issues against the [Bazel](https://github.com/bazelbuild/bazel) repository instead of this repository.

Support
-------

- Google+ Community: https://plus.google.com/communities/105153134372062985968
- Stack Overflow: http://stackoverflow.com/questions/tagged/android-testing

If you've found an error in this sample, please file an issue:
https://github.com/googlesamples/android-testing

Patches are encouraged, and may be submitted by forking this project and
submitting a pull request through GitHub. Please see CONTRIBUTING.md for more details.

License
-------

Copyright 2015 The Android Open Source Project, Inc.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements.  See the NOTICE file distributed with this work for
additional information regarding copyright ownership.  The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License.  You may obtain a copy of
the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
License for the specific language governing permissions and limitations under
the License.
