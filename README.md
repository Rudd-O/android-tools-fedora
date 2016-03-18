# Update of Android tools for Fedora

The purpose of this repository is to collect the necessary build bits to produce an up-to-date
version of the Fedora packages that distribute ADB and fastboot.  The ones shipping with
Fedora at this time cause Android phones to get bricked when flashing new stock versions of
the Google Nexus images.  This updated package solves that problem, and is a drop-in
update to the existing `android-tools` package.

## How to build the source and binary RPMs

First, clone this repository somewhere locally.

Then verify that the `SOURCES/create-snapshot` script will download the desired `TAG=`
(see the top of the file after the comments).  After any needed edits, run the following
sequence of steps:

```sh
cd SOURCES
./create-snapshot
cd ..
```

That step gets you the sources in a tarball within that `SOURCES` directory.

Now, verify that the abbreviated Git commit hashes in the tarball's file name match the
ones mentioned at the top of the specfile in `SPECS`.  If they are different, update the
specfile values at the top of it, and also bump the date variable.

Then you can produce the proper buildable source RPM:

```sh
rpmbuild --define '_topdir ./' -bs SPECS/android-tools.spec
```

Now you can rebuild them altogether -- but do make sure to install the `BuildRequires`
dependencies if the following step fails:

```sh
rpmbuild --define '_topdir ./' --rebuild SRPMS/*.src.rpm
```

That's it.  After building, the `RPMS/` directory will contain a valid, functional
working RPM of the `android-tools` package, updated to the latest sources.
