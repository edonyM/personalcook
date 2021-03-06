## FEDUP(8)                                             fedup User Manual                                            FEDUP(8)

## NAME
       fedup - Fedora Upgrade tool

## SYNOPSIS
       fedup [OPTIONS] SOURCE

## DESCRIPTION
       fedup is the Fedora Upgrade tool.

       The fedup client runs on the system to be upgraded. It determines what packages are needed for upgrade and gathers
       them from the source(s) given. It also fetches and sets up the boot images needed to run the upgrade and sets up
       the system to perform the upgrade at next boot.

       The actual upgrade takes place when the system is rebooted, using the boot images set up by fedup. The upgrade
       initrd starts the existing system (mostly) as normal, lets it mount all the local filesystems, then starts the
       upgrade.

       When the upgrade finishes, it reboots the system into the newly-upgraded OS.

## OPTIONS
###   Required Arguments
       --product [cloud,server,workstation,nonproduct]
           Specify the Fedora flavor to target for upgrades. Required for updating pre-F21 systems.

###   Optional arguments
       -h, --help
           Show a help message and exit.

       -v, --verbose
           Print more info.

       -d, --debug
           Print lots of debugging info.

       --debuglog DEBUGLOG
           Write debugging output to the given file. Defaults to /var/log/fedup.log.

###   SOURCE
       These options tell fedup where to look for the packages and boot images needed to run the upgrade. At least one of
       these options is required.

       --device [DEV]
           Device or mountpoint of mounted install media. If DEV is omitted, fedup will scan all currently-mounted
           removable devices (USB disks, optical media, etc.)

               Note
               Fedora Live images cannot be used as install media!

       --iso ISO
           Installation image file.

       --network VERSION
           Online repos matching VERSION (a number or "rawhide")

       Multiple sources may be used, if desired.

###   Additional options for --network
       --enablerepo REPOID
           Enable one or more repos (wildcards allowed).

       --disablerepo REPOID
           Disable one or more repos (wildcards allowed).

       --addrepo REPOID=[@]URL
           Add the repo at URL. Prefix URL with @ to indicate that the URL is a mirrorlist.

       --instrepo REPOID
           Get upgrader boot images from the repo named REPOID. The repo must contain a valid .treeinfo file which points
           to the location of usable kernel and upgrade images.

           By default, fedup will ask https://mirrors.fedoraproject.org/ for the installable tree for the given
           --networkVERSION. The actual filename requested is:

           /metalink?repo=fedora-install-$releasever&arch=$basearch

           where $basearch is the system’s arch (i386, x86_64, etc.) and $releasever is the VERSION given with the
           --network option.

               Note
               You should only need to use this option if you’re testing fedup before the release is public.

###   Cleanup commands
       --resetbootloader
           Remove any modifications made to bootloader configuration.

       --clean
           Clean up everything written by fedup.

## EXAMPLES
       fedup --network 18

       Upgrade to Fedora 18 by downloading all needed packages and data from the Fedora mirror system.

       fedup --device --network 18

       Upgrade to Fedora 18 using install media mounted somewhere on the system, fetching updates from the network if
       needed.

## EXIT STATUS
       0
           Success.

       1
           Cancelled by user, failure writing files to disk, or other unknown error

       2
           Failed to download/copy files from the given SOURCE

       3
           RPM upgrade transaction test failed

## BUGS
       The --iso image must be on a filesystem listed in /etc/fstab.

## AUTHORS
       Will Woods <wwoods@redhat.com>



## fedup                                                   04/16/2015                                                FEDUP(8)
***P.S.***<br>
*defup is abandoned and replaced by dnf system-upgrade.*

*Please try dnf [`system-upgrade`](http://fedoraproject.org/wiki/Changes/DNF_System_Upgrades)*
