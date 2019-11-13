################################
Introduction to the Creol System
################################

Welcome to the Creol system. It is a complex combination of State of the art IoT, Built Environment Protocols (KNX, DALI), BIM and Blockchain. Together they bring a new paradigm in the built environment known as DABE. 

What is a DABE?
---------------

DABE stands for decentralized autonomous built environment. What does this mean you ask ? It is a name given to a new type of digital twin system in the built environment. 
Traditionally BIM (Building Information Modeling) models are used to "twin" a real life building and feed design infromation to interested parties. DABEs differ in that they also are able to feed occupancy data back to the BIM model in a verifiable way that is also cryptographically true. 
Through this they also enable much more autonomy when it comes to their lifecycle. The building is able to manage its own maintenance, scheduling and operations. By using blockchain, the records for these models can live on without the need to trust any authority to maintain them. 
Previously, large buildings would have plans stored by the main architects/engineers and held by the city leaving it very difficult for people outside the industry to query and learn from the information. 

DABEs allow for trustless information storage and analysis by making use of smart contracts. Oracles within the built environment pipe in data to smart contracts that can then be used for various features.

The primary use case developed by Creol revolves around lighting within the built environment. This includes control, scheduling, energy reduction and carbon offsetting. Further integrations are being developed for HVAC, blinds and other building operations.


Architecture
------------

Current system architecture relies on the three parts namely ``creol-dlt`` , ``creol-hardware`` and ``creol-dapp`` working together to provide the baseline data for DABEs. The aim is for DABEs to fully integrate with ``creol-bim`` and exist in BIM/blockchain space. 

A diagram is shown below demontrasting the relationships found in the current Creol DABE prototype system.


[DIAGRAM HERE]

