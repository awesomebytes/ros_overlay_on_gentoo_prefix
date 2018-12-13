# Notes for desktop

Need to run docker with:

```bash
--security-opt seccomp:unconfined
```

# to build qt

```bash
echo "# For ROS stuff that uses qt\n\
# dev-qt/qtgui udev\n\
dev-qt/qtgui -libinput
" >> /tmp/gentoo/etc/portage/package.use
```
```bash
echo "# For ROS stuff that uses qt
# dev-qt/qtgui udev
dev-qt/qtgui -libinput
" >> /tmp/gentoo/etc/portage/package.use
```

```bash
echo ">=dev-libs/libpcre2-10.32 pcre16" >> /tmp/gentoo/etc/portage/package.use 
```

```bash
echo "# required by media-gfx/graphviz-2.40.1-r1::gentoo
# required by dev-python/pydot-1.2.3::gentoo
# required by ros-kinetic/qt_dotgraph-0.3.11::ros-overlay
# required by ros-kinetic/rqt_dep-0.4.9::ros-overlay
# required by ros-kinetic/rqt_common_plugins-0.4.8::ros-overlay
# required by ros-kinetic/viz-1.3.2::ros-overlay
# required by ros-kinetic/desktop-1.3.2::ros-overlay
# required by ros-kinetic/desktop (argument)
>=media-libs/gd-2.2.5-r1 truetype jpeg png fontconfig
# required by dev-qt/qtgui-5.11.2-r1::gentoo[xcb]
# required by dev-qt/qtwidgets-5.11.2::gentoo
# required by ros-kinetic/turtlesim-0.7.1::ros-overlay
# required by ros-kinetic/ros_tutorials-0.7.1::ros-overlay
# required by ros-kinetic/desktop-1.3.2::ros-overlay
# required by ros-kinetic/desktop (argument)
>=x11-libs/libxcb-1.13.1 xkb
# required by ros-kinetic/opencv3-3.3.1-r5::ros-overlay
# required by ros-kinetic/cv_bridge-1.12.8::ros-overlay
# required by ros-kinetic/rqt_image_view-0.4.13::ros-overlay
# required by ros-kinetic/rqt_common_plugins-0.4.8::ros-overlay
# required by ros-kinetic/viz-1.3.2::ros-overlay
# required by ros-kinetic/desktop-1.3.2::ros-overlay
# required by ros-kinetic/desktop (argument)
>=sci-libs/vtk-8.1.0-r3 rendering qt5
# required by dev-qt/qtgui-5.11.2-r1::gentoo[xcb]
# required by dev-qt/qtwidgets-5.11.2::gentoo
# required by ros-kinetic/turtlesim-0.7.1::ros-overlay
# required by ros-kinetic/ros_tutorials-0.7.1::ros-overlay
# required by ros-kinetic/desktop-1.3.2::ros-overlay
# required by ros-kinetic/desktop (argument)
>=x11-libs/libxkbcommon-0.8.2 X
# required by dev-libs/collada-dom-2.5.0::gentoo
# required by ros-kinetic/collada_urdf-1.12.12::ros-overlay
# required by ros-kinetic/robot_model-1.12.11::ros-overlay
# required by ros-kinetic/robot-1.3.2::ros-overlay
# required by ros-kinetic/desktop-1.3.2::ros-overlay
# required by ros-kinetic/desktop (argument)
>=sys-libs/zlib-1.2.11-r2 minizip" >> /tmp/gentoo/etc/portage/package.use
```

# until https://github.com/ros/ros-overlay/pull/719 is merged

```bash
mkdir -p /tmp/gentoo/etc/portage/patches/ros-kinetic/message_filters
echo "From b0e4cf7ad3960684d7915dbf84b1754f93b5447e Mon Sep 17 00:00:00 2001
From: Sammy Pfeiffer <Sammy.Pfeiffer@student.uts.edu.au>
Date: Mon, 10 Dec 2018 21:58:54 +1100
Subject: [PATCH] Changed invocation to 'add' to conform template syntax

---
 include/message_filters/synchronizer.h | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/include/message_filters/synchronizer.h b/include/message_filters/synchronizer.h
index 7891890..1c14a6f 100644
--- a/include/message_filters/synchronizer.h
+++ b/include/message_filters/synchronizer.h
@@ -355,7 +355,7 @@ private:
   template<int i>
   void cb(const typename mpl::at_c<Events, i>::type& evt)
   {
-    this->add<i>(evt);
+    this->template add<i>(evt);
   }
 
   uint32_t queue_size_;
-- 
2.7.4

" >> /tmp/gentoo/etc/portage/patches/ros-kinetic/message_filters/sync.patch
emerge ros-kinetic/message_filters
```

