# Leveraging Microsoft AutoUpdate 3.18 "msupdate" binary with Jamf Pro 10 Patch Policies

While we're waiting for Jamf Pro 10's [Patch Management to support scripts](https://www.jamf.com/jamf-nation/feature-requests/6733/), here's one method to leverage [Microsoft AutoUpdate (MAU) version 3.18 msupdate command-line tool](https://www.jamf.com/jamf-nation/discussions/27872/) to update Office apps via Jamf Pro 10's built-in patch features, inspired by @pbowden-msft.

---

## Background

After testing @pbowden-msft's [MSUpdateHelper4JamfPro.sh](https://github.com/pbowden-msft/msupdatehelper) script in our environment, my first thought was to use a payload-free package's post-install script to simply call a policy which ran Paul's script:

### Microsoft Office 2016 Update 1.0.0.pkg
#### Post-install Script
```
#!/bin/sh
## postinstall

pathToScript=$0
pathToPackage=$1
targetLocation=$2
targetVolume=$3

echo " "
echo "###"
echo "# Microsoft Office 2016 Update"
echo "###"
echo " "

/usr/local/bin/jamf policy -trigger microsoftUpdate -verbose

exit 0
```

After counseling with [ted.johnsen](https://www.jamf.com/jamf-nation/users/9966/ted-johnsen), he convinced me that a Patch Policy calling a standard policy was too much of a hack.

---

## Payload-free Post-install Script

Using Composer, create a payload-free package which contains the following post-install script. (In Composer, I named the package _Microsoft Office 2016 msupdate_.)

The post-install script will use the word after "Microsoft" in the pathToPackage variable as the application name to be updated (i.e., "Excel") and the word after "msupdate" in the pathToPackage variable as the target version number (i.e., "16.12.18041000").

In Composer, build the .PKG and in the Finder, manually duplicate and rename it based on the application to update and the desired version.

For example:
- Microsoft Word 2016 msupdate 16.12.18041000.pkg
- Microsoft Excel 2016 msupdate 16.12.18041000.pkg
- Microsoft PowerPoint 2016 msupdate 16.12.18041000.pkg
- Microsoft Outlook 2016 msupdate 16.12.18041000.pkg
- Microsoft OneNote 2016 msupdate 16.12.18041000.pkg

Add the packages to the [definitions](https://github.com/dan-snelson/Jamf-Pro-Patch-Definitions) of the Microsoft Office Patch Management Software Titles.

When the patch policies run, the post-install script will leverage the "msupdate" binary to apply the updates.

<<<<<<< HEAD
=======
---

## Microsoft Office 2019

>>>>>>> 61b2228c39d8becaa8140c219e748b9bb2d58913
8-Oct-2018: Updated for [Office 2019](https://github.com/dan-snelson/Jamf-Pro-Scripts/blob/master/Microsoft%20Office%202016%20msupdate%20Post-install/Microsoft%20Office%202019%20msupdate%20Post-install.sh)
