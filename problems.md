# Problems and Solutions

This is not meant to be an exhaustive troubleshooting guide.
It is only a record of problems I encountered and how I solved,
worked around, or learned to live with them.

## Using LiveCD on UEFI Computer

The problem presented as the computer freezing while loading
modules, usually `/etc/hostid` or `/etc/?entropy?` (or something
similar: I'll check when I get home).

After many BIOS configuration changes, I restored to factory
recommendations and made the following two changes:

1. **SATA = IDE**
   There are two possible settings, *achi* or *ide*, with the
   default being *achi*.  Use *ide* when booting the LiveCD.
   It seems only to be necessary with the LiveCD, and you can
   turn it back to *achi* after installation

2. **BOOT MODE=Windows 8 UEFI**  (Please confirm exact spelling
   when returned from Kalamazoo).  
   There is a /boot/EFI directory with the appropriate settings
   to run in UEFI mode.  There is no need to attempt the
   *legacy* setting, it won't work.

When these two BIOS settings were made, the LiveCD booted and
the OS could be installed.  You may restore the **SATA** setting
to **ACHI** if you want.

## Removing the Boot Loader Screen

I intend to run FreeBSD as a headless server, so I want to
minimize booting time to a minimum.  I don't want the server
to pause waiting for the non-existent user to select multi-user
mode.

The solution is to edit the `/boot/loader.rc` file, 

A further complication I found was that the file was opened
in read-only mode.  Using my preferred **emacs**, I toggled that
off with Ctrl-x Ctrl-q.  Then I could comment out the offending
line and successfully write the changes.