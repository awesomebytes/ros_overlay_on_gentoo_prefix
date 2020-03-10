# ros_overlay_on_gentoo_prefix
[![Build Status](https://dev.azure.com/12719821/12719821/_apis/build/status/awesomebytes.ros_overlay_on_gentoo_prefix)](https://dev.azure.com/12719821/12719821/_build/latest?definitionId=5)

Building ROS over Gentoo Prefix (on `/tmp/gentoo` for amd64 over a Docker image of Ubuntu 16.04) thanks to [ros-overlay](https://github.com/ros/ros-overlay).

This is a project closely related to [Gentoo Prefix CI](https://github.com/awesomebytes/gentoo_prefix_ci).

Builds page on Azure Pipelines: https://dev.azure.com/12719821/12719821/_build?definitionId=5

Ready-to-use releases: https://github.com/awesomebytes/ros_overlay_on_gentoo_prefix/releases

# Currently building

* ros-melodic/ros_base
* ros-melodic/desktop

# Try it

Go to https://github.com/awesomebytes/ros_overlay_on_gentoo_prefix/releases and download the latest release of your choice (ros-melodic/ros_base is 2GB~), it's divided in 1GB parts.

Put the parts together and extract (4.4GB~):
```
cd ~
cat gentoo_on_tmp* > gentoo_on_tmp.tar.gz
tar xvf gentoo_on_tmp.tar.gz
# Probably delete the intermediate files
rm gentoo_on_tmp*
# Just enter the Gentoo Prefix environment
./gentoo/startprefix
```

ros-melodic/desktop is ~4GB, extracted ~6GB.

# WIP

* ros-melodic/desktop_full
