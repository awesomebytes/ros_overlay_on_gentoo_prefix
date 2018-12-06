# ros_overlay_on_gentoo_prefix
[![Build Status](https://dev.azure.com/12719821/12719821/_apis/build/status/awesomebytes.ros_overlay_on_gentoo_prefix)](https://dev.azure.com/12719821/12719821/_build/latest?definitionId=5)

Building ROS over Gentoo Prefix (on `/tmp/gentoo` for amd64 over a Docker image of Ubuntu 16.04).

This is a project closely related to [Gentoo Prefix CI](https://github.com/awesomebytes/gentoo_prefix_ci).

Builds page on Azure Pipelines: https://dev.azure.com/12719821/12719821/_build?definitionId=5

Ready-to-use releases: https://github.com/awesomebytes/ros_overlay_on_gentoo_prefix/releases

# Currently building

* ros-kinetic/ros_base

# Try it

Go to https://github.com/awesomebytes/ros_overlay_on_gentoo_prefix/releases and download the latest release (ros-kinetic/ros_base is 2GB~), it's divided in parts.

Put together and extract (4.4GB~):
```
cd ~
cat gentoo_on_tmp* > gentoo_on_tmp.tar.gz
tar xvf gentoo_on_tmp.tar.
# Probably delete the intermediate files
rm gentoo_on_tmp*
# Just enter the Gentoo Prefix environment
./gentoo/startprefix
```

# TODO

* ros-kinetic/desktop
* ros-kinetic/desktop_full
