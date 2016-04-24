---
layout: post
title: "OpenCV 3.1.0 Installation Error Note (Fixed)"
---

I'm going to install the OpenCV 3.1.0 contrib module in my ubuntu 15.10 laptop, so I have to build the entire OpenCV lib again.

With the bravery that I have sussecced many times, I just git clone the main tree on github, [OpenCV](https://github.com/Itseez/opencv) and [OpenCV contrib](https://github.com/Itseez/opencv_contrib). After a quick ccmake and cmake config, I began to build. `make -j4`

The error occurred.

<!-- excerpt -->

```
[ 54%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_gphoto2.cpp.o
Documents/opencv/modules/videoio/src/cap_gphoto2.cpp: In member function ‘void cv::gphoto2::DigitalCameraCapture::initContext()’:
Documents/opencv/modules/videoio/src/cap_gphoto2.cpp:325:66: error: invalid conversion from ‘void (*)(GPContext*, const char*, void*) {aka void (*)(_GPContext*, const char*, void*)}’ to ‘GPContextErrorFunc {aka void (*)(_GPContext*, const char*, __va_list_tag*, void*)}’ [-fpermissive]
     gp_context_set_error_func(context, ctxErrorFunc, (void*) this);
                                                                  ^
In file included from /usr/include/gphoto2/gphoto2-abilities-list.h:28:0,
                 from /usr/include/gphoto2/gphoto2-library.h:28,
                 from /usr/include/gphoto2/gphoto2.h:49,
                 from Documents/opencv/modules/videoio/src/cap_gphoto2.cpp:32:
```

invalid conversion? I thought that the reason might be that my gphoto2 is incompatible to OpenCV's. Then I do a quick search and find the [same problem in StackOverflow](https://stackoverflow.com/questions/33020197/open-cv-build-error)

So I remove `gphoto2-2.6` and follow the answer which prove my guess and install `libgphoto2-2.5.7` and `gphoto2-2.5.6` from source.

After running `ldconfig`, I confidently make again and go through the `cap_ghoto2.cpp` and error occur again in the dynamic lib `videoio`.

```
[ 75%] Building CXX object modules/video/CMakeFiles/opencv_test_video.dir/test/test_main.cpp.o
../../lib/libopencv_videoio.so.3.1.0: undefined reference to `gp_camera_autodetect'
collect2: error: ld returned 1 exit status
```

I want to have a commment on the solusion on StackOverflow, but I have not enough reputation to do so.

---

...

After one day, I have another try.

I use `dpkg --get-selections | grep gphoto` to find out what I have installed related to gphoto: 

`libgphoto2-6:amd64             install`

`libgphoto2-6` is still here after I remove `gphoto2-2.6` and install `gphoto2-2.5.7`. So, I remove all gphoto and follow the solution again. 
After gphoto installation I use `sudo ldconfig -v` instead. 

And I am now successfully build openCV 3.1.0 ~
