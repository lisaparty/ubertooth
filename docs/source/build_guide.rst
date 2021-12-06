===========
Build Guide
===========



Release 2020-12-R1
~~~~~~~~~~~~~~~~~~

`Release 2018-12-R1 <https://ubertooth.readthedocs.io/en/latest/Release_2018-12-R1.html>`__



Prerequisites
^^^^^^^^^^^^^

Before building libbtbb and the Ubertooth tools, some prerequisites will need to be installed. Examples that may be available from your operating system’s package repositories are:



Debian 10 / Ubuntu 20.04 / Kali
+++++++++++++++++++++++++++++++

	.. code-block:: sh

		sudo apt install cmake libusb-1.0-0-dev make gcc g++ libbluetooth-dev wget \
		pkg-config python3-numpy python3-qtpy python3-distutils python3-setuptools



Fedora 33
+++++++++

	.. code-block:: sh

		sudo dnf install libusb1-devel make gcc gcc-c++ cmake wget tar bluez-libs-devel
		echo /usr/local/lib | sudo tee /etc/ld.so.conf.d/libc.conf



RHEL 8
++++++

	.. code-block:: sh

		sudo subscription-manager repos --enable=codeready-builder-for-rhel-8-x86_64-rpms
		sudo dnf install libusb1-devel make gcc gcc-c++ wget tar bluez-libs-devel python36 python36-devel
		echo /usr/local/lib | sudo tee /etc/ld.so.conf.d/libc.conf
		# Note: you will also need to install CMake 3.12.0 or greater. See https://cmake.org/install/



CentOS 8
++++++++

	.. code-block:: sh

		sudo dnf config-manager --enable powertools
		sudo dnf install libusb1-devel make gcc gcc-c++ wget tar bluez-libs-devel python36 python36-devel
		echo /usr/local/lib | sudo tee /etc/ld.so.conf.d/libc.conf
		# Note: you will also need to install CMake 3.12.0 or greater. See https://cmake.org/install/



Mac OS X users can use either MacPorts or Homebrew to install these required packages:
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

	.. code-block:: sh

		brew install libusb wget cmake pkg-config
		or
		sudo port install libusb wget cmake python38 py38-numpy py38-qtpy



FreeBSD users can install the host tools and library directly from the ports and package system:
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

	.. code-block:: sh

		sudo pkg install ubertooth



libbtbb
^^^^^^^

Next, build the Bluetooth baseband library (libbtbb) for the Ubertooth tools to decode Bluetooth packets:

	.. code-block:: sh

		wget https://github.com/greatscottgadgets/libbtbb/archive/2020-12-R1.tar.gz -O libbtbb-2020-12-R1.tar.gz
		tar -xf libbtbb-2020-12-R1.tar.gz
		cd libbtbb-2020-12-R1
		mkdir build
		cd build
		cmake ..
		make
		sudo make install
		sudo ldconfig



Ubertooth Tools
^^^^^^^^^^^^^^^

The Ubertooth repository contains host code for sniffing Bluetooth packets, configuring the Ubertooth, and updating firmware. Build and install using:

	.. code-block:: sh

		wget https://github.com/greatscottgadgets/ubertooth/releases/download/2020-12-R1/ubertooth-2020-12-R1.tar.xz
		tar -xf ubertooth-2020-12-R1.tar.xz
		cd ubertooth-2020-12-R1/host
		mkdir build
		cd build
		cmake ..
		make
		sudo make install
		sudo ldconfig	



Wireshark plugins
^^^^^^^^^^^^^^^^^

Users of Wireshark version 2.2+ do not need to build any plugins at all and may skip this section (see `this comment <https://github.com/greatscottgadgets/libbtbb/issues/50#issuecomment-284128258>`__). This includes users of Debian 10+, Ubuntu 20.04+, Fedora 33+, RHEL 8.3+, and most other Linux distributions. You can check your version by clicking on Help --> About Wireshark.

Wireshark versions 1.12 and newer include the Ubertooth BLE plugin. It is also possible to `capture BLE from Ubertooth directly into Wireshark <https://ubertooth.readthedocs.io/en/latest/capturing_BLE_Wireshark.html>`__ with a little work.

The Wireshark BTBB and BR/EDR plugins allow Bluetooth baseband traffic that has been captured using Kismet to be analysed and disected within the Wireshark GUI. They are built separately from the rest of the Ubertooth and libbtbb software.

The directory passed to cmake as ``MAKE_INSTALL_LIBDIR`` varies from system to system, but it should be the location of existing Wireshark plugins, such as ``asn1.so`` and ``ethercat.so``. On macOS this is likely ``/Applications/Wireshark.app/Contents/PlugIns/wireshark/``.

	.. code-block:: sh

		sudo apt-get install wireshark wireshark-dev libwireshark-dev cmake
		cd libbtbb-2020-12-R1/wireshark/plugins/btbb
		mkdir build
		cd build
		cmake -DCMAKE_INSTALL_LIBDIR=/usr/lib/x86_64-linux-gnu/wireshark/libwireshark3/plugins ..
		make
		sudo make install	

Repeat for the BT BR/EDR plugin:

	.. code-block:: sh

		sudo apt-get install wireshark wireshark-dev libwireshark-dev cmake
		cd libbtbb-2020-12-R1/wireshark/plugins/btbredr
		mkdir build
		cd build
		cmake -DCMAKE_INSTALL_LIBDIR=/usr/lib/x86_64-linux-gnu/wireshark/libwireshark3/plugins ..
		make
		sudo make install



Third Party Software
~~~~~~~~~~~~~~~~~~~~

There are a number of pieces of `third party software <https://ubertooth.readthedocs.io/en/latest/third_party_software.html>`__ that support the Ubertooth. Some support Ubertooth as is, while others require plugins to be built.



Firmware
~~~~~~~~

Always `update the firmware <https://ubertooth.readthedocs.io/en/latest/firmware.html>`__ on the Ubertooth device to match the software release version being used. Next step: <https://ubertooth.readthedocs.io/en/latest/getting_started.html>``__
