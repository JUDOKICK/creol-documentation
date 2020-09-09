##########
Creol dApp
##########

General overview
================

The Creol dApp is an iOS/Android app built with React-Native to enable user interaction with the smart contracts from the ``creol-eth`` and ``creol-carbon-eth``. It is currently unstable and highly experimental but a release candidate is on the way.
It allows for users to interact with the spaces that their lights occupy by interacting with ``RoomState`` and ``LEDState`` contracts.

A BIM/Forge model explorer is being worked on seperately in conjunction with the Creol-Bim module.

The dApp currently supports an Overview page, Controls, and a Carbon Page. Each one showing metrics or current states of the smart contracts represented.

Quick Start
-----------

To launch a demo version of the app, you'll need ``expo`` to do so.

- ``expo start`` will run the build/packager process.
- Scan the QR code with the Android Expo Client or iOS camera app to open the app.

Limitations
-----------

This app is far from complete and is constantly undergoing improvements and changes. Check back often for new updates, or if you wish to contribute, check out the contributing guide.

App Structure
=============

TBC

App Components
==============

Components
----------

TBC

Configs
----------

TBC

Containers
----------

DemoChoiceView
^^^^^^^^^^^
The Demo Choice page comes after the user has chosen their Web3 provider. This page asks the user to choose one of their registered access hubs or to view a demo version of the app.

``componentDidMount`` the logic found within this hook is used to query and return any corresponding access hubs registered to the user's address.

Functions:

``getAccessHubs``
This function queries the AccountRegistry contract to return any relevant access hub addresses

Parameters:

Returns:

``handleHubChange``
Sets state after select component is used

Parameters:

Returns:

``handleHubChange``
Upon activation of the 'submit' button, creates a new instance of the ControlState contract

Parameters:

Returns:

``handleSubmitDemo``
Upon activation of the 'demo' button, sets demoActive state in Redux by calling ``resolveDemoActive`` from App.js

Parameters:

Returns:

``mapStateToProps``
Used to retrieve state from Redux

Parameters:
``state``: object used to access demoActive state

Returns:

``mapDispatchToProps``
Used to set state in Redux

Parameters:
``dispatch``: object used to set demoActive state

Returns:


Props:
``drizzle``: drizzle object passed in, allowing access drizzle contracts and methods

``drizzleState``: drizzleState object giving access to user address

``history``: history object allowing the app to 'push' to different pages

``actions``: actions object used within ``mapDispatchToProps`` to set state in Redux


EnergyView
^^^^^^^^^^^
EnergyView contains the React framework for the three metric pages (Energy, Carbon, Ranking)

Props:

DisplayCard
~~~~~~~~~~~
A component containing 3 columns displaying a value and related icon

Props:
``data``: Data object containing value and description for each column
``Image1Required``: Boolean determining whether to display an image in the first column
``Image2Required``: Boolean determining whether to display an image in the second column
``Image3Required``: Boolean determining whether to display an image in the third column
``userCarbon``: Number of carbon credits owned by user

Functions:

``returnImage``
Returns a given image based on the input string

Parameters:
``ImageName``: String of the required image

Returns:
The relevant image


``DetermineImage``
Returns the React framework for the card image

Parameters:
``data``: Object containing the card data
``ImageRequired``: Boolean determining whether to return an image

Returns:
The React framework for the relevant image


LoadingBox
~~~~~~~~~~
Circular loading component

Props:

Returns:
Loading component

OverviewTabs
~~~~~~~~~~~~
Tabs component

Props:

Returns:
Tabs component

Functions:
``handleChange``:
Function to set state on tab selection

Parameters:

Returns:

``handleTabChange``:
Function to pass state up to parent component

Parameters:

Returns:


PieChart
~~~~~~~~
Pie chart component for data display

Props:
``SiteData``: Array containing the necessary data to populate pie chart

Returns:

Functions:
``renderActiveShape``:
Function to calculate the geometry of the pie chart and specify the unit

Parameters:
``props``: Props for the PieChart component

Returns:
Pie Chart React component


SimpleTabs
~~~~~~~~~~
Tabs component

Props:

Returns:
Tab component

Functions:
``TabPanel``:
Function to return a React tab panel

Parameters:
``Props``: Props for the tab panel

Returns:
React tab panel


``a11yProps``:
Function returns the tab id

Parameters:
``index``: Number corresponding to tab index

Returns:
Tab id





CarbonOverview
~~~~~~~~~~~~~~
Second of the metric pages; details ownership of user's NFTs.

Props:
``drizzle``: drizzle object passed in, allowing access drizzle contracts and methods

``drizzleState``: drizzleState object giving access to user address

``history``: history object allowing the app to 'push' to different pages

``actions``: actions object used within ``mapDispatchToProps`` to set state in Redux

``demoActive``: shape containing boolean determining whether to display demo content

Functions:
``fetchUserNFTs``:
Queries CarbonVCU and VCUSubtoken contracts to return NFT content

Parameters:
``address``: string containing user address to be queried
``drizzle``: drizzle object used to access contracts

Returns:

``constructNFTCards``:
Function to format NFT metadata and return React component

Parameters:
``NFTs``: Array of NFT metadata

Returns:
React component containing NFT card

``calculateUserProjectsAndSupply``:
Function to calculating supply of user projects

Parameters:
``NFTs``: Array of NFT metadata

Returns:
Array of projects, tokenSupplies, scopeNumbers

