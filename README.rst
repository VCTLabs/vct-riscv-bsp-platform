=============================
 VCT riscv32/64 BSP Manifest
=============================

The various branches available here will configure the repo build
for the appropriate branches in each repository and clone them in the typical fashion,
ie, with either poky or oe-core as the parent directory and the additional metadata
layers underneath (as documented in the upstream setup).

The main branches build either the poky reference distro or oe-core with your choice
of distro.  Note that oe-core will build "distroless" by default, however, you can set
DISTRO = "vctlabs" in your local.conf if you want the mainline kernel to be the default.
See the riscv page on the `Gentoo wiki`_ and the `BeagleV`_ pages for more info.

.. _Gentoo wiki: https://wiki.gentoo.org/wiki/Project:RISC-V
.. _BeagleV: https://beagleboard.org/beaglev


machine variants
----------------

Current machine variants on master branch:

* beaglev-starlight-jh7100
* freedom-u540
* qemuriscv32
* qemuriscv64


Build branches
--------------

Select the build branch using the github branch button above, which will select the
correct manifest branches and BSP/metadata using the respective branches in this
repo as shown below.

Follow the steps in the readme for the branch you select, then use the same branch
name with the "repo init" command.  This will checkout the
corresponding branch HEAD commits on the configured branch for each repository.

You can get status and more using the ``repo`` command in the top level directory
once you've run the ``repo init`` and ``repo sync`` commands.  Try::

  $ repo info
  $ repo show
  $ repo status

See the default.xml file for repo and branch details; the following example is generic
so go back and select a branch from this repo.

Install the repo utility
------------------------

::

  $ mkdir ~/bin
  $ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
  $ chmod a+x ~/bin/repo

Download the BSP source
-----------------------

::

  $ PATH=${PATH}:~/bin
  $ mkdir riscv-bsp
  $ cd riscv-bsp
  $ repo init -u https://github.com/VCTLabs/vct-riscv-bsp-platform -b oe-kirkstone
  $ repo sync

At the end of the above commands you have all the metadata you need to start
building with poky and meta-oe on gatesgarth branches.

To start a simple image build for a riscv qemu target::

  $ cd oe-core
  $ source ./oe-init-build-env build-dir  # you choose name of build-dir
  $ ${EDITOR} conf/local.conf             # set MACHINE to qemuriscv32 (see above)
  $ bitbake core-image-minimal


You can use any directory (build-dir above) to host your build. The above
commands will build an image for riscv using the riscv BSP
machine config and the default yocto-linux kernel.

The main source code is checked out in the bsp dir above, and the build
output dir will default to oe-core/build-dir unless you choose a different
path above.

Source code
-----------

Download the manifest source here::

  $ git clone https://github.com/VCTLabs/vct-riscv-bsp-platform

Using Development and Testing/Release Branches
----------------------------------------------

Replace the repo init command above with one of the following:

For developers - kirkstone

::

  $ repo init -u https://github.com/VCTLabs/vct-riscv-bsp-platform -b oe-kirkstone

For intrepid developers and testers - master

Patches are typically merged into master-next and then are merged into master
after a testing and comment period. It’s possible that master has
something you want or need.  But it’s also possible that using master
breaks something that was working before.  Use with caution.

::

  $ repo init -u https://github.com/VCTLabs/vct-riscv-bsp-platform -b oe-master