For https://bugs.gentoo.org/649808
```bash
mkdir -p/tmp/gentoo/etc/portage/profile/
echo "sys-fs/udev-init-scripts-32" >> /tmp/gentoo/etc/portage/profile/package.provided
```
<!-- 
We need udev so...
```bash
echo "=sys-apps/hwids-99999999 **" >> /tmp/gentoo/etc/portage/package.accept_keywords
emerge sys-apps/hwids
emerge sys-fs/udev
``` -->

(This didn't really work, I actually did `emerge =media-libs/libv4l-1.10.1`)
```bash
echo ">media-libs/libv4l-1.10.1" >> /tmp/gentoo/etc/portage/package.use
```

For https://bugs.gentoo.org/654804
TODO: make a repository for these kind of patches...
https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/CustomTree#Defining_a_custom_repository
```bash
sed -i 's/local tmp_root=\${D%\/}\/tmp/local tmp_root=\${ED%\/}\/tmp/g' /tmp/gentoo/usr/portage/dev-python/PyQt5/PyQt5-5.10.1-r1.ebuild
ebuild /tmp/gentoo/usr/portage/dev-python/PyQt5/PyQt5-5.10.1-r1.ebuild manifest
```

For Rviz... patch all the stuff and

It's needed cause SIP (which makes wrappers for python for Rviz) invokes the qmake binary, which by default is not in the path.
```bash
cd /tmp/gentoo/usr/bin
# points to qtchooser
unlink qmake
ln -s /tmp/gentoo/usr/lib64/qt5/bin/qmake qmake
#export PATH=/tmp/gentoo/usr/lib64/qt5/bin:$PATH
```

For qtwebkit, seems to have a missing dependency (as said here https://forums.gentoo.org/viewtopic-t-1082176-view-next.html?sid=7f16f26bd54e232780811b9b35a6cd65)
```bash
# set ruby to 24
eselect ruby set 1
emerge dev-ruby/rubygems 
```

```bash
echo "# required by dev-ruby/power_assert-1.1.3::gentoo[-test,ruby_targets_ruby25]
# required by dev-ruby/test-unit-3.2.9::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.5.3::gentoo
# required by dev-ruby/xmlrpc-0.3.0::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.4.5::gentoo
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.3.8::gentoo[rdoc]
# required by dev-ruby/racc-1.4.14::gentoo[ruby_targets_ruby23]
>=virtual/rubygems-14 ruby_targets_ruby25
# required by dev-lang/ruby-2.5.3::gentoo
# required by dev-ruby/xmlrpc-0.3.0::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.4.5::gentoo
# required by dev-ruby/power_assert-1.1.3::gentoo[ruby_targets_ruby24]
# required by dev-ruby/test-unit-3.2.9::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.3.8::gentoo
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby23]
>=dev-ruby/json-2.1.0 ruby_targets_ruby25
# required by dev-lang/ruby-2.5.3::gentoo
# required by dev-ruby/xmlrpc-0.3.0::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.4.5::gentoo
# required by dev-ruby/power_assert-1.1.3::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.3.8::gentoo
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby23]
>=dev-ruby/test-unit-3.2.9 ruby_targets_ruby25
# required by dev-lang/ruby-2.5.3::gentoo
# required by dev-ruby/xmlrpc-0.3.0::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.4.5::gentoo
# required by dev-ruby/power_assert-1.1.3::gentoo[ruby_targets_ruby24]
# required by dev-ruby/test-unit-3.2.9::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.3.8::gentoo
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby23]
>=dev-ruby/minitest-5.11.3 ruby_targets_ruby25
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.5.3::gentoo[rdoc]
# required by dev-ruby/xmlrpc-0.3.0::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.4.5::gentoo
# required by dev-ruby/power_assert-1.1.3::gentoo[ruby_targets_ruby24]
# required by dev-ruby/test-unit-3.2.9::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.3.8::gentoo
# required by dev-ruby/racc-1.4.14::gentoo[ruby_targets_ruby23]
>=dev-ruby/kpeg-1.1.0-r1 ruby_targets_ruby25
# required by dev-lang/ruby-2.5.3::gentoo
# required by dev-ruby/xmlrpc-0.3.0::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.4.5::gentoo
# required by dev-ruby/power_assert-1.1.3::gentoo[ruby_targets_ruby24]
# required by dev-ruby/test-unit-3.2.9::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.3.8::gentoo
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby23]
>=dev-ruby/rake-12.3.1 ruby_targets_ruby25
# required by dev-lang/ruby-2.5.3::gentoo
# required by dev-ruby/xmlrpc-0.3.0::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.4.5::gentoo
# required by dev-ruby/power_assert-1.1.3::gentoo[ruby_targets_ruby24]
# required by dev-ruby/test-unit-3.2.9::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.3.8::gentoo
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby23]
>=dev-ruby/net-telnet-0.2.0 ruby_targets_ruby25
# required by virtual/rubygems-14::gentoo[ruby_targets_ruby25]
# required by dev-ruby/xmlrpc-0.3.0::gentoo[-test,ruby_targets_ruby25]
# required by dev-lang/ruby-2.5.3::gentoo
# required by dev-ruby/did_you_mean-1.2.1::gentoo[ruby_targets_ruby25]
>=dev-ruby/rubygems-2.7.8 ruby_targets_ruby25
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.5.3::gentoo[rdoc]
# required by dev-ruby/xmlrpc-0.3.0::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.4.5::gentoo
# required by dev-ruby/power_assert-1.1.3::gentoo[ruby_targets_ruby24]
# required by dev-ruby/test-unit-3.2.9::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.3.8::gentoo
# required by dev-ruby/rubygems-2.7.8::gentoo[ruby_targets_ruby23]
# required by virtual/rubygems-14::gentoo[ruby_targets_ruby23]
# required by dev-ruby/did_you_mean-1.2.1::gentoo[-test,ruby_targets_ruby25]
>=dev-ruby/racc-1.4.14 ruby_targets_ruby25
# required by dev-lang/ruby-2.5.3::gentoo[rdoc]
# required by dev-ruby/xmlrpc-0.3.0::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.4.5::gentoo
# required by dev-ruby/power_assert-1.1.3::gentoo[ruby_targets_ruby24]
# required by dev-ruby/test-unit-3.2.9::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.3.8::gentoo
# required by dev-ruby/racc-1.4.14::gentoo[ruby_targets_ruby23]
>=dev-ruby/rdoc-6.0.4 ruby_targets_ruby25
# required by dev-lang/ruby-2.5.3::gentoo
# required by dev-ruby/xmlrpc-0.3.0::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.4.5::gentoo
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.3.8::gentoo[rdoc]
# required by dev-ruby/racc-1.4.14::gentoo[ruby_targets_ruby23]
>=dev-ruby/power_assert-1.1.3 ruby_targets_ruby25
# required by dev-lang/ruby-2.5.3::gentoo
# required by dev-ruby/xmlrpc-0.3.0::gentoo[ruby_targets_ruby25]
# required by dev-lang/ruby-2.4.5::gentoo
# required by dev-ruby/power_assert-1.1.3::gentoo[ruby_targets_ruby24]
# required by dev-ruby/test-unit-3.2.9::gentoo[ruby_targets_ruby24]
# required by dev-lang/ruby-2.3.8::gentoo
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby23]
>=dev-ruby/did_you_mean-1.2.1 ruby_targets_ruby25
# required by dev-lang/ruby-2.5.3::gentoo
# required by dev-ruby/did_you_mean-1.2.1::gentoo[ruby_targets_ruby25]
>=dev-ruby/xmlrpc-0.3.0 ruby_targets_ruby25" >> /tmp/gentoo/etc/portage/package.use
echo 'RUBY_TARGETS="ruby25"' >> /tmp/gentoo/etc/portage/make.conf
```


# Emerge it (237 packages)
```bash
emerge ros-kinetic/desktop
```

