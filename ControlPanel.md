# Android Notification Control Panel

Passengers want easy accessibility to switches and toggles that controls their
environment on an aircraft right from where they are sitting. What if we are
able to do that using the tablet on each passenger's seat? Could we find an
easy way to customize it for each airline's preferences?

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
  * Remote View
  * Notification Builder
  * Service

* * *

### Demo

![ControlPanel1](/demo/ControlPanel/ControlPanel1.png) | ![ControlPanel2](/demo/ControlPanel/ControlPanel2.png)

### Approach and Backend Explanation

We approached this problem by creating a remote view that can be attached to
Android's notification bar as a service. The notification is then programmed so
that it would always be at the top and cannot be cleared. This made it so
the user can access this control panel on their device, at any time, regardless
of application they are running on their machine.

To do this, the notification attachment is linked to a service that is started
automatically at boot up. When a user presses any of the button on the notification,
it will trigger an intent in the service, which can be programmed to do various
tasks such as changing volume, calling attendant, toggling reading lights, etc.

The neat thing about this is that we only need to install the APK for the service
to run. If we were to customize it for different airlines, we would only need
to create a new APK and install it on that airline's machine. This removes the
necessity to change the core code of the in-flight entertainment system to fit
the need of each individual airline.
