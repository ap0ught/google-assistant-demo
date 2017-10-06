# Google Assistant Demo for CHIP Pro

### What Is This?:

This is a demo project for running Google Assistant on a CHIP Pro Dev Kit. It uses the onboard microphones and wifi connectivity so that users can ask questions by saying, "Hey Google."

This demo requires the user to grant access to their Google Account. To set this up, users can connect CHIP Pro to desktop computer with a USB cable and access a specific website that is running on the device. This site allows users to configure the wifi settings and authorize CHIP Pro to access Google.

For ease of use, a pre-made CHP image is included that can be flash directly to the device so that no compiling is required. See the "Getting Strated" instructions below.

### Requirements:

 * One CHIP Pro Dev kit
 * Use of the included [external wifi antenna](https://docs.getchip.com/chip_pro_devkit.html#wifi-antenna) 
 * One speaker or a pair of headphones connected to the dev kit
 * A Windows, Mac, or Linux desktop computer with a web browser
 * At least one available wifi network
 * A Google Developer account (more details about this later)


### Getting Started:

The quickest way to use Google Assistant on your CHIP Pro is to download a pre-made Google Voice Assistant CHP file and use the [online web flasher](https://docs.getchip.com/chip_pro_devkit.html#flash-with-an-os) to copy it to your dev kit. Be sure to select the “Choose an image” button on the web flasher and direct it to the Google Assistant Demo CHP file.

Once you have flashed your CHIP Pro dev kit, simply connect a speaker or some headphones, reboot, and listen for the audio instructions.

The voice instructions will instruct you to attach the dev kit to your desktop computer with a USB cable and access a configuration web page. The address should either be http://192.168.81.1 or http://192.168.82.1. Make sure to wait for the audio instructions to tell you when it is ready for you to connect.

Once you access this web page, you will see an interface for connecting CHIP Pro to a nearby wifi network. It will also provide details on how to sign up for a Google Developer account if you have not done so already. The final step in the setup process is to then grant permission for CHIP Pro to access your Google account.

If the device has been successfully authorized with Google, the audio instructions and web interface will inform you that it is ready to use. The CHIP Pro dev kit has two onboard microphones. This means you can say, “hey google” or “OK Google” followed by a command. For example:

 * “Hey Google, what’s the weather like today?”
 * “OK Google, tell me a joke.”
 * “Hey Google, ask me a trivia question.”

### Building The Project From Source

This demo uses Docker to create a container that can easily be deployed to CHIP Pro with the Gadget Operating System. Follow these setup instructions to install any dependencies:

https://docs.getchip.com/gadget.html#set-up-gadget

After installing Docker and the Gadget tools on your desktop machine, you can build this project from the Google Assistant Demo directory:

```gadget build```

Unfortunately, this Docker container is slightly too large to use the normal "gadget deploy" method. Instead, we will use the "inject project" method to include this Docker container into a custom image that can be flashed to CHIP Pro.

First: You'll first need to clone this repository and follow the README instructions for building the Linux kernel:

```
cd /home/myName
git clone https://github.com/NextThingCo/Gadget-OS.git
```

From the Gadget OS directory, execute the inject project script and point it to the directory that contains the Google Assistant Demo source code. For example:

```
cd /home/myName/Gadget-OS
./scripts/inject project /home/myName/google-assistant-demo
```

Now make the Linux kernel, [put your CHIP Pro into FEL mode](https://docs.getchip.com/chip_pro_devkit.html#flashing-process), connect it to your computer over USB, and flash it:

```
make
./scripts/flash-gadget
```

Once flashing has finished, reboot your CHIP Pro and listen for the audio instructions with headphones or a speaker.

### Including Credentials In A Custom Build

The default behavior for this demo is that users must use the web interface in order to give permission for the device to access their Google account.

However, you may choose to automatically authorize this device by including your credentials into the image that you flash to CHIP Pro. This means the demo will no longer require the Google Authentication procedure.

If you have already launched the Google Assistant Demo on a device and successfully ran through the authorization steps, you can copy the credential files to your machine. On CHIP Pro, these files will be in the following location:

* /data/.config

Now in the Gadget OS directory on your build machine, you can place the two JSON files from the .config directory into the root filesystem that will be flashed to your device. For example:

```
cp -r /myData/.config /home/myName/Gadget-OS/gadget/board/nextthing/chippro/overlay/opt/
```

So here, you would now have a folder inside "opt" called ".config", which contains the two JSON files for Google's authentication service.

Build your Gadget-OS like normal, flash it, and it should now include the credentials and authorize automatically once the device has connected to wifi.
