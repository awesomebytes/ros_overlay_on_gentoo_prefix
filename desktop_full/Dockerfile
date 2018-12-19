FROM awesomebytes/roogp_ros_kinetic_desktop_2

# If we can go faster, try
RUN sed -i -e 's/j1/j'"$((`grep -c \^processor \/proc\/cpuinfo` / 2))"'/g' $EPREFIX/etc/portage/make.conf
RUN sed -i -e 's/j2/j'"$((`grep -c \^processor \/proc\/cpuinfo` / 2))"'/g' $EPREFIX/etc/portage/make.conf

# Patch issue https://github.com/ros/ros-overlay/issues/607
ADD 0001-Force-find-gtk2.0.patch 0001-Force-find-gtk2.0.patch
RUN mkdir -p $EPREFIX/etc/portage/patches/ros-kinetic/image_view &&\
    mv 0001-Force-find-gtk2.0.patch $EPREFIX/etc/portage/patches/ros-kinetic/image_view

# desktop_full includes desktop, perception & simulators
RUN $PREFIXED emerge ros-kinetic/perception

# Workaround (bug https://bugs.gentoo.org/672598)
RUN sed -i 's/${D}/${ED}/g' /tmp/gentoo/usr/portage/dev-libs/libspnav/libspnav-0.2.2.ebuild &&\
    $PREFIXED ebuild $EPREFIX/usr/portage/dev-libs/libspnav/libspnav-0.2.2.ebuild manifest

RUN echo "# required by dev-lang/ruby-2.5.3::gentoo\n\
# required by dev-ruby/did_you_mean-1.2.1::gentoo[ruby_targets_ruby25]\n\
>=virtual/rubygems-14 ruby_targets_ruby25\n\
# required by dev-lang/ruby-2.5.3::gentoo\n\
# required by dev-ruby/did_you_mean-1.2.1::gentoo[ruby_targets_ruby25]\n\
>=dev-ruby/json-2.1.0 ruby_targets_ruby25\n\
# required by dev-lang/ruby-2.5.3::gentoo\n\
# required by dev-ruby/did_you_mean-1.2.1::gentoo[ruby_targets_ruby25]\n\
>=dev-ruby/test-unit-3.2.9 ruby_targets_ruby25\n\
# required by dev-lang/ruby-2.5.3::gentoo\n\
# required by dev-ruby/did_you_mean-1.2.1::gentoo[ruby_targets_ruby25]\n\
>=dev-ruby/minitest-5.11.3 ruby_targets_ruby25\n\
# required by dev-lang/ruby-2.5.3::gentoo\n\
# required by dev-ruby/did_you_mean-1.2.1::gentoo[ruby_targets_ruby25]\n\
>=dev-ruby/net-telnet-0.2.0 ruby_targets_ruby25\n\
# required by virtual/rubygems-14::gentoo[ruby_targets_ruby25]\n\
# required by dev-ruby/mustache-1.1.0::gentoo\n\
# required by app-text/ronn-0.7.3-r3::gentoo[ruby_targets_ruby23,-test]\n\
# required by sci-electronics/gazebo-7.11.0::ros-overlay\n\
# required by =sci-electronics/gazebo-7.11.0 (argument)\n\
>=dev-ruby/rubygems-2.7.8 ruby_targets_ruby25\n\
# required by dev-lang/ruby-2.5.3::gentoo[rdoc]\n\
# required by dev-ruby/did_you_mean-1.2.1::gentoo[ruby_targets_ruby25]\n\
>=dev-ruby/rdoc-6.0.4 ruby_targets_ruby25\n\
# required by dev-lang/ruby-2.5.3::gentoo\n\
# required by dev-ruby/did_you_mean-1.2.1::gentoo[ruby_targets_ruby25]\n\
>=dev-ruby/power_assert-1.1.3 ruby_targets_ruby25\n\
# required by dev-lang/ruby-2.5.3::gentoo\n\
# required by dev-ruby/did_you_mean-1.2.1::gentoo[ruby_targets_ruby25]\n\
>=dev-ruby/rake-12.3.2 ruby_targets_ruby25\n\
# required by dev-lang/ruby-2.5.3::gentoo\n\
# required by dev-ruby/rubygems-2.7.8::gentoo[ruby_targets_ruby25]\n\
# required by virtual/rubygems-14::gentoo[ruby_targets_ruby23]\n\
# required by dev-ruby/mustache-1.1.0::gentoo\n\
# required by app-text/ronn-0.7.3-r3::gentoo[ruby_targets_ruby23,-test]\n\
# required by sci-electronics/gazebo-7.11.0::ros-overlay\n\
# required by =sci-electronics/gazebo-7.11.0 (argument)\n\
>=dev-ruby/did_you_mean-1.2.1 ruby_targets_ruby25\n\
# required by dev-lang/ruby-2.5.3::gentoo\n\
# required by dev-ruby/did_you_mean-1.2.1::gentoo[ruby_targets_ruby25]\n\
>=dev-ruby/xmlrpc-0.3.0 ruby_targets_ruby25\n\
\n\
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby25]\n\
# required by dev-lang/ruby-2.4.5-r1::gentoo[rdoc]\n\
# required by dev-ruby/mustache-1.1.0::gentoo[ruby_targets_ruby24]\n\
# required by app-text/ronn-0.7.3-r3::gentoo[ruby_targets_ruby23,-test]\n\
# required by sci-electronics/gazebo-7.11.0::ros-overlay\n\
# required by =sci-electronics/gazebo-7.11.0 (argument)\n\
>=dev-ruby/racc-1.4.14 ruby_targets_ruby25\n\
# required by dev-ruby/rdoc-6.0.4::gentoo[ruby_targets_ruby25]\n\
# required by dev-lang/ruby-2.4.5-r1::gentoo[rdoc]\n\
# required by dev-ruby/mustache-1.1.0::gentoo[ruby_targets_ruby24]\n\
# required by app-text/ronn-0.7.3-r3::gentoo[ruby_targets_ruby23,-test]\n\
# required by sci-electronics/gazebo-7.11.0::ros-overlay\n\
# required by =sci-electronics/gazebo-7.11.0 (argument)\n\
>=dev-ruby/kpeg-1.1.0-r1 ruby_targets_ruby25" >> $EPREFIX/etc/portage/package.use


