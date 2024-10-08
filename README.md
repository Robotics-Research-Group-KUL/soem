# SOEM for ROS
__Note: for Ros2 use the foxy-devel branch__

```
git clone https://github.com/Robotics-Research-Group-KUL/python_gui_ros2.git $RTT_ROS_UTILITIES/src/python_gui \
```
**Table of Contents**

- [SOEM ROS Package Upgrade](#SOEM-ROS-Package-Upgrade)
- [Package Description](#Package-Description)
- [Installation](#Installation)
- [Usage](#Usage)
- [Development](#Development)

## SOEM ROS Package Upgrade
This package has been upgraded to the new Release of the **upstream SOEM repo v1.4.0**.
This upgrade brings not only the new release, but also changes to the `catkin` plumbing.
This allows you to use `soem` from within your regular ROS workspace.
So you don't need to specify `${soem_INCLUDE_DIRS}/soem` in the `include_directories` section of your package anymore.
The changes to the build system are backwards compatible.

If you experience problems with the new version, please try to revert to tag v1.3.0
and test if this solves your issues.

## Package Description

SOEM is an open source EtherCAT master library written in C.
Its primary target is Linux but can be adapted to other OS and embedded systems.

SOEM has originally been hosted at http://developer.berlios.de/projects/soem/
but has been moved to [GitHub and the OpenEtherCATsociety organisation](
https://github.com/OpenEtherCATsociety/SOEM).

This package contains the upstream SOEM repository as a git subtree and wraps it to be easily used within ROS.

**Disclaimer**:
This package is not a development package for SOEM, but rather a wrapper to provide SOEM to ROS.
In the end, this just provides the CMake quirks that allows releasing SOEM as a ROS package.

All bug reports regarding the original SOEM source code should go to the bugtracker at
https://github.com/OpenEtherCATsociety/SOEM/issues.

All ROS related issues should target the [bug tracker on GitHub](https://github.com/mgruhler/soem/issues)
(but might be redirected ;-)).

Obviously, any support, being it bug reports or pull requests (obviously preferred) are highly welcome!

## Installation

If `soem` has been released for your respective ROS distribution, you can simply install it using

```bash
sudo apt install ros-<DISTRO>-soem
```

Currently, `soem` has been released for ROS `indigo`, `kinetic` and `melodic`.
If you want to use `soem` from source, please check out the section about [Development](#Development).

## Usage

To use `soem` in your ROS package add the following to your `package.xml`and `CMakeLists.txt`, respectively.

In your `package.xml` add:

```xml
  <build_depend>soem</build_depend>
  <exec_depend>soem</exec_depend>
```

and in your `CMakeLists.txt`, add it to `find_package` and adapt the `include_directories` as shown:

```CMake
find_package(catkin REQUIRED COMPONENTS
  ...
  soem
  ...
)

include_directories(
  ...
  ${catkin_INCLUDE_DIRS}
)
```

### Running without sudo/root
SOEM requires access to certain network capabilities as it is using raw sockets, and as such any executable linking
against SOEM needs to be run with certain privileges.
Typically, you run any SOEM executables with `sudo` or as `root`.
Tis is impractical for any ROS system, and as such there exists a tool called
[`ethercat_grant`](https://github.com/shadow-robot/ethercat_grant) that helps with that.

Install with
```bash
sudo apt install ros-<DISTRO>-ethercat-ethercat_grant
```
and add the following to your your `node` tag in your launchfile
```xml
launch-prefix="ethercat_grant
```

## Development

With the integration of the upstream SOEM repo as a git subtree, and a major overhaul of the build system,
it is now possible to use the soem ROS package easily from your regular ROS workspace.

Simply clone this repository into your workspace
```bash
git clone git@github.com:mgruhler/soem.git
```

Note that if you want to update or patch the subtree which includes the SOEM upstream repository, you need to be sure
to do this properly.
When creating this, I followed the instructions in
[this Atlassian blog post](https://www.atlassian.com/blog/git/alternatives-to-git-submodule-git-subtree).
This covers all the things you need.

This package has been tested using both, `catkin_make` and `catkin build`.
