# Android Dynamic Video Preview

Passenger aircraft nowadays are filled with digital contents that are constantly
being swapped in and out. The goal of this project is to figure out a way to show
previews for any given media file as the user scrubs the seek bar without the need
to perform any pre-processing on the media file prior to loading onto the aircraft.

* * *

### Tools Used

* **Development Environment**
  * Ubuntu
  * Android
* **Development Device**
  * Pixel C
* **Programming Language**
  * Java
* **Android API**
  * Media Player
  * Media Controller
  * Media Metadata Retriever
  * Media Session
  * Notification Builder
  * Service

* * *

#### Demo

Multi-Preview + Remote Control | Single Preview
:-----------------------------:|:---------------:
![MultiPreview](/demo/VideoPreview/multi-preview.gif) | ![SinglePreview](/demo/VideoPreview/single-preview.gif)

#### Preview System Backend Explanation

Initially, we used Android's Media Metadata Retriever tool to grab frames and display
it as the user is scrubbing through the seekbar. This worked relatively well when we
are only displaying one preview and the media content is hosted locally. The lag time
increases noticeably when we start showing more preview frames and the media content
is moved to a remote server.

As a result, we decided to reduce the lag time by pre-loading all the 1000 preview
frames into an evenly spaced Bitmap array before allowing the user to start playing
the video. This process requires utilizing the Android's Media Metadata Retriever tool
to retrieve each of the 1000 preview frames, and then rescaling and reducing the quality
of each of the preview frame to fit into RAM.

This made scrubbing through the video on single and multi-preview extremely smooth
with no lag time at all. This was great! For a two hour media content hosted locally,
preloading all the previews takes roughly one minute. However, for the same content
hosted on a remote server, this could take up to three minutes. This would not suffice
in a real world environment, and therefore, we had to come up with a different approach
for tackling this scenario.

In our final version, we decided to only cache in 20 preview frames initially before
allowing the user to play the video. Once the initial 20 frames are cached and the
video is playing, the program will in the background fetch all of the remaining frames,
filling them in a manner similar to divide and conquer. This means if the initial frames
are 0, 20, 40, etc, the second round of fill in would do frame 10, 30, 50, etc, and
the third round would fill in frame 5, 15, 25, 25, etc. When the user scrubs, the
program will try to index directly into the preview array at that time using
*floor(seekTime / totalTime)*. If this is a miss, it'll index into the nearest filled
in frame index. This is also a constant time index because the program keeps track
of how many spacing are between frames that are currently filled in. This makes the
whole process of figuring out which preview frame to show for the current seek time a
constant runtime operation for both a hit and a miss.

Using this method, the user would only have to wait a few seconds before the video
starts. Then once it starts, they'll be able to scrub through the entire video,
hardly noticing the difference between whether the preview frames are still
loading or are already fully populated.

#### Notification Media Controller

The goal here is to be able to control the media player from outside of the media
player application, such as through the notification.

We were able to accomplish this using Android Media Session. Upon opening a new
media player, we can attach a media session to it. This media session would have a
token, which we can pass to the media style Android notification widget system.
Whenever a user hits play/pause/etc. on the notification media controller, it will trigger
a transport control callback which is linked to the media session, causing the
media player to react appropriately to whichever button the user presses.

To make sure that the notification media controller has the correct play/pause state
of the media player, we placed a wrapper around the video view that has a play/pause
listener. Whenever a user hits the play/pause button on the actual media player, it
will update the notification media controller accordingly.
