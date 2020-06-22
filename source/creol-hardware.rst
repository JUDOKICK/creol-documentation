##############
Creol Hardware
##############


CreezyPi - DALI
---------------

Creol has put together a simple to use and assemble DALI enabled control system that uses a variety of off the shelf components. This device is commonly referred to as a "CreezyPi". 
The pieces for the device are all readily available from various merchants on the internet. The complete system is comprised of 6 components:

* Raspberry Pi Model 3B+ (Although currently looking at the Raspberry Pi 4)
* MicroSD Card
* ATX LED DALI HAT (Comes in 1,2,or 4 DALI Universes)
* 3D Printed Shell
* Power Supply
* Kirale USB Thread Dongle

Details on assembly can be found here at our `downloads <https://creol.io/downloads>`_ page. You'll also find a pre-built system image that can be used for ease of deployment and setup.

Quick Start
^^^^^^^^^^^

The CreezyPi image comes loaded with all scripts required to run the device but requires some initial setup from the user. The initial Wi-Fi reconnection script must be configured if the CreezyPi is not connected via ethernet.

To edit this script and enable access via Wi-Fi, use the following.

.. code-block:: bash

	sudo nano /etc/systemd/system/wifi-otbr-permanent.service

Then edit the file as follows.	
	
[Unit]

Description=WiFi Connection
Requires=NetworkManager.service
After=ap-config.service

[Service]
Type=oneshot
ExecStart=/usr/bin/wifi_connect WIFISSIDHERE WIFIPASSHERE

[Install]
WantedBy=multi-user.target

and then reenable with

.. code-block:: bash
	
	sudo systemctl enable wifi-otbr-permanent
	

You'll notice that this method stores a plaintext key to the wifi on the device. For this reason, it is recommended to instead use ethernet and to encrypt the device after testing.


Building from Source
--------------------

If you wish to build from source, simply clone the git repository onto a CreezyPi that has been flashed with a standard Raspbian image.

.. code-block:: bash

	git submodule init
	git submodule update

	install python3.7
	single line

	sudo apt-get update -y && sudo apt-get install build-essential tk-dev libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libsqlite3-dev libssl-dev libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev libffi-dev -y && wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tar.xz && tar xf Python-3.7.0.tar.xz && cd Python-3.7.0 && ./configure && make -j 4 && sudo make altinstall && cd .. && sudo rm -r Python-3.7.0 && rm Python-3.7.0.tar.xz && sudo apt-get --purge remove build-essential tk-dev libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libsqlite3-dev libssl-dev libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev libffi-dev -y && sudo apt-get autoremove -y && sudo apt-get clean

	install RPi.GPIO

	sudo apt-get install npm
	npm install -g truffle

This should install all dependencies required to run the creezypi.py script.

Launch
------

Now run the script with 

.. code-block:: bash

	python3.7 creezypi/creezypi.py

From here youll be prompted with the Creol menu. Type

.. code-block:: bash

	start
	
To begin the creezypi monitoring. This procedure should be combined with the use of ``screen`` to enable easy to resume viewing of the system and for troubleshooting.

Classes
-------

daliBus.py
^^^^^^^^^^

daliBus is the class used to convert commands to serial comms to the ATX LED DALI hat. It supports all types of commands relayed to the version 1 board. The new boards with multiple DALI universes have a new command set that is being added to this class. 

dali_init  						- Initializes a new DALI network
dali_send_xxx_cmd   			- Sends a framed command with either 8 bits, 16 bits, 16 bits twice or 24 bits. It has a confirm flag which is a flag that resends the command in case of BUS collisions.
dali_search_address 			- Searches existing dali network for devices that are already initialized
dali_search_and_assign_address  - Searches randomized addresses and initializes them with a DALI 64 address.

statemonitor.py
^^^^^^^^^^^^^^^

This is the current interface for dealing with the state changes on chain. It monitors a "RoomState" contract and pulls the initial LED data when initializing. Afterwards the it used to listen for events using an Infura connection.

creezypi.py
^^^^^^^^^^^

This is the script that runs the CreezyPi in its entirety. It is a very simple call/response design. It monitors an Infura node at specific contracts for LED changes. 
From here you can access the dalibus and the statemonitor functions with some abstractions in the menu. 



Considerations
--------------

The creezypi software is under constant development and is subject to change until a stable release is made. Use at your own risk.





