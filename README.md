This is a fork of the old project aimed at building/using it with more modern versions of linux (to the extent that's possible).

So far I have:
- modifiedto the Makefile to support crosscompiling
- setup a workflow to build PowerPC and Mipsel binaries

I have not yet confirmed these binaries work with any devices.

# AVR Event Daemon (old PPC-EVTD)

Copyright © 2006	Bob Perry <lb-source@users.sf.net>
Copyright © 2008-2015	Rogério Brito <rbrito@users.sf.net>

> This program is free software; you can redistribute it and/or modify it
> under the terms of the GNU General Public License as published by the
> Free Software Foundation; either version 2, or (at your option) any
> later version.
>
> This program is distributed in the hope that it will be useful,
> but WITHOUT ANY WARRANTY; without even the implied warranty of
> MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
> GNU General Public License for more details.
>
> You should have received a copy of the GNU General Public License
> along with this program; if not, write to the Free Software
> Foundation, Inc., 675 Mass Ave, Cambridge, MA 02139, USA.

----

# Usage

The `avr-evtd` daemon is aimed at the Buffalo series of Linkstation and
Kurobox.  This replaces the existing `ppc_uartd` daemon.  The daemon
configures the AVR and provides the necessary 'keep-alives' to the AVR
watchdog timer.

The daemon also checks that the `/mnt` and `/` filesystems are within scope
(disk space remaining) and also monitors the power button and the 'red'
reset button.  These button events are turned into requests into the
`EventScript`.  This allows a user to control what occurs (system wise) when
an event is received.  The same script is also used by the timer shutdown
process.

# System Requirements

Linkstation or equivalent.  Either stock or custom kernel and either
standard or Debian distribution.

# Installation

If you have the GCC tools installed, rebuild and install the process by
typing:

    make
    make install

Remove the current start and shutdown symbolic links for the
`ppc_uartd`:

    update-rc.d -f ppc_uartd remove

Create the new symbolic links:

    update-rc.d avr-evtd start 12 2 . stop 95 0 6 .

This daemon can use either the existing melco provided files or a custom
configuration file.  This custom file should exist within the `/etc/default`
directory.  Just edit the sample file contained there and move:

    mv avr-evtd.sample avr-evtd

# Credits

The Linkstation and Kuro communities.

# KNOW ISSUES

None known, but there are probably issues with the code, as it has survived
changes in the maintenance.

It has been tested on Kuroboxes with PowerPC processors, Linux 2.6, and
Debian as the distribution. Reports of bugs are welcome.

----

# This is Free Software for Linkstation/Kuro fans and lovers!

Please just mention our name if modified or linked to.
Bob Perry (UK)		June 2006.
Rogério Brito (BR)	May 2015.


----

# Old contents of README.IMPORTANT

Following major changes with the daemon and the support scripts, some of
the functions of the reset and power buttons have been changed.  These
mostly affect the reset button.  The reset feature has been removed (as
power button can perform this feature) and instead this can be used to
selected EM-Mode (HDD diaster recovery feature of the LS/Kuro). If the
reset button is held for at least 20 seconds then a message is written
to the flash memory and a reboot is issued.  At this point, the box is
now in EM-Mode as it has booted from ram flash and not HDD image.

PLEASE NOTE, THIS IS FOR USE ONLY BY THOSE WHO ARE AWARE OF THE ISSUES
OF ENTERING EM-MODE.  DO NOT USE OTHERWISE YOUR HDD DATA IS AT RISK.
