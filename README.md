# Ubuntu server installation to build a computer system for visual impaired person.

## Setting up a Ubuntu computer for a blind person.

## Download the Ubuntu Server iso file and copy to USB stick.

## Install Ubuntu Server 

## Install alsa-utils
The Advanced Linux Sound Architecture (ALSA) utilities. These 
utilities include a speaker-test, which is used to ensure the sound and 
speakers are functioning.

Install alsa-utils and then reboot for it to be implemented...

```bash
    $ sudo apt install alsa-utils 
    $ reboot
```

Test the sound. *speaker-test* will just output noise.
```bash
    $ speaker-test
```
For a more interesting test of a 1KHz sine wave to both front speakers...
```bash
    $ speaker-test -Dplug:front -c 2 -t sine f 1000
```
Note that later, when everything has been set up, the speaker-test will not 
run as permission will be denied.

## Install espeakup module.
The installation of the espeakup module also installs over twenty other 
modules. Among these installed dependency modules is espeak. 
```bash
    $ sudo apt install espeakup
```
Towards the end of the installation the screen reader should commence 
speaking. It should now speak everything that you type on the console and 
speak the computer console response. If the numeric keypad's *Num Lock* is 
off, then the numeric keypad now provides cursor and speach review commands.

The [Speakup Users Guide](http://www.linux-speakup.org/spkguide.txt) 
describes the actions of the numeric keypads speakup review keys. 

You can at any time silence the current stream of speech being output with the 
keypads enter key. 

To toggle the speach on and off hold down the numeric keypads *0* key and press 
the numeric keypads *Enter* key.


## Enable startup module to be loaded on booting
The linux kernel does not include the speakup module, so it must be loaded.
Upon loading it will create the devices softsynth and synth. 

Change default to the /etc/modules-load.d directory and edit a new file which
is called speakup.conf...
```bash
    $ cd /etc/modules-load.d/
    $ sudo nano speakup.conf
```
Add the following two lines to the speakup.conf file and save...
```bash
    # This conf file is to load the speakup module on booting.
    speakup_soft
```
Check the contents of the new file...
```bash
    $ cat speakup.conf
    # This conf file is to load the speakup module on booting.
    speakup_soft
```

When the system has been rebooted perform the following check...
```bash
    $ ls /dev/
```
The list of devices will now include *softsynth* and *synth*

## Enable espeakup on reboot
Espeakup will be started by systemd on reboot, however it requires to enabled 
to do this, with these commands...
```bash
    $ sudo systemctl enable espeakup.service
    $ sudo systemctl daemon-reload
```
Perform a reboot and during the boot the speach should commence. 

## Tailoring the login messages
The default Ubuntu Server login messages can be a bit verbose and take too 
long to listen to. The unwanted messages may be moved to a *not_used* 
sub-folder, as follows...
```bash
    $ cd /etc/update-motd.d/
    $ sudo mkdir not_used
    $ sudo mv 10-help-text ./not_used/
```
This will remove the displaying on login of the Documentation, Management and 
Support links. To trim it down even further, you might want to...

```bash
    $ sudo mv 00-header ./not_used/
    $ sudo mv 50-motd-news ./not_used/
```

To remove the last login line. E.g. 
*Last login: Sun Oct 2017 on tty2*

```bash
    $ cd /etc/pam.d/
    $ sudo nano login
```
...then find...
```bash
session   optional   pam_lastlog.so
```
...and change it to...
```bash
session   optional   pam_lastlog.so   silent
```
If you want to modify line *Welcome to Ubuntu 17.04 
(GNU/Linux 4.10.0-35-generic x86_64)* then...
```bash
    $ sudo nano /etc/update-motd.d/00-header
```
...and, for example, change the line...
```bash
    printf "Welcome to %s (%s %s %s)\n" "$DISTRIB_DESCRIPTION" "$(uname -o)" "$(uname -r)"  "$(uname -m)" 
```
...to...
```bash
    printf "Welcome to %s (%s %s %s)\n" "$DISTRIB_DESCRIPTION"
```
...then the message will be *Welcome to Ubuntu 17.04*

## Adding alternative editors. For example joe
Ubuntu includes nano and vi text editors. To add the editor *joe*...
```bash
    $ sudo apt install joe
```

## Adding Email client
mutt?
