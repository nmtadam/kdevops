# kdevops

The master git repo for kdevops is:

  * https://github.com/linux-kdevops/kdevops

<img src="images/kdevops-trans-bg-edited-individual-with-logo-gausian-blur-1600x1600.png" width=250 align=center alt="kdevops logo">

kdevops provides a framework for automation for optimal Linux kernel development
and testing. It is intended to help you both be able to ramp up with any complex
Linux kernel development environment super fast, and to also let you ramp up
an entire lab for Linux kernel testing for a complex subsystem in a jiffy.

It makes use of local ansible roles and optionally lets you use vagrant for
different virtualization technologies or terraform in order to support any
cloud provider, if you're into that sort of stuff. Variability is provided
through the same variability language used in the Linux kernel, kconfig. It
is written by Linux kernel developers, for Linux kernel developers. The
project aims to enable support for all Linux distributions, and even has
some support for OS X.

## Quick kdevops demos

To give you an idea of the power and goals behind kdevops we provide a few
quick demos of what you can do below. More workflows will be added with time.
Additional documentation detailing how to get started as well as how to add new
workflows follows the quick demos.

### Start kernel hacking in just 4 commands

Configure kdevops to use bare metal, cloud or a local vm based solution, pick
your distribution of choice, enable the Linux kernel workflow, select target
git tree, and get up and running on a freshly compiled Linux git tree in just
4 commands:

  * `make menuconfig`
  * `make`
  * `make bringup`
  * `make linux`
  * `make linux HOSTS="kdevops-xfs-crc kdevops-xfs-reflink"` for example if you wanted to restrict running the above command only to the two hosts listed

### Start running fstests in 2 commands

To test a kernel against fstests, for example, if you enable the fstests
workflow you can just run:

  * `make fstests`
  * `make fstests-baseline`

Be sure to use CONFIG_KDEVOPS_WORKFLOW_DEDICATE_FSTESTS=y unless you know
what you are doing. Also if you are building Linux be sure to first run
`make fstests` prior to `make linux` if using vagrant, see this
[create_data_partition](playbooks/roles/create_partition/README.md)
documentation for the known issue and suggested fix targetted for future
kdevops v6.3.

### Start running blktests in 2 commands

To test a kernel against fstests, for example, if you enable the blktests
workflow you can just run:

  * `make blktests`
  * `make blktests-baseline`

Be sure to use CONFIG_KDEVOPS_WORKFLOW_DEDICATE_BLKTESTS=y unless you know
what you are doing.

### Runs some kernel selftests in a parallel manner

