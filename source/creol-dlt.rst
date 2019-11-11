#########
Creol DLT
#########

While DLT is often use to describe any sort of technology using a form of blockchain, the Creol DLT components is made up of many blockchain elements.
Currently it is comprised of a few elements

* `creol-eth <https://github.com/creol/creol-eth>`_ (Work in Progress)
* `creol-carbon-eth <https://github.com/creol/creol-carboon-eth>`_ (Work in Progress)
* `creol-dapp <https://github.com/creol/creol-dapp>`_ (Work in progress)
* `creol-mpc <https://github.com/creol/creol-mpc>`_ (Pre-Release)

This section will focus on the first two. ``creol-eth`` and ``creol-carbon-eth``

Creol-Eth
=========

Creol-Eth is the primary repository for smart contracts pertaining to storing and managing the state of the built environment it is deployed for. 
At present time it is centered around Lighting and Lighting control. More extensions for HVAC/Blinds/etc are currently being developed and this document will be updated when they are deployed.
It relies on a hardware device described in ``creol-hardware`` to be installed in the building to cryptographically sign and report information about the DALI system. This Oracle provides state information to the contracts about each LED found within a space.
For more detailed information on how these interact checkout the Intro to the Creol Network page. 

Background
----------

This suite of contracts enable complete autonomous and trustless tracking of a deployed built environment, ensuring that the energy usage reported is correct and verifiable. 
The main motivation for this component is to ensure that reporting and auditing of energy usage within a built environment is easy to track and is verifiable. The smart contracts present this functionality. 
By enabling this, the second component ``creol-carbon-eth``	is able to trustlessly offset against these metrics without the need for 3rd party verification or auditing.

Quick Start
-----------

1. npm install will install all the required packages to run these smart contracts.

2. Run truffle compile to compile the contracts with the dependencies such as openzeppelin/oraclize (now provable) etc.

3. Contracts can then be deployed with Remix IDE

LEDState
--------

This contract pertains to the state of an LED found within a built environment that is controlled using DALI by either the Creol Hardware or by another DALI/Creol enabled device.
While the inner workings of the contracts can be found in the NatSpec of the contracts. A high level overview of what it does is as follows:

``constructor (bool defaultState, address _roomContract,address _roomOwner, uint256 _daliShortAddress, uint256 _LEDDim, uint256 _LEDWattage)`` The constructor takes a defaultState of the LED (on/off), LED Wattage and the DALI Dim of 0-254 (Representing 0-100%)
Each LED contract requires a "RoomContract" which is the room contract to which the LED is deployed to. As well as the owner of the room. 

These are automatically taken care of when deploying new hardware and using the RoomState contract to do so. The LEDFactory ensures that this is done properly using the interface and the LEDLibrary as a gas optimization. 

Each LED maintains individual control of its:

* State
* Wattage
* DALI Short Address
* Dim Settings

And tracks:

* Total Burn hours
* Daily Runtime
* Weekly Runtime
* Monthly Runtime

In addition, when state changes occure the LED contract automatically calculates a cumulative burn time and then adds that value to the elapsed time since deployment. 

Considerations
^^^^^^^^^^^^^^

While the system is heavily reliant on outside information, the firmware running the hardware is written in such a way that tracks faults and sets the state to Off if the LED fails in some sort of way.

