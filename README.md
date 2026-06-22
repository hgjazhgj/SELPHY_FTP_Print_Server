# SELPHY FTP Print Server - user manual

This application is an independent third-party driver and is not affiliated with, endorsed by, or sponsored by Canon Inc.  

This is an Android app that starts an FTP server on your phone  
Whenever an image file is uploaded to the server, it will be sent to the printer and get printed  
It is strongly recommend to use this app instead of IPP because your photo will be transferred via CPNP, the Canon manufacturer-defined communication protocols, we will not introduce any unnecessary lossy encoding so you can get your photo printed as the best quality  

It is compatible with both CP and QX series of Canon SELPHY photo printers  
Test passed on CP1500 (firmware version 1.0.6.0) and QX20 (firmware version 1.0.0.0)  
The part of the app that communicates with the printer is based on my observations and analysis of the network communication process of the printers I actually purchased, There is no guarantee that the program will work properly for devices other than the two models mentioned above because I have not bought them and therefore cannot test them  

It is worth mentioning that,  
If you have a camera that supports FTP photo transfer, you can get your phone connected to both the camera and printer simultaneously via WLAN (when using QX20, usually WiFi connect to the printer and AP to let camera connect)  
You can make your modern digital camera works like an instant camera and obtain the printed photo within just ~50 seconds after pressing the shutter  

You can view a detailed user manual at <https://github.com/hgjazhgj/SELPHY_FTP_Print_Server>  
And there is a [video](https://www.bilibili.com/video/av116757494760088/) that shows the whole workflow, with ZH/EN/JA subtitles  

Any customer may request a full refund within 2 hours of purchase for any reason or no reason at all. Refund requests made within this period will be honored without question  
If you want a refund or have any other question when using this app, contact <hgjazhgj@gmail.com>  

## Usage

**This app is designed for make your digital camera work like Polaroid**  
But you can use any FTP client instead of "the camera" below  

### Setup

Usually you only need to do the setup once  
The network will automatically connect the next time you turn on all your devices  
But if your phone is already connected to another WiFi, you may need to select manually  

#### Printer side

##### QX

Turn on your printer and the WiFi switch on your phone  

If open the ink cartridge cover on your printer, you will see an QR code and a string printed on the back on the cover  
Assuming the content of the string is `ABCDEFGHI`  
Connect to the WiFi which SSID is `QX20-GHI`, password is `ABCDEFGHI`  

Launch the app and enter `192.168.0.1` as the printer IP address  
Press the `Refresh printer status` button in the app and confirm whether the paper size information has been read correctly  

##### CP

You can do it like QX without any other devices  

If your phone and printer is connected tu your home WiFI, you check the printer's IP at the `WiFi settings - Confirm Settings - IPV4 Address` page on your printer  

#### Camera side

##### QX

Turn on hotspot (WLAN AP) switch on your phone and connect your camera to your phone's AP  

You can check the phone's IP at the `Default gateway` item of the WiFi information page
Setup the FTP connection as belows:  

+ hostname: your phone's IP  
+ port: 2121  
+ username: anonymous  

Set the camera to capture photos in JPEG or RAW+JPEG format, if RAW+JPEG selected, also set only transfer JPEG in FTP settings  

It is recommended to set the JPEG file size to the minimum to speed up transmission  
It is also recommended to set a button to "send the currently displayed photo via FTP" on playback screen  

##### CP

You can do it like QX without any other devices  

If your phone and camera is connected to your home WiFI, you can check the phone's IP as the `Network Information` page on your phone  

### Running

Press the "send the currently displayed photo via FTP" button and the print should start in a few seconds  
That is the only thing you need to do so that your shooting process will not be disrupted  

The image you provide will be scaled proportionally to fill the entire page, with the relatively longer side cropped  
If the image you provide has the different orientation to the page size, it will be rotated 90 deg ccw before scaling  
Also, there will be some bleeding for borderless printing on printer side  
When taking photos for printing, keep the above points in mind when composing the shot  

## Upload from any FTP client

This application is essentially a server that accepts and prints images  
So you can upload your image from any FTP client  

You can upload any format supported by `BitmapFactory.decodeByteArray`, such as BMP, PNG, and JPEG  
However, if your image has the following dimensions and is in JPEG format, your image will be sent to the printer **as-is**, without any quality degradation due to secondary encoding  

| Paper size  | JPEG image dimension |
| ----------- | -------------------- |
| CP card     | 1040 x 668           |
| CP L        | 1100 x 1456          |
| CP postcard | 1232 x 1800          |
| QX square   | 848 x 826            |
| QX card     | 644 x 826            |

These dimensions match the printer's advertised DPI and the physical dimensions of the paper, and are the dimensions sent to the printer by the Canon official SELPHY Photo Layout application  
Therefore, I have reason to believe that this image data corresponds one-to-one with the printed pixel size  
*Although Canon claims that CP1500 has the DPI of 300, but trust me, **treat it as 287** (same value as QX20) to get the proper print size, especially editing your ID photos*  
*I spent a lot of paper and ribbon to verifying this*  
*e.g. If you want to print a 40mm line on CP L paper, draw `40/25.4*287=452px` on a 1100x1456 canvas*  
*e.g. The bleeding for CP postcard will be approximately `(1800/287*25.4-148)/2=5.65mm` on the long side and `(1232/287*25.4-1400)/2=4.51mm` on the short side*  

## Overcoat

The printer will print a transparent layer as an overcoat at the final step of printing  
The Canon official SELPHY Photo Layout application only allows you to choose from several preset options, but the hardware accepts any patterns  
You definitely want to have **full** control over your device, and this application empowers you to do so!  
It is really cool to see your name on the photo under certain lighting conditions  

When `Custom (pick an image)` option selected in the `Overcoat` spinner, you need to provide an image as the overcoat layer  
The image you provide will be scaled proportionally to the size described in the previous section, if the image aspect ratio does not match, cropping will occur  
If the image you provide has the different orientation to the page size, it will be rotated 90 deg ccw before scaling  
White pixels will be printed and gives the gloss surface texture  
Black pixels will be not printed and gives the matte surface texture  
Since the printer only accepts 0-1 data for overcoat, your image will be convert into grayscale and then dithered into a black and white image  

You can also select to draw current datetime in one of the corner on your selected overcoat pattern  

It is recommended to use lossless format like PNG when making your own overcoat pattern  
It is also recommended to use a bold font if you want yo show some text in your own overcoat pattern  

Any option other than `Custom (pick an image)` will be treated as `Gloss` when using QX printers  
When custom overcoat (or `Semi-gloss` or `Satin` device option when using CP printers) set, the print speed of the overcoat layer will be slower than default gloss  