The Linux kernel has a set of sets under tools/testing/selftests which we
call "Kernel selftests". Read the [Linux kernel selftests documentation](https://www.kernel.org/doc/html/latest/dev-tools/kselftest.html).
Running selftests used to be fast back in the day when we only had a few
kernel selftests. But these days there are many kernel selftests. Part of
the beauty of Linux kernel selftests is that there are no rules -- you make
your rules. The only rules are at least expicitly mentioning a few targets
for Makefiles so that the overall selftests facility knows what target to
call to run some tests. Part of the complexity in selftests these days is
that due to the lack of rules, you may end up needing a bit of dependencies
installed on the target node you want to run the tests on. Kdevops will take
care of that for you, and so selftests support are added by each developer
which wants to help make this easier for users. Today there is support for
at least 3 selftests:

  * `make selftests`
  * `make selftests-baseline`

You can also run specific tests:
    
  * `make selftests-firmware`
  * `make selftests-kmod`
  * `make selftests-sysctl`

### Get a Linux CXL development environment going and test CXL in just 2 commands:

Using CXL today means you have to build qemu. kdevops supports building qemu
for you, and it will be done for you if you want to enable a CXL development
environment. To ramp up with CXL (other than bringup and the above linux target)
just run:

  * `make cxl`
  * `make cxl-test-probe`
  * `make cxl-test-meson`

## kdevops chat server

We have a public chat server up, for now we use discord:

  * https://bit.ly/linux-kdevops-chat

## Parts to kdevops 

It is best to think about kdevops in phases of your desired target workflow.
The first thing you need to do is get systems up. You either are going to
use baremetal hosts, use a cloud solution, or spawn local virtualized guests.

The phases of use of kdevops can be split into:

  * Bring up
  * Make systems easily accessible, and install generic developer preferences
  * Run defined workflows

![kdevops-diagram](images/kdevops-diagram.png)

---

# kdevops documentation

Below is kdevops' recommended documentation reading.

  * [kdevops requirements](docs/requirements.md)
  * [kdevops' evolving make help](docs/evolving-make-help.md)
  * [kdevops configuration](docs/kdevops-configuration.md)
  * [kdevops mirror support](docs/kdevops-mirror.md)
  * [kdevops first run](docs/kdevops-first-run.md)
  * [kdevops running make](docs/running-make.md)
  * [kdevops libvirt storage pool considerations](docs/libvirt-storage-pool.md)
  * [kdevops PCI-E passthrough support](docs/libvirt-pcie-passthrough.md)
  * [kdevops running make bringup](docs/running-make-bringup.md)
  * [kdevops example workflow: running make linux](docs/kdevops-make-linux.md)
  * [kdevops running make destroy](docs/kdevops-make-destroy.md)
  * [kdevops make mrproper](docs/kdevops-restarting-from-scratch.md)

# kdevops kernel-ci support

kdevops supports its own kernel continous integration support, so to allow
Linux developers and Linux distributions to keep track of issues present in
any of supported kdevops workflows and be able to tell when new regressions
are detected. Note though that kernel-ci for kdevops is only implemented on
a few workflows, such as fstestse and blktests. In order to support a kernel-ci
part of the hard task is to come up with what a baseline is, and in kdevops
style, be able go easily `git diff` and read a regression with one line
per regression. This requires a bit of time and work. And it is why some
other workflows do not yet support a kernel-ci.

Documentation for this follows:

  * [kdevops kernel-ci](docs/kernel-ci/README.md)


# kdevops organization

kdevops was put under the linux-kdevops organization to enable other developers
to commit / push updates without any bottlenecks.

# kdevops tests results

kdevops has started to enable users / developers to also push results for
tests. This goes beyond just collecting baseline rusults for known failures,
this aims to collect *within* all dmesg / bad log files for each test that
failed.

An arbitrary namespace is provided so to enable developers, part of the
linux-kdevops organization to contribute findings.

# Video presentations on kdevops or related

  * [https://youtu.be/9PYjRYbc-Ms](2022 - LSFMM - Challenges with running fstests and blktests )
  * [https://youtu.be/-1KnphkTgNg](2020 - SUSE Labs Conference - kdevops: bringing devops to kernel development)

# Underneath the kdevops hood

Below are sections which get into technical details of how kdevops works.

  * [Why vagrant is used for virtualization](docs/why-vagrant.md)
  * [A case for truncated files with loopback block devices](docs/testing-with-loopback.md)
  * [Seeing more issues with loopback / truncated files setup](docs/seeing-more-issues.md)
  * [adding a new workflow to kdevops](docs/adding-a-new-workflow.md)
  * [kconfig integration](docs/kconfig-integration.md)
  * [Motivation behind kdevops](docs/motivations.md)
  * [Linux distribution support](docs/linux-distro-support.md)
  * [Overriding all ansible role options with one file](docs/ansible-override.md)
  * [kdevops vagrant support](docs/kdevops-vagrant.md)
  * [kdevops terraform suppor - cloud setup with kdevops](docs/kdevops-terraform.md)
  * [kdevops local ansible roles](docs/ansible-roles.md)
  * [Tutorial on building your own custom vagrant boxes](docs/custom-vagrant-boxes.md)

License
-------

This work is licensed under the copyleft-next-0.3.1, refer to the [LICENSE](./LICENSE) file
for details. Please stick to SPDX annotations for file license annotations.
If a file has no SPDX annotation the copyleft-next-0.3.1 applies. We keep SPDX annotations
with permissive licenses to ensure upstream projects we embraced under
permissive licenses can benefit from our changes to their respective files.
Likewise GPLv2 files are allowed as copyleft-next-0.3.1 is GPLv2 compatible.
