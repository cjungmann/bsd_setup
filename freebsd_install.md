# FreeBSD LiveCD Install

While installing FreeBSD from a LiveCD, the user will be asked
to enable and disable various security accommodations and for
a root password.

## Preparing FreeBSD for Remove Users

- Ensure the **sshd** option is enabled (it is enabled by default,
  don't disable it).

- Add a user when offered the opportunity (after the main install).
  Use default settings except for the **Name**, **Full Name**, and
  **Password** inputs.

  Non default inputs:
  - **Login Group**: set to *wheel* for **sudo** access

- After adding one or more users, there will be an opportunity
  to *open a shell in the new system to make any final manual
  modifications*.  Say **Yes** and enter the following command(s):

  - Install important packages:  
    `pkg install -y sudo bash git`

  - Enable root access for members of the **wheel** group.
    Doing it with **sed** avoids using a possibly unfamiliar text
    editor (**vi**) to edit sudoers with **visudo**:  
    `sed -i -e 's/# \(%wheel ALL=(ALL) ALL\)/\1/' /usr/local/etc/sudoers

  - Disable boot menu (optional):  
    `echo "autoboot_delay=\"2\"" >> /boot/loader.conf`  
    `echo "beastie_disable=\"YES\"" >> /boot/loader.conf`

  - Disable delay for search for missing usb drive:  
    `echo "hw.usb.no_boot_wait=1" >> /boot/loader.conf`

- I'll have to test this when I return from Kalamazoo, but I
  think at this point you just type:
  `reboot`  
  and when the computer is running, you should be able to
  connect to it using **ssh**.


