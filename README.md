# Steps to build rpm package in Linux environment

## First create the spec file to buld the rpm

```shell
###############################################################################
# Spec file for utils
################################################################################
# Configured to be built by user student or other non-root user
################################################################################
#
Summary: Utility scripts for testing RPM creation
Name: utils
Version: 1.0.0
Release: 1
License: GPL
#URL: http://www.test1.org
Group: System
Packager: SS
Requires: bash
Requires: screen
#Requires: mc
Requires: dmidecode
#BuildRoot: ~/rpmbuild/
BuildRoot: /data/projects/sakti/rpmbuild/

# Build with the following syntax:
# rpmbuild --target noarch -bb utils.spec

%description
A collection of utility scripts for testing RPM creation.

%prep
################################################################################
# Create the build tree and copy the files from the development directories    #
# into the build tree.                                                         #
################################################################################
echo "BUILDROOT = $RPM_BUILD_ROOT"
mkdir -p $RPM_BUILD_ROOT/data/projects/sakti/temp_RPM_1
mkdir -p $RPM_BUILD_ROOT/data/projects/sakti/temp_RPM_2

cp /data/projects/sakti/*.log $RPM_BUILD_ROOT/data/projects/sakti/temp_RPM_1
cp /data/projects/sakti/*.py $RPM_BUILD_ROOT/data/projects/sakti/temp_RPM_2

exit

%files
%attr(0744, sakti, sakti) /data/projects/sakti/temp_RPM_1/*
%attr(0644, sakti, sakti) /data/projects/sakti/temp_RPM_2/*

%pre
echo "this is \%pre-install part"

%post
echo "this is \%post install part"

pwd
ls -ltr /data/projects/sakti/temp_RPM_*/*

%postun
echo "this is \%postun part to uninstall the package"

%clean
echo "this is \%clean part"
rm -rf $RPM_BUILD_ROOT/data/projects/sakti/temp_RPM_1
rm -rf $RPM_BUILD_ROOT/data/projects/sakti/temp_RPM_2

%changelog
* Fri Dec 28 2018 sakti <email@id.com>
 - This the first build of rpm
   initia comments
```

## %description
The %description section of the spec file contains a description of the rpm package. It can be very short or can contain many lines of information.

## %prep
The %prep section is the first script that is executed during the build process. This script is not executed during the installation of the package.

This script is just a Bash shell script. It prepares the build directory, creating directories used for the build as required and copying the appropriate files into their respective directories. This would include the sources required for a complete compile as part of the build.

The $RPM_BUILD_ROOT directory represents the root directory of an installed system. The directories created in the $RPM_BUILD_ROOT directory are fully qualified paths, such as /data/projects/sakti/temp_RPM_1, /data/projects/sakti/temp_RPM_2, /usr/local/bin, and so on, in a live filesystem.

## %files
This section of the spec file defines the files to be installed and their locations in the directory tree. It also specifies the file attributes and the owner and group owner for each file to be installed. The file permissions and ownerships are optional. Directories are created as required during the installation if they do not already exist.

## %pre
This section is empty in our lab project’s spec file. This would be the place to put any scripts that are required to run during installation of the rpm but prior to the installation of the files.

## %post
This one runs after the installation of files. This section can be pretty much anything you need or want it to be, including creating files, running system commands, and restarting services to reinitialize them after making configuration changes.
like pwd, ls in our case to veryfy the files and currentdirectory just for info in this case.
## %clean
This Bash script performs cleanup after the rpm build process. The two lines in the %clean section below remove the build directories created by the rpmbuild command.
```shell
rm -rf $RPM_BUILD_ROOT/data/projects/sakti/temp_RPM_1
rm -rf $RPM_BUILD_ROOT/data/projects/sakti/temp_RPM_2
```
## %changelog
This optional text section contains a list of changes to the rpm and files it contains. The newest changes are recorded at the top of this section.
```shell
%changelog
* Fri Dec 28 2018 sakti <email@id.com>
 - This the first build of rpm
   initia comments
```
## Build RPM command
```shell
rpmbuild --target noarch -bb utils.spec
```
Check in the ~/rpmbuild/RPMS/noarch directory to verify that the new rpm exists there.

## Test the RPM installation
As root, install the rpm to verify that it installs correctly and that the files are installed in the correct directories.
```shell
rpm -ivh utils-1.0.0-1.noarch.rpm
```

## verify installed package using below command
```shell
rpm -q --changelog utils
```
or
```shell
rpm -ql utils
```
or
```shell
rpm -qi utils-1.0.0-1.noarch
```
## Remove the rpm package
```shell
rpm -e utils-1.0.0-1.noarch
```
