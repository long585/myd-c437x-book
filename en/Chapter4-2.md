###4.2 LCD Test
 
This example demonstrates the usage of Linux API for Linux framebuffer, users can use these API to paint points, lines and areas on LCD frame buffer. 
At the end of this section, demonstrate drawing a picture on LCD framebuffer with `fbv` application built by Buildroot.  

**Hardware Preparation: **    
  * One MYD-C437X-PRU development board     
  * MY-TFF070CV2 connects to J21 of the MYD-C437X-PRU development board    
  * One USB to TTL converter used to connect J25 of MYD-C437X-PRU development board and host PC, set the baudrate of serial port on  host PC to 115200-8-n-1

**Software Preparation:**    
  * Linux Kernel 4.1.18   
  * framebuffer_test application    
  * fbv application built by Buildroot  

**Test Steps: **    
  * Copy cross compiled `<WORKDIR>/Examples/framebuffer/framebuffer_test` to `/usr/bin` directory of the MYD-C437X-PRU development board, run `framebuffer_test` application as below:  

```
# chmod 777 /usr/bin/framebuffer_test
# framebuffer_test -h
Usage: framebuffer_test [options]

Version 1.0
Available options:
-d | --device name   framebuffer device name, default: /dev/fb0
-h | --help          Print this message

# framebuffer_test -d /dev/fb0
xres:800 >>> yres:480 >>> bpp:32>>>
```  

During `framebuffer_test` running, serval colors of background are painted on LCD one by one, and then colorful points, lines, areas are painted.   

  * Copy a BMP file with 32BPP and resolution of 800*480 to "/media/1.bmp" of the MYD-C437X-PRU development board, display the picture on LCD by fbv application:   

```
# fbv
Usage: fbv [options] image1 image2 image3 ...

Available options:
 --help        | -h : Show this help
 --alpha       | -a : Use the alpha channel (if applicable)
 --dontclear   | -c : Do not clear the screen before and after displaying the image
 --donthide    | -u : Do not hide the cursor before and after displaying the image
 --noinfo      | -i : Supress image information
 --stretch     | -f : Strech (using a simple resizing routine) the image to fit onto
 										  screen if necessary
 --colorstretch| -k : Strech (using a 'color average' resizing routine) the image to 
 											fit onto screen if necessary
 --enlarge     | -e : Enlarge the image to fit the whole screen if necessary
 --ignore-aspect| -r : Ignore the image aspect while resizing
 --delay <d>   | -s <delay> : Slideshow, 'delay' is the slideshow delay in tenths of seconds.

Keys:
 r            : Redraw the image
 a, d, w, x   : Pan the image
 f            : Toggle resizing on/off
 k            : Toggle resizing quality
 e            : Toggle enlarging on/off
 i            : Toggle respecting the image aspect on/off
 n            : Rotate the image 90 degrees left
 m            : Rotate the image 90 degrees right
 p            : Disable all transformations
Copyright (C) 2000 - 2004 Mateusz Golicz, Tomasz Sterna.
Error: Required argument missing.

# fbv /meida/1.bmp
fbv - The Framebuffer Viewer
/media/1.bmp
800 x 480
```  
After complete, the picture displays just right for the LCD.  






