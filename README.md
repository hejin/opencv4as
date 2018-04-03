# opencv4as
use opencv 2.4.13.5 without opencv manager to demo opencv sample tutorial2.

0. pre-condition
* install AS 3.1 (my env is Mac OS, Linux should be OK. Not checked with Windows)
* download opencv4android 2.4.13.5 and unzip it. 

1. use the project code
* clone the code 
* create 2 symlinks in code directories, assuming the code root directory is OPENCV4AS_ROOT and the opencv4android unzip directory is OPENCV_SDK:
  * ${OPENCV4AS_ROOT}/app/src/main/jniLibs     --> ${OPENCV_SDK}/sdk/native/libs
  * ${OPENCV4AS_ROOT}/app/src/main/cpp/include --> ${OPENCV_SDK}/sdk/native/jni/include

2. or build by yourself from scratch 
* start AS and create an android project, saying opencv4as here
* create the android application with 'basic activity' with C++ support. Just in case, choose 'c++11' in compiler compatibility options. All others just keep the defaults.
* create an android library in the project with the menu of 'New -> Module'. NOTE: u'd better set the library package name to be 'org.opencv', or else u have to make a lot of modifications in following steps. Assuming the name of the library created to be 'opencv'
* execute 'cp -Ra ${opencv4android SDK unzipped directory}/sdk/java/src/org/opencv/*  ${Your project root directory}/opencv/src/main/java/org/opencv/'
* execute 'cp ${opencv4android SDK unzipped directory}/sdk/java/res/values/attrs.xml  ${Your project root directory}/opencv/src/main/res/values'
* remove ${Your project root directory}/opencv/src/main/java/org/opencv/android/AsyncServiceHelper.java. and comment out its calling in ${Your project root directory}/opencv/src/main/java/org/opencv/android/OpenCVLoader.java
* click 'Sync Now' button in the library created
* choose App in menu of 'Project structure' and add dependencies for the app. select the type of dependency to be 'Module'. then choose the 'opencv' and click OK.
* try to build the whole project and see if anything wrong. 
* if everything is OK so far, continue. or else, go back to fix it.
* execute 'ln -s ${opencv4android SDK unzipped directory}/sdk/native/libs ${Your project root directory}/app/src/main/jniLibs'
* execute 'ln -s ${opencv4android SDK unzipped directory}/sdk/native/jni/include ${Your project root directory}/app/src/main/cpp/'
* open ${opencv4android SDK unzipped directory}/samples/tutorial-2-mixedprocessing/jni/jni_part.cpp, and copy&past the JNI function body to lib-native.cpp in ${Your project root directory}/app/src/main/cpp/. NOTE: pls modify the JNI function name according to the sample JNI_String().
* open and change the app layout file (refer the project's 'app/src/main/res/layout/content_main.xml'). It puts the cameraView component in and deleted the original string component.
* make necessary changes to '${Your project root directory}/app/src/main/java/${your_pkgname}/MainActivity.java'. The main work is to add opencv_java library initialiation logic in, and the video capture and transform(gray, edge and feature detection) logic.
* add CAMERA related permission grant in project 'app/src/main/AndroidManifest.xml' as following:
  *     <uses-permission android:name="android.permission.CAMERA"/>
  *     <uses-feature android:name="android.hardware.camera"/>
  *     <uses-feature android:name="android.hardware.camera.autofocus"/>
  *     <uses-feature android:name="android.hardware.camera.front"/>
  *     <uses-feature android:name="android.hardware.camera.front.autofocus"/>
* modify the CMakeLists.txt to add opencv related stuff(include directory and libs) in:
  * include_directories(${CMAKE_CURRENT_SOURCE_DIR}/app/src/main/cpp/include)
  * add_library( lib_opencv SHARED IMPORTED )
  * set_target_properties(lib_opencv PROPERTIES IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI}/libopencv_java.so)
  * target_link_libraries( # Specifies the target library.
  *                     native-lib
  *                     # Links the target library to the log library
  *                     # included in the NDK.
  *                     ${log-lib}
  *                     lib_opencv
  *                     )
* make and run!
