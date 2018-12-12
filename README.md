Android Dropbear 2018.76
=========

A script & source to cross-compile Dropbear SSH server/client for use on Android with password authentication.
As the 64-bit binaries don't seem to work reliably, this project is configured to compile 32-bit binaries
using a standalone Android NDK toolchain.

Generated binares will all be PIE (position indepedent executable) binaries as it is required on Android 5 (L/ollipop) and above.

If building for android < 4.1 then before building, issue:
```
export DISABLE_PIE=1
```

Building Dropbear for Android
----

The process consists of just 3 parts:  

1) Git clone this repo:   
```
git clone https://github.com/Geofferey/dropbear-android.git
```  

2) Change to the direcotry:  
```
cd android-dropbear
```

3) Run the build script:  
```
./build-dropbear-android.sh
```

Generated binaries will be outputted to ``{android dropbear repo directory}/target/arm``


Customizations
----

Much of the project is pre-configured with sane defaults, but if you'd like to customize the behavior of Dropbear for Android, here are a few tips.
1) To change/configure most options, look in and modify the following files as appropriate:  
	a) default_options.h  
	b) sysoptions.h  
	c) config.h  

For instance, to change the port Dropbear runs on or to change the default location in which Dropbear tries to generate keys, edit ``default_options.h`` and modify the respective values.  
  
Basic usage
----
Dropbear for Android adds a few special flags to Dropbear:  
- A: signifies Android mode and allows for password authentication in the absence of the ```crypt()``` lib
- G: allows us to specify the GID dropbear should run as  
- U: allows us to specify the UID dropbear should run as  
- N: specify the login username for the session  
- T: specify the authentication key for the session  

A typical usecase would be:  
```
./dropbear -d /path/to/dropbear_dss_host_key -r /path/to/dropbear_rsa_hostkey -p 10022 -P /path/to/dropbear.pid -R -A -N user -C password -U u0_aXX -G u0_aXX
```

The above command will run the Dropbear server with password authentication enabled for the user 'user' with password 'password' and will attempt to run as u0_aXX and in that group. More information can be found by issuing:  
  
```
./dropbear --help
````

Credits
----
Big thanks to mkj who has been maintaining Dropbear:  
https://github.com/mkj/dropbear


Thanks to NHellfire who's work made the process of getting 2018.76 up and running much easier:  
https://github.com/NHellFire/dropbear-android  


Thanks to jmfoy for the ```config.sub``` and ```config.guess``` files:  
https://github.com/jfmoy/android-dropbear


Thanks to wolfdude & serasihay @XDA for ```netbsd_getpass.c``` implementation:  
https://forum.xda-developers.com/nexus-7-2013/general/guide-compiling-dropbear-2015-67-t3142412/page2  
https://forum.xda-developers.com/nexus-7-2013/general/guide-compiling-dropbear-2016-73-t3351671  

Thanks to yoshinrt for ```openpty.patch``` fix:  
https://github.com/yoshinrt/dropbear-android

Another thank you to the various other repositories out there whose various approches helped lead to this completed project.
