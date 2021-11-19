# BSD SETUP Project

Most of the contents of this repository were taken from my
[Raspberry PI project](https://github.com/cjungmann/raspi.git).
More recent work with FreeBSD on other hardware makes me realize
that this information should be hardware-agnostic.

Installing FreeBSD from a LiveCD or Memory Stick results in a
different set of installed options than the direct-to-SDCard
RaspberryPi version.  For example, the root signon and password
are set as part of the LiveCD setup, while on the RaspberryPi
version, *root* is builtin with *root* as password: you need
to change it before making it publicly available.

## Setup

The FreeBSD image will be lacking many utilities one normally
counts on being installed by default.  There are two stages to
setting up a new FreeBSD, the first stage is to configure and
install software for remote access to execute the second stage,
which is installing whatever packages are necessary for the
intended usage.


### Stage One: Command Line Setup

It's much more convenient to operate BSD remotely for several
reasons, including the comfort of a familiar workstation and,
perhaps more importantly, console emulators typically allow
you to scroll up to see lines that have scrolled out of view.
For these reasons, my first objective when setting up a new
system is to enable remote root access.

There are several things necessary for remote access:

1. A non-root account (FreeBSD discourages remote root login)
1. Install, if necessary, and enable sudo for the non-root account
1. Ensure there is an SSH daemon.




1. **Install git** to download these instructions so they are
   locally available:  
   `pkg install -y git`

1. **Clone these instructions**  
   `git clone https://github.com/cjungmann/bsd_setup`

1. **Prepare Root** with an appropriate password.  Note that for
   testing, it may be OK to use a simple or predictable password,
   but a publicly-available computer needs more robust protection.

   The LiveCD installation will ask for a root password as part
   of the installation process.  The RaspberryPi version includes
   the **root** user with a password of **root**.  **root** is
   not appropriate for a public-facing computer, and must be changed
   on the RaspberryPi distribution.  Use the **passwd** command.  
   `passwd`


1. **Prepare `sudo`** in order for a non-root user to access root
   privileges.  Be aware that, without modifications, the **root**
   user cannot login to FreeBSD through **ssh**.   These instructions
   assume that sudo-enabled users will be part of the group **wheel**,
   and will be granted root privileges based on the group.  Other
   configurations are possible, such as using a *sudo* group, explicit
   user privileges, etc.

   - `pkg install sudo` to install the **sudo** application.
   - Update sudoers with sed:  
     `sed -i -e 's/# \(%wheel ALL=(ALL) ALL\)/\1/' /usr/local/etc/sudoers`  
     Using **sed** sidesteps the need to call **visudo** with
     a **vi** editor.

1. **Create user for ssh** who will have remote root access.
   - `adduser` and answer the questions.  For our scenario,
     make sure the new user is a member of group **wheel**.

- **Remove Login Screen**  
  This doesn't apply to the RaspberryPi version.  You will need
  to edit the file `/boot/loader.rc` and comment-out or remove the
  line that mentions *beastie*.

  I was alarmed at first to find that **emacs** opened the file
  in read-only mode.  In **emacs**, simply toggle read-only mode
  with Ctrl-x Ctrl-q, then you will be able to edit and save the
  file.

- Install GNU development software:  
  `sudo pkg install lang/gcc gmake fcgi-devkit-2.4.0_5 texinfo wget`