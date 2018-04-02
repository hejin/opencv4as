# opencv4as
use opencv 2.4.13.5 without opencv manager to demo opencv sample tutorial2.


steps as following:

1. install AS 3.1 (my env is Mac OS, Linux should be OK. Not checked with Windows)

2. download opencv4android 2.4.13.5 and unzip it

3. clone the code 

4. create 2 symlinks in code directories, assuming the code root directory is OPENCV4AS_ROOT and the opencv4android unzip directory 
is OPENCV_SDK:

4.1. ${OPENCV4AS_ROOT}/app/src/main/jniLibs     --> ${OPENCV_SDK}/sdk/native/libs

4.2. ${OPENCV4AS_ROOT}/app/src/main/cpp/include --> ${OPENCV_SDK}/sdk/native/jni/include

