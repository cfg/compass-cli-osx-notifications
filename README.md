compass-cli-osx-notifications
=============================
Set of scripts to allow showing Growl/Notification Center notifications from the command line when using `compass watch`.
Minimally tested, with minimal error checking.

Basic Setup
-----------
1. Edit `sample.compass-notify.config` and save as `~/.compass-notify.config`
2. Symlink or copy `notify-compass` into `/usr/local/bin/` and `chmod +x notify-compass`
3. If you are using Growl:
    1. Symlink or copy `growlnotify` into `/usr/local/bin/` and `chmod +x growlnotify`
4. If you are using Growl or will be using the `subl://` URL handler with Notification Center (which is not recommended) then symlink or copy `handler-subl` into `/usr/local/bin` and `chmod +x handler-subl`

iTerm2 Configuration
--------------------
* Open Preferences > Profiles, and select the profile you wish to edit.
* Select the Advanced tab and Edit your triggers.
* Create a new trigger with the following settings:
    * __Regular Expression:__ `echo '\3' | /usr/local/bin/notify-compass '\2' '\1' "\2:\1"`
    * __Action:__ Run Command
    * __Parameters:__ `echo '\3' | /usr/local/bin/notify-compass '\2' '\1' "\2:\1"`

Known Issues
------------
* Filenames or SASS compilation errors containing single quotes in them will break the notification command. This is a limitation of iTerm2, which doesn't provide a method for manipulating any of the tched output to escape quotes.
* Because iTerm2 doesn't know the full path to the files, you have to hardcode the root directory in `~/.compass-notify.config`. I can't think of a good way to address this, and it's not a big deal for me.
* The included `compass-terminal-notifier` isn't signed with a valid code signing certificate, and probably won't work for you.
    * Use [terminal-notifier](https://github.com/alloy/terminal-notifier/) if you don't care about the icon of your Notifcation Center notifications
    * Sign the app yourself with `codesign -f -s com.your.cert.name /path/to/compass-terminal-notifier.app`
* Adding a custom URI scheme for `subl://` style links is a bit of a pain. You can write your own, or use [subl-handler](https://github.com/asuth/subl-handler), which works fine, but stays open in the tray which annoys me.
* Assuming you installed `handler-subl` in `/usr/local/bin/` you could use the following AppleScript for your handler:

```AppleScript
on open location this_URL
	do shell script "/usr/local/bin/handler-subl '" & this_URL & "'"
	quit
end open location
```