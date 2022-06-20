![alt text](https://cdn.glitch.com/13771a9f-c66b-4a73-87e9-b9fe0f4bb542%2Fsamsunginternet-logo.png?v=1629113116392 "Samsung Internet Logo")

# The folding webcam

A webcam that detects the folded posture and change its layout accordingly to improve user's experience.

## Getting started

This is a webcam created by Samsung Internet to showcase the device posture API on a Samsung Galaxy Flip using WebRTC. 

### Folding Posture

Folding devices are here! These smartphones allow having multiple form factors with just one device which can lead to new opportunities for innovation.
As developers should be able to detect what is the current posture of the device, Samsung is putting an effort within the W3C on making a new web standard called “Device Posture API” , 
this allows web applications to request and be notified of changes of the posture of a device. Web Apps can take advantage of the new form factors and improve user's experience.

### Prerequisites

- Basic knowledge of HTML5, CSS and Javascript
- Latest version of Samsung Internet Web Browser
- For testing: A foldable device / [Samsung Remote Test Lab Account](https://developer.samsung.com/remote-test-lab) / [Device Posture API Polyfill](https://github.com/w3c/device-posture/tree/gh-pages/polyfill)

### What You'll Build

You will create a web app that uses your device camera/s to take pictures and changes it's layout depending on the device's posture.
You will be using:
- HTML5 and CSS modern features
- WebRTC
- Device Posture API

## Step by step Guide

### Set up

For this codelab, we recommend using Glitch

Open a new browser tab and go to https://glitch.com.
Click New Project, then Clone from Git Repo.
Clone https://github.com/SamsungInternet/camera-fold-starterkit

### Add HTML video markup

We always recommend to take a look at the files of your project before starting to code.
Let's inspect our HTML file. We will have two main sections, one is dedicated to our video camera and the other one to our video camera controls.
You can identify the video section as its contain the class "video-container". 
Complete the html markup adding the video element, width, height and id attributes included.


```html
    <div class="video-container">       
      <video id="video" width="1280" height="200" >
        Video stream not available.
      </video>
    </div>
```

Once the video markup it's added the html file is complete, besides this you can find:
- A <canvas> element into which the captured frames are stored, it keeps hidden since users dont need to see it. 
- An <img> element where the pictures will be displayed.
  
### Enable Front and Back Camera with facingMode
  
  Let's go to our javascript file, if you take a look at index.js, you'll find a startUp() function.
  Here is where:
  - We initialize most values
  - We grab elements references to use later
  - We set up our camera and img
  Let's start setting up our camera. For this codelab you will have access to both front and back camera, in order to do this you will
use facingMode, a DOMString that indicates the direction in which the camera producing the video track is.
  It's always a good practice to check if this function is available for use, add the following lines of code in startUp(), the exact place is
  indicated with comments.
  
  ```javascript
   let supports = navigator.mediaDevices.getSupportedConstraints();
    if (supports["facingMode"] === true) {
      flip_button.disabled = false;
    }
  ```

### Get streaming media video
  
  If you are able to use facingMode, in our function called capture, we will use this option to detect which is the current camera used
  and later send it to the flip button. Now, the camera should be activated, insert this block of code after retrieving defaultOpts.video value using facingMode.
  
  ```javascript
        navigator.mediaDevices
        .getUserMedia(defaultsOpts)
        .then(_stream => {
          stream = _stream;
          video.srcObject = stream;
          video.play();
        })
        .catch(error => console.error(error));
  ```
  
  In order to get the media stream, you call navigator.mediaDevices.getUserMedia() and 
  requesting a video stream, which returns a promise.
  The success callback receives a stream object as input. It is the <video> element's source to our new stream.
  Once the stream is linked to the <video> element, we start it playing by calling video.play().
  It's always a good practice to include the error callback too, just in case there's not a camera available or permissions are denied.
  
### Taking a look at Javascript 
  
  Our functionality is complete, but before the next step, let's review the rest of the javascript
  code:
  - There is an event listener video for the canplay event, this will check when the video playback begins. If it's the first time running,
  it will set up video attributes like width and height.
  - For the snip button, there is an event listener for click that will capture the picture
  - The flip button will be waiting for a click event to assign the flag about which camera is being used (front or back camera) within the variable shouldFaceUser 
  and initialize the camera again.
  - clearPicture() creates an image and it converts it to a format that will be displayed in the <img> element.
  - Finally takePicture() captures the currently displayed video frame, convert it into a PNG file, and display it in the captured frame box.
  
  
### Using the Device Posture API
  
By now you should have a working prototype of the web camera, video camera should be displayed and a picture should be taken by the snip button, showing 
  a preview in the small preview display. The flip button should also switch between front and back cameras. 
Now it's time to play around the layout and take advantage of the features available on a foldable device, in order to do that we
  will implement the Device Posture API that allow developers to detect what is the current position of the phone.
  As this example will be tested on a Galaxy Flip device, the device posture that you will look for is "book" to change the layout when it's flipped.
  
  ![alt text](https://cdn.glitch.com/6a5ee1f7-a3d2-4dcd-a8e2-a036213f3b06%2Fmultipleform.JPG?v=1629243387099 "Multiple Form Factors")
  
  
  In your CSS, apply the following media query:
  
  ```css
  @media (device-posture: folded)  {
  
   body {
     display: flex;
     flex-flow: column nowrap;
   }
   .video-container .camera-controls {
     flex: 1 1 env(fold-bottom);
   } 
  .msg {
    display:block;
    margin: 5em;
  }
  
  ```
  
  Using modern css features like display:flex and grid, we can change the layout of the body element and the elements with the class video-container and
  camera-controls when the phone is flipped.
  
  ### Testing
  
  #### 1- Enable the device posture API in your browser
  
  Whether you test on a real device or using the remote test lab, you need to enable the device posture API in Samsung Internet. First of all, make sure that you have
  the latest version of Samsung Internet, you can install the latest update directly from the Galaxy Store.
  Once installed, open the browser and inn the url bar, write *internet://flags" and look for the "Device Posture API", enable the toggle.
  
  #### 2- Testing in a real device
  
  If you have a real physical device, you can test it directly in your browser using the url that glitch provides you by clicking on "Show in a new window"
  menu. Just flipped your phone and you will see how the layout changes and you will even discover a hidden message!
  
  #### 3- Using remote test lab
  
  The other option if you dont have a physical device is using [Samsung Remote Test Lab](https://developer.samsung.com/remote-test-lab), you can choose the Galaxy Flip
  from the list of real devices and follow up the same instructions as you would in a real device! Just make sure to enable the device posture API and have the latest version
  of Samsung Internet, then just use the buttons provided by the Test Lab to flip your remote Galaxy Device!!
  
  #### 4- Implementing polyfill
  
  The [polyfill](https://w3c.github.io/device-posture/polyfill/demo.html) allows you to emulate this behavior on devices that do not have folding capabilities. 
  It helps you visualize how the content responds to the different angle and posture configurations. Just include sfold-polyfill.js directly into your code and
use the polyfill settings of the web component that will emulate the angle of the device and therefore it will change its posture.
As the polyfill has a previous version of the API just replace the media query with the following one.
```css
@media (screen-fold-posture: laptop) 
```
  
  
Congratulations!! You just created, designed and tested a web cam that changes its layout when a device is folding.
Please let us know how it went [here](https://docs.google.com/forms/d/e/1FAIpQLSeai6Bwqle_ANIdRxRPgN5gkc0rvyuvfbsHTt9rasiMqf9x0g/viewform), your feedback is really important as it will be part of the discussion in the W3C Devices and Sensors Group. Join the conversation!     
If you would like to learn more about it, here are some resources:
  - [Overview of Galaxy Z](https://developer.samsung.com/galaxy-z/blog.html)
  - [Web Responsive Design of Foldable Devices](https://medium.com/samsung-internet-dev/web-responsive-design-for-the-new-generation-of-foldable-devices-ee434ae0fcb5)
  - [The Device Posture API](https://github.com/w3c/device-posture)
   
  - [Video explainer](https://www.youtube.com/embed/SfKrjvsaOnU)
  
  
  
  
  ![alt text](https://cdn.glitch.com/13771a9f-c66b-4a73-87e9-b9fe0f4bb542%2Fzip.jpg?v=1629114185887 "Samsung Galaxy Flip")
  
  