ADD ignition-transport-3.1.0-r1.ebuild ignition-transport-3.1.0-r1.ebuild
RUN mv ignition-transport-3.1.0-r1.ebuild $EPREFIX/usr/local/portage/sci-libs/ignition-transport &&\
    sed -i 's/net-libs\/ignition-msgs:0=/sci-libs\/ignition-msgs:0=/' $EPREFIX/usr/local/portage/sci-libs/ignition-transport/ignition-transport-3.1.0-r1.ebuild &&\
    $PREFIXED ebuild $EPREFIX/usr/local/portage/sci-libs/ignition-transport/ignition-transport-3.1.0-r1.ebuild manifest
RUN sed -i 's/net-libs\/ignition-transport:3=/sci-libs\/ignition-transport:3=/' $EPREFIX/usr/local/portage/sci-electronics/gazebo/gazebo-7.11.0.ebuild &&\
    sed -i 's/net-libs\/ignition-msgs:=/sci-libs\/ignition-msgs:=/' $EPREFIX/usr/local/portage/sci-electronics/gazebo/gazebo-7.11.0.ebuild &&\
    $PREFIXED ebuild $EPREFIX/usr/local/portage/sci-electronics/gazebo/gazebo-7.11.0.ebuild manifest

# RUN echo ">=sci-electronics/gazebo-7.12" >> $EPREFIX/etc/portage/package.mask
RUN echo "# required by sci-electronics/gazebo-7.11.0::ros-overlay\n\
# required by sci-electronics/gazebo (argument)\n\
>=media-libs/freeimage-3.15.4-r1 png" >> $EPREFIX/etc/portage/package.use

# Ruby >2.3 is messed up in this installation
# This trick is a pretty ugly workaround
# Maybe do a copy instead?
RUN cd /tmp/gentoo/usr/lib64/ruby/site_ruby &&\
    rm -rf 2.4.0 2.5.0 &&\
    ln -s 2.3.0 2.4.0 &&\
    ln -s 2.3.0 2.5.0
RUN cd /tmp/gentoo/usr/lib64/ruby/gems &&\
    ln -s 2.3.0 2.4.0 &&\
    ln -s 2.3.0 2.5.0

RUN cd /tmp/gentoo/usr/portage/dev-ruby/rdoc &&\
    rm rdoc-6.0.4.ebuild &&\
    wget https://gist.githubusercontent.com/awesomebytes/ec0d765fd1cd0dab7b80175aedc975bb/raw/a9f9cc152281180d7dc57fba95100a5ab0688aef/rdoc-6.0.4.ebuild &&\
    $PREFIXED ebuild rdoc-6.0.4.ebuild manifest

# Modify the gazebo-7.11.0 ebuild to not use app-text/ronn cause there is no way to emerge it
# or actually add it to package.provided
# as it will just say:
# ronn not found, manpages won't be generated
RUN echo "app-text/ronn-0.7.3-r3" >> /tmp/gentoo/etc/portage/profile/package.provided 

# also got Missing: Ignition Transport (libignition-transport-dev or libignition-transport2-dev

RUN mkdir -p /tmp/gentoo/etc/portage/patches/sci-electronics/gazebo-7.11.0 &&\
    cd /tmp/gentoo/etc/portage/patches/sci-electronics/gazebo-7.11.0 &&\
    wget https://gist.githubusercontent.com/awesomebytes/8d34f25516855fca38be0a8539ac10a0/raw/b02c0dfb283b0f450d440ddc1af8818e8b8875b8/0001-Fix-scoped_ptr-was-not-declared-in-this-scope.patch

RUN $PREFIXED emerge =sci-electronics/gazebo-7.11.0

RUN $PREFIXED emerge ros-kinetic/desktop_full

