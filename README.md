fedora-upgrade
==============

Upgrade Fedora to next version using dnf upgrade.

This is attempt to automatize steps as listed here:

https://fedoraproject.org/wiki/Upgrading_Fedora_using_yum


What works
==========

Please note that fedora-upgrade is NOT officially supported upgrade path.
That means it is not tested by Fedora QA before release and bugs in fedora-upgrade
are not blocker for release.
However I am aiming for official support - see https://fedoraproject.org/wiki/Features/FedoraUpgrade

Currently you can upgrade only to next version. You can upgrade from
* Fedora 19 -> Fedora 20 (removed in master)
* Fedora 20 -> Fedora 21 (removed in master)
* Fedora 21 -> Fedora 22
* Fedora 22 -> Fedora 23
* Fedora 23 -> Fedora 24
* Fedora 24 -> Fedora 25
* Fedora 25 -> Fedora 26
* Fedora 26 -> Fedora rawhide

You can *not* upgrade from older releases.
You can *not* skip release - e.g. upgrade from Fedora 16 to Fedora 18.


How it works
============

1. Display link to release notes and ask you if you really want to upgrade.
2. Check if dependencies are installed and install them. This step is needed only if you download script from GitHub. Dependencies are always present if you install fedora-upgrade as rpm package.
3. Resolve old .rpmsave and .rpmnew files using [rpmconf](https://github.com/xsuchy/rpmconf/). This step is optional and can be skipped. But it is better to start upgrade with clean state.
4. Download and install new GPG keys - including rpmfusion if you are using it.
5. Update dnf and clean all dnf metadata.
6. Upgrade system using dnf. In this or any previous step, you can hit Ctrl + C and interrupt upgrade and repeat it as many times you wish. After this step, you could not return (you can use back up, you created backup before upgrade, did you?).
7. Install packages from group 'Minimal Install'. It may happen that some new essential packages have been introduced to Fedora. This step will install them. You may however skip this step if you want to.
8. Resolve old .rpmsave and .rpmnew files using [rpmconf](https://github.com/xsuchy/rpmconf/). This step is optional and can be skipped. But it is better to finish upgrade with clean state.
9. Reset service priorities - the order of init scripts could have changed from the previous version. This steps is optional and can be skipped. And this does not affect services already migrated to systemD units.
11. Optionally report dead packages (packages that are no present in Fedora any more) and you have them installed from previous version.
12. Report success and suggest you reboot.

If there will be a problem during upgrade, this script will immediately stop and will not continue.
If you hit problem before step 6, you can run fedora-upgrade again after you resolve the problem.
If you hit problem in later stage, you can resolve the issue manually and open fedora-upgrade as it is just bash script. And try to finish manually. But most steps after step 6 are optional, so it should not affect stability of your system.

Example
=======

    # fedora-upgrade 
    Going to upgrade your Fedora to version 24.
    You may want to read Release Notes:
      http://docs.fedoraproject.org/release-notes/
    Hit Enter to continue or Ctrl + C to cancel.
    
    Going to run 'dnf upgrade' before upgrading.
    This step is highly recommended, but can be safely skipped.
    Hit Enter to continue, Ctrl + C to cancel or S + Enter to skip. 
    Last metadata expiration check: 2:35:57 ago on Mon Apr 25 16:05:04 2016.
    Závislosti vyřešeny.
    Není co dělat
    Hotovo!
    
    Going to resolve old .rpmsave and .rpmnew files before upgrading.
    This step is highly recommended, but can be safely skipped.
    Hit Enter to continue, Ctrl + C to cancel or S + Enter to skip. 
    Configuration file '/etc/mime.types'
    -rw-r--r--. 1 root root 57068 25. Sep  2015 /etc/mime.types.rpmnew
    -rw-r--r--. 1 root root 57618  9. Nov 09.45 /etc/mime.types
    
     ==> Package distributor has shipped an updated version.
       What would you like to do about it ?  Your options are:
        Y or I  : install the package maintainer's version
        N or O  : keep your currently-installed version
          D     : show the differences between the versions
          M     : merge configuration files
          Z     : background this process to examine the situation
          S     : skip this file
     The default action is to keep your current version.
    *** aliases (Y/I/N/O/D/M/Z/S) [default=N] ? 
    Your choice: i
    Choose upgrade method
      * offline - this use dnf-plugin-system-upgrade plugin and requires two reboots
                - this is official upgrade method
      * online  - this use distro-sync and require only one reboot
                - this is not offically tested by FedoraQA
    For more information see https://fedoraproject.org/wiki/Upgrading
    
    What is your choice? (offline/online) offline
    
    ********** End of DNF plugin output **********
    
    Download complete! The downloaded packages were saved in cache till the next
    successful transaction. You can remove cached packages by executing
    'dnf clean packages'.
    In next step your computer will be REBOOTED and packages will be upgraded.
    Hit Enter to continue, Ctrl + C to cancel or S + Enter to skip.
    

Note
====

If you are upgrading to branched development version then updates-testing is automatically enabled.

BUGS
====

See https://apps.fedoraproject.org/packages/fedora-upgrade/bugs

If you find bug, please [report](https://bugzilla.redhat.com/enter_bug.cgi?product=Fedora&version=rawhide&component=fedora-upgrade) it.


License
=======

GPLv2
