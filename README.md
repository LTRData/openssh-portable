# Portable OpenSSH

## About this LTRData fork

This repository is an LTRData fork of [PowerShell/openssh-portable](https://github.com/PowerShell/openssh-portable), Microsoft's Windows port of [Portable OpenSSH](https://github.com/openssh/openssh-portable). It contains historical Windows OpenSSH source and LTRData-specific console and build changes.

### Branches and source versions

The default branch is **`latestw_all`**, but the later LTRData changes are on **`LTRData.openssh-portable-initial`**.

| Branch | Source baseline |
| --- | --- |
| [`latestw_all`](https://github.com/LTRData/openssh-portable/tree/latestw_all) (default) | February 2020 snapshot; `version.h` identifies OpenSSH for Windows 8.1p1. |
| [`LTRData.openssh-portable-initial`](https://github.com/LTRData/openssh-portable/tree/LTRData.openssh-portable-initial) | LTRData changes plus upstream merges through June 2021; `version.h` identifies OpenSSH for Windows 8.6p1. |
| [`latestw`](https://github.com/LTRData/openssh-portable/tree/latestw) | Older branch ending with the October 2018 OpenSSH 7.9 import. |

These are historical source snapshots; the branch names do not indicate that this fork is synchronized with current upstream. For current Windows source, see [PowerShell/openssh-portable](https://github.com/PowerShell/openssh-portable); Windows releases and documentation are maintained in [PowerShell/Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH).

### LTRData changes

The [changes on the LTRData branch relative to its last merged upstream revision](https://github.com/LTRData/openssh-portable/compare/75835a2462e1d8caf614cdb7011e45da929dc142...d9cb0dc3d0f1402af0333e02b6e64048472661af) include:

- Windows console state handling when GUI/console operations are unavailable, and restoration of raw input mode.
- Visual C++ project adjustments, including ARM configurations, delayed loading of `user32.dll`, and SDK, runtime and library path settings.
- Additional diagnostics for a missing X11 `xauth` program.

These changes belong to `LTRData.openssh-portable-initial`; they are not part of the default branch.

### Working with this source

To check out the LTRData changes explicitly:

```sh
git clone --branch LTRData.openssh-portable-initial https://github.com/LTRData/openssh-portable.git
cd openssh-portable
```

Windows projects and build scripts are under `contrib/win32/openssh`, including `Win32-OpenSSH.sln`, `OpenSSH-build.ps1` and `OpenSSHBuildHelper.psm1`. Review `paths.targets`, `README.txt` and the selected project's configuration on the branch you check out. These historical projects use configuration-specific Visual C++ toolsets, Windows SDKs and LibreSSL/zlib paths; the LTRData branch also contains local library search paths that may need adjustment for another checkout.

The Autoconf instructions below describe the Unix-like portable build. Their clone command selects the upstream repository. They are not instructions for building the native Windows projects.

Licensing and copyright notices are in [LICENCE](LICENCE) and the individual source files.

## Inherited upstream documentation

The remainder of this README is preserved from Portable OpenSSH. Its release, development and reporting links refer to upstream; current online manuals may describe options newer than the source in this fork.

---

OpenSSH is a complete implementation of the SSH protocol (version 2) for secure remote login, command execution and file transfer. It includes a client ``ssh`` and server ``sshd``, file transfer utilities ``scp`` and ``sftp`` as well as tools for key generation (``ssh-keygen``), run-time key storage (``ssh-agent``) and a number of supporting programs.

This is a port of OpenBSD's [OpenSSH](https://openssh.com) to most Unix-like operating systems, including Linux, OS X and Cygwin. Portable OpenSSH polyfills OpenBSD APIs that are not available elsewhere, adds sshd sandboxing for more operating systems and includes support for OS-native authentication and auditing (e.g. using PAM).

## Documentation

The official documentation for OpenSSH are the man pages for each tool:

* [ssh(1)](https://man.openbsd.org/ssh.1)
* [sshd(8)](https://man.openbsd.org/sshd.8)
* [ssh-keygen(1)](https://man.openbsd.org/ssh-keygen.1)
* [ssh-agent(1)](https://man.openbsd.org/ssh-agent.1)
* [scp(1)](https://man.openbsd.org/scp.1)
* [sftp(1)](https://man.openbsd.org/sftp.1)
* [ssh-keyscan(8)](https://man.openbsd.org/ssh-keyscan.8)
* [sftp-server(8)](https://man.openbsd.org/sftp-server.8)

## Stable Releases

Stable release tarballs are available from a number of [download mirrors](https://www.openssh.com/portable.html#downloads). We recommend the use of a stable release for most users. Please read the [release notes](https://www.openssh.com/releasenotes.html) for details of recent changes and potential incompatibilities.

## Building Portable OpenSSH

### Dependencies

Portable OpenSSH is built using autoconf and make. It requires a working C compiler, standard library and headers, as well as [zlib](https://www.zlib.net/) and ``libcrypto`` from either [LibreSSL](https://www.libressl.org/) or [OpenSSL](https://www.openssl.org) to build. Certain platforms and build-time options may require additional dependencies.

### Building a release

Releases include a pre-built copy of the ``configure`` script and may be built using:

```
tar zxvf openssh-X.Y.tar.gz
cd openssh
./configure # [options]
make && make tests
```

See the [Build-time Customisation](#build-time-customisation) section below for configure options. If you plan on installing OpenSSH to your system, then you will usually want to specify destination paths.
 
### Building from git

If building from git, you'll need [autoconf](https://www.gnu.org/software/autoconf/) installed to build the ``configure`` script. The following commands will check out and build portable OpenSSH from git:

```
git clone https://github.com/openssh/openssh-portable # or https://anongit.mindrot.org/openssh.git
cd openssh-portable
autoreconf
./configure
make && make tests
```

### Build-time Customisation

There are many build-time customisation options available. All Autoconf destination path flags (e.g. ``--prefix``) are supported (and are usually required if you want to install OpenSSH).

For a full list of available flags, run ``configure --help`` but a few of the more frequently-used ones are described below. Some of these flags will require additional libraries and/or headers be installed.

Flag | Meaning
--- | ---
``--with-pam`` | Enable [PAM](https://en.wikipedia.org/wiki/Pluggable_authentication_module) support. [OpenPAM](https://www.openpam.org/), [Linux PAM](http://www.linux-pam.org/) and Solaris PAM are supported.
``--with-libedit`` | Enable [libedit](https://www.thrysoee.dk/editline/) support for sftp.
``--with-kerberos5`` | Enable Kerberos/GSSAPI support. Both [Heimdal](https://www.h5l.org/) and [MIT](https://web.mit.edu/kerberos/) Kerberos implementations are supported.
``--with-selinux`` | Enable [SELinux](https://en.wikipedia.org/wiki/Security-Enhanced_Linux) support.

## Development

Portable OpenSSH development is discussed on the [openssh-unix-dev mailing list](https://lists.mindrot.org/mailman/listinfo/openssh-unix-dev) ([archive mirror](https://marc.info/?l=openssh-unix-dev)). Bugs and feature requests are tracked on our [Bugzilla](https://bugzilla.mindrot.org/).

## Reporting bugs

_Non-security_ bugs may be reported to the developers via [Bugzilla](https://bugzilla.mindrot.org/) or via the mailing list above. Security bugs should be reported to [openssh@openssh.com](mailto:openssh.openssh.com).