``handleTabChange``:
Function to push new page on tab selection

Parameters:

Returns:

``mapStateToProps``
Used to retrieve state from Redux

Parameters:
``state``: object used to access demoActive state

Returns:

CreditsView
~~~~~~~~~~~
Info page detailing the types of credits owned by the user (Currently in progress)

Props:
``history``: history object allowing the app to 'push' to different pages

Functions:
``returnCardData``:
Function to return the correct data for a specified card

Parameters:
``CardNumber``: Number relating to card index

Return:
Relevant card data

``handleTabChange``:
Function to push new page on tab selection

Parameters:

Returns:


ProjectsView
~~~~~~~~~~~~
Page detailing project info

Props:
``history``: history object allowing the app to 'push' to different pages

Functions:
``constructNFTInfo``:
Function to return the correct data for a specified project

Parameters:
``ProjectNumber``: Number relating to project index

Return:
Relevant project data

``handleTabChange``:
Function to push new page on tab selection

Parameters:

Returns:



EnergyOverview
~~~~~~~~~~~~~~
First of the metric pages; details energy usage of different sites.

Props:
``drizzle``: drizzle object passed in, allowing access drizzle contracts and methods

``drizzleState``: drizzleState object giving access to user address

``history``: history object allowing the app to 'push' to different pages

``actions``: actions object used within ``mapDispatchToProps`` to set state in Redux

``demoActive``: shape containing boolean determining whether to display demo content

Functions:
``fetchSiteData``:
Function returning the every LED runtime for three time periods (Monthly, Yearly, AllTime)

Parameters:
``address``: string containing user address to be queried
``drizzle``: drizzle object used to access contracts
``drizzleState``: drizzleState object, used to access user account

Returns:

``formatSiteData``:
Function to format the data from ``fetchSiteData`` into a format the PieChart component can use

Parameters:
``runtimeArray``: Array of LED runtimes
``SiteData``: Empty array to be populated by site data

Returns:

``returnSiteGoals``:
Function to return the relevant goals for a given site

Parameters:
``SiteNumber``: Number relating to the index of a site
``SiteDataLength``: Total number of sites

Returns:
React text component containing site goals

``handleTimeChange``:
Function to set TimeDisplay state on tab selection

Parameters:

Returns:

``handleTabChange``:
Function to push new page on tab selection

Parameters:

Returns:

``mapStateToProps``
Used to retrieve state from Redux

Parameters:
``state``: object used to access demoActive state

Returns:


RankingOverview
~~~~~~~~~~~~~~~
Third of the metric pages; contains impact metrics and user rankings (Currently in progress)

Props:
``drizzle``: drizzle object passed in, allowing access drizzle contracts and methods

``drizzleState``: drizzleState object giving access to user address

``history``: history object allowing the app to 'push' to different pages

``actions``: actions object used within ``mapDispatchToProps`` to set state in Redux

``demoActive``: shape containing boolean determining whether to display demo content

Functions:
``getPastEvents``:
Function to get the previous 'transfer' events of the CarbonVCU contract. Used to determine which addresses own the most credits (Currently in progress)

Parameters:
``drizzle``: drizzle object allowing access drizzle contracts and methods

Returns:

``fetchUserCarbonData``:
Function to query the carbon credit balance of a given user

Parameters:
``address``: string of the user's address
``drizzle``: drizzle object allowing access drizzle contracts and methods

Returns:

``fetchTransferEvents``:
Function to fetch all transfer events, calling ``getPastEvents``:

Parameters
``address``: string of the user's address
``drizzle``: drizzle object allowing access drizzle contracts and methods

Returns:

``returnCardData``:
Function to return the correct data for a specified card

Parameters:
``CardNumber``: Number relating to card index

Return:
Relevant card data

``handleTabChange``:
Function to push new page on tab selection

Parameters:

Returns:

CarView
~~~~~~~
Page to display the user's carbon metrics in relation to car usage

Props:
``history``: history object allowing the app to 'push' to different pages

Functions:
``returnCardData``:
Function to return the correct data for a specified card

Parameters:
``CardNumber``: Number relating to card index

Return:
Relevant card data

RankView
~~~~~~~
Page to display the further details on the user's ranking

Props:
``history``: history object allowing the app to 'push' to different pages

Functions:
``returnCardData``:
Function to return the correct data for a specified card

Parameters:
``CardNumber``: Number relating to card index

Return:
Relevant card data

TreeView
~~~~~~~
Page to display the user's carbon metrics in relation to trees

Props:
``history``: history object allowing the app to 'push' to different pages

Functions:
``returnCardData``:
Function to return the correct data for a specified card

Parameters:
``CardNumber``: Number relating to card index

Return:
Relevant card data



Data
====

OverviewData
------------
This data is used to populate the EnergyOverview section with demo data and goals
Structure:
* 1st Level - Sites
* 2nd level - Site number (0,1,2 etc.)
* 3rd level (Name) - Contains a string of the site name
* 3rd level (TotalEnergyUsage) - Contains a number representing total energy usage of a site
* 3rd level (TotalCarbonUsage) - Contains a number representing total carbon usage of a site
* 3rd level (SiteGoals) - Contains a an array of arrays, each sub-array containing a string of a given site goal
* 3rd level (Rooms) - Rooms object
* 4th level (Rooms) - Room Number
* 5th level (Rooms) - Contains a string of the room name








