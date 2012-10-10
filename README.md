Description
===========

Setup a dev environment for Gitlab.

The final product contain all databases set up, working tests and all gems
installed.

Requirements
============

* [VirtualBox](https://www.virtualbox.org)
* [Vagrant](http://vagrantup.com)
* A few gems: bundler, chef, librarian
* Have the NFS packages installed (already there if you are using Mac OS, for Debian/Ubuntu: sudo apt-get install nfs-common)
* And some patience :)

Installation
============

Install the required gems:

```bash
$ gem install bundler
$ gem install vagrant
```

Clone the repository and install chef's necessary packages:

```bash
$ git clone https://github.com/gitlabhq/gitlab-vagrant-vm
$ cd gitlab-vagrant-vm
$ bundle install
$ librarian-chef install
```

Finally, you should be able to use:

```bash
$ vagrant up
```

You'll be asked for your password to set up NFS shares.

Once everything is done you can log into the virtual machine to run tests:

```bash
$ vagrant ssh
$ cd /vagrant/gitlabhq/
$ bundle exec rake gitlab:test
```

You should also configure your own remote since by default it's going to grab
gitlab's master branch.

```bash
$ git remote add mine git://github.com/me/gitlab-vagrant-vm.git
$ # or if you prefer set up your origin as your own repository
$ git remote set-url origin git://github.com/me/gitlab-vagrant-vm.git
```

Virtual Machine Management
==========================

When done just log out with `^D` and suspend the virtual machine

```bash
$ vagrant suspend
```

then, resume to hack again

```bash
$ vagrant resume
```

Run

```bash
$ vagrant halt
```

to shutdown the virtual machine, and

```bash
$ vagrant up
```

to boot it again.

You can find out the state of a virtual machine anytime by invoking

```bash
$ vagrant status
```

Finally, to completely wipe the virtual machine from the disk **destroying all its contents**:

```bash
$ vagrant destroy # DANGER: all is gone
```

Troubleshooting
==========================

Mounting NFS shared folders failed
----------------------------------

If after running `vagrant up` you get the following error:

```
[vagrant] Preparing to edit /etc/exports. Administrator privileges will be required...
sudo: /etc/init.d/nfs-kernel-server: command not found

****(ommited other output)

Mounting NFS shared folders failed. This is most often caused by the NFS
client software not being installed on the guest machine. Please verify
that the NFS client software is properly installed, and consult any resources
specific to the linux distro you're using for more information on how to
do this.
```

Some users report that this issue can be fixed by following directions from [Stackoverflow](http://stackoverflow.com/a/9719588/1567796).

If that doesn't help, you can skip this step by changing Vagrantfile config:

    config.vm.share_folder("v-root", "/vagrant", ".", :nfs => true)

to

    config.vm.share_folder("v-root", "/vagrant", ".", :nfs => false)

