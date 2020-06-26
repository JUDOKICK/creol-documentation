#############
Creol Offsets
#############

General overview
================

The Creol Offsets dApp is a very simple interface designed to make the purchasing of carbon offsets as seamless as possible.
It was developed with non-Crypto users in mind, despite it featuring most of the standard features crypto natives would expect.

The entire premise is to begin the transition of carbon registries to blockchain based systems, this is a simple abstraction
of the VCU system into ERC-721 tokens and powered by an ERC-20 subtoken. Far from rocket science in terms of crypto applications.

What crypto seems to lack across the board, is a seamless experience for users to become familiar with systems and onboard
them as fast as possible. While it may seem counterintuitive to provide a 3rd party FIAT payment system like Stripe into the system,
this is in fact what pretty much every dApp today does when they say "Get ETH elsewhere". It immediately puts a 3rd party barrier into the UX
of the dApp.

This problem is also mirrored in Carbon Offsetting platforms, where access to the physical offsets themselves is often quite
restricted or difficult. Usually because you must be a big corporate entity or "in the know" of how the other side of the system works.

We aimed to simplify this process as much as possible to get it to be as easy and headache free for people to offset themselves, while
also knowing their own footprint.

We've got our subscription system down to 2 clicks, login, and then pay.

App Structure
=============

Generally speaking, this a standard react app that uses redux for some state systems and simply has a web3 engine inside it.
It is built upon React dApp Boilerplate by hackingbeauty (big thanks to them) and it really made deploying this quickly quite easy.

It features the following integrations

* Web3Modal (For ease of logging in)
* Wallet type integration (Portis, Native(Metamask, Nifty, etc) , Torus (Gmail) , Fortmatic (Phone/Email)
* Drizzle/Redux (Smart Contract Integration)
* Stripe (Payments)

Within the App we have also built two distinct Carbon footprint calculators. Loosely modelled on existing wellknown calculators
but simplified even further to abstract some complexity around the calculation around of carbon footprints.


App Components
==============

Components
----------

General components for use within the react dApp

Most components carry a ``styles.scss`` file that governs the styling choices for that given component and in most cases can
be overriden with inline JSX styling. Most styles are set given the overall Mui Theme.

AppBar
^^^^^^

AppBar is an abstraction of MuiAppBar that allows for children to rendered within it.

Takes no paremeters other than the ``children`` prop for rendering children within it.


AppBarInverse
^^^^^^^^^^^^^

AppBarInverse is the AppBar with colors reversed in the style sheet. See AppBar for more details.

BottomNavigation
^^^^^^^^^^^^^^^^

General sized bar to hold components for navigation. This is currently not used in main pages.

Carries two props:

* children: for rendering children within the component
* transparent: Bool determining if the color of the bar is transparent or not. (Default: false)

Button
^^^^^^

Generic button for use, has no parameters and is simply an abstraction of MuiButton, this is an artifact of the boilerplate.

Will be deprecated soon.

CreolFooter
^^^^^^^^^^^

This is the default footer used throughout the site. Styles associated for resizing are kept in the styles sheet.
It takes no parameters as it is a static/pure component.

ExpansionPanel
^^^^^^^^^^^^^^

This is an extension to the ExpansionPanel component in Material UI. It combines specific usage of it in a particular way for usage throughout the site for text.
It has three parameters:

* ``expansionHeading`` : The heading to display when the panel is not expanded
* ``expansionContent`` : The text content that is to be display when panel is opened
* ``expansionIcon`` : Icon to depict for opening panel displayed on far right of panel

LoadingContainer
^^^^^^^^^^^^^^^^

This is a container that can be used when waiting for Drizzle to load data from smart contracts. It also detects web3 failures as well.

Takes 1 prop:

* ``drizzle`` : Drizzle object containing web3 and smart contract functions

NFTCard
^^^^^^^

This component is used to render the NFTs available to a particular customer based on their wallet address.
It renders information based on the ID passed in the props.

Parameters:

* ``tokenCount`` : How many NFTs of this type
* ``projectID``: Which ID to lookup for data/description
* ``scopeNumber``: Whic scope number to know which icon to use to display in the card
* ``homeView``: Bool on where is this nft card being displayed (True for HomeView)
* ``networkID``: Id for the network to pull from, valid values are for Mainnet or Rinkeby (Ethereum)

Returns:

Rendered NFTCard containing information on NFT belonging to that particular lookup.

StripeCheckoutButton
^^^^^^^^^^^^^^^^^^^^

This component is used to redirect to the Stripe Checkout system when looking to subscribe a user to a subscription

Parameters:

* ``buttonText`` : Text to display on the button ("Pay Now" or "Buy Now")
* ``stripe``: Stripe object to be passed in to call ``redirectToCheckout`` function
* ``address``: Ethereum address to pass to Stripe customer profile for automated NFT billing
* ``plan``: Stripe Plan ID to charge and bill for
* ``inverted``: Bool to determine whether or not to invert the colors.

Returns:

Nothing but redirects customers to right place for successful and failed stripe purchases.

StripeOneOffButton
^^^^^^^^^^^^^^^^^^

This component is used to redirect to the Stripe Checkout system when looking to charge for multiple carbon offset items.

Parameters:

* ``Currency`` : Currency symbol to display in the button text
* ``TotalPrice``: Price to display in the button text
* ``stripe``: stripe object to be  passed in to call ``redirectToCheckout`` function
* ``itemPrice``: stripe code for the pricing to be charged
* ``itemQuantity``: Stripe code quantity to be charged

Returns:

Nothing but redirects customers to right place for successful and failed stripe purchases.

SubscriptionCard
^^^^^^^^^^^^^^^^

This component displays a card demonstrating a particular subscription type within the account management section

Parameters:

* ``subscription`` : Type of subscription that the user is currently subscribed to
* ``stripe``: Stripe object to pass and upgrade or cancel subscription function to
* ``address``: Ethereum address of user
* ``buttonText``: Text to display on the signup button

Returns:

Subscription Card based on rendering parameters given.

SubscriptionCardHome
^^^^^^^^^^^^^^^^^^^^

Renders the same component as SubscriptionCard however adapted to be on the homepage as an example. Does not need address or stripe object

Parameters:

* ``subscription`` : Code to render for that particular subscription

Configs
-------

Below are the configuration settings for the Creol React dApp. They follow various categories.

* Theme: settings pertaining to the Material UI theme.
* Drizzle: Drizzle settings for smart contract interaction
* Main: Main app settings
* Stripe: Stripe settings for the API lookup to Creol's product codes
* Web3Modal: Config for the web3 Modal object


Theme
^^^^^

Two files, styles and theme are responsible for governing the styles choices.

Styles: The coloring within the system is governed here with CSS hex values.

Theme: This file builds a Mui Theme and applies styles for general use globally from the style sheet.

Drizzle
^^^^^^^

This configuration loads the appropriate Smart Contract ABIs required for usage in the Creol system. Also contains configurations for a fallback
web3 object should the user fail to login correctly.

For more details see the `Drizzle Options Docs <https://www.trufflesuite.com/docs/drizzle/reference/drizzle-options>`_ on Truffle's documentation

Main
^^^^

Simple configuration of the overall dApp name. Used for building the webpack. Is generally overwritten everywhere with appropriate meta-tags


Stripe
^^^^^^

Stripe config contains the public stripe keys for authenticating the stripe session. The actual key signing and verification is handled by Stripe on
the "Approved" domains section on stripe.com


Web3Modal
^^^^^^^^^

Web3Modal, config options for choosing and rendering which web3 provider for users to choose from.


Containers
----------

Containers hold complete and complex structures of individual "Pages" within the site.
The routing and authentication is handled by the App container and each View is responsible for displaying information correctly

App
---
Parameters:
* ``actions``:
* ``history``:
* ``drizzle``:
* ``drizzleState``:
* ``computedMatch``:
* ``stripe``:

Behaviour:
TO-DO

Returns:
The routes for the different pages


``resolveWeb3Modal`` TO-DO
Parameters:
``props`` TO-DO
``specifiedProvider`` TO-DO
Returns:
TO-DO

``setDrizzleRedux`` TO-DO
Parameters:
``props`` TO-DO
``provider`` TO-DO
Returns:
TO-DO

``resolveLayout`` TO-DO
Parameters:
``props`` TO-DO
Returns:
TO-DO

``NFTResolve`` TO-DO
Parameters:
``props`` TO-DO
Returns:
TO-DO

``shouldComponentUpdate`` TO-DO
Parameters:
``nextProps`` TO-DO
``nextState`` TO-DO
Returns:
TO-DO

``componentWillUnmount`` TO-DO
Parameters:
Returns:
TO-DO

``getDerivedStateFromProps`` TO-DO
Parameters:
``nextProps`` TO-DO
``nextState`` TO-DO
Returns:
TO-DO

``mapStateToProps`` TO-DO
Parameters:
``state`` TO-DO
Returns:
TO-DO

``mapDispatchToProps`` TO-DO
Parameters:
``dispatch`` TO-DO
Returns:
TO-DO


LandingLayoutRoute
~~~~~~~~~~~~~~~~~~
Parameters:
``component`` TO-DO
Returns:
TO-DO

Header
______
Parameters:
``history`` TO-DO
Returns:
TO-DO

``handleChange`` TO-DO
Parameters:
``event`` TO-DO
``newValue`` TO-DO
Returns:
TO-DO

``updateURL`` TO-DO
Parameters:
``tab`` TO-DO
Returns:
TO-DO


NormalLayoutRoute
~~~~~~~~~~~~~~~~~
Parameters:
``component`` TO-DO
Returns:
TO-DO

Footer
______
Parameters:
``history`` TO-DO
Returns:
TO-DO

FooterNavigation
****************
Parameters:
``history`` TO-DO
Returns:
TO-DO

OpenIconSpeedDialMenu
*********************
Parameters:
``history`` TO-DO
Returns:
TO-DO

``SpeedDials`` TO-DO
Parameters:
``props`` TO-DO
Returns:
TO-DO

``handleDirectionChange`` TO-DO
Parameters:
``event`` TO-DO
Returns:

``handleHiddenChange`` TO-DO
Parameters:
``event`` TO-DO
Returns:

``handleClose`` TO-DO
Parameters:
``name`` TO-DO
Returns:

``handleOpen`` TO-DO
Parameters:
Returns:

styles
~~~~~~
This folder contains the default styling for the text, links and animations.



AccountView
^^^^^^^^^^^

The AccountView section shows the user's account based on parameters provided by the Web3 object.

There's a few behaviours that need to be documented within the lifecycle hooks.

``componentDidMount`` The logic found with this hook is designed to create the drizzle subscription method to the smart contract
and then query the subscription contract to see if the particular user is subscribed to any given subscription.

Functions:

``formatSubscriptionData(subData)``
This function takes a JSON object from the SubscriptionData folder and formats/parses it for use within this component

Parameters:

``subData`` : Raw JSON Object full of subscription data.

Returns:

``subArray`` : Array containing JSON subscription data for use within the component.

``getButtonText(subscription, isSubscribed, subscriptionTier)``
This function returns the text required for the based on the subscription parameters from the given account

Parameters:

``subscription``  : Object containing subscription information
``isSubscribed`` : Bool true or false on whether or not the subscriber has an active subscription
``subscriptionTier`` : Which particular tier the user is subscribed to.

Returns:

``string`` String containing the button text.

Props:

``stripe`` : Stripe object that gets passed in for use for charging for new subscription choices.


AccountGrid
~~~~~~~~~~~

AccountGrid is used to render the top grid in the AccountView section.

Parameters:

``address`` : Ethereum address to render
``headerText`` : Header text to render

Returns:
Returns an AccountGrid component for display


CarbonView
^^^^^^^^^^

The CarbonView component is used to render the user's accumulated Carbon offsets on the /carbon page. It collects the
NFTs in the associated address and displays and renders them accordingly.

Functions:

``componentDidMount`` : Within this hook, the page collects the users NFTs to display.

``componentWillUpdate`` : This hook checks to see if the NFTs are finished being collected and then triggers the render
function

``fetchUserNFTs(address, drizzle)`` : Requires the users address and a valid Drizzle object to lookup contracts from. Loads and verifies NFTs belonging to this address provided

``constructNFTCards(NFTs)`` : takes in a valid NFT object containing the NFT data to render and renders the NFTCards for each set of NFTs

``calculateUserProjectsAndSupply(NFTs)`` : Quickly determines the number of NFTs in a given project a User has.

``checkScopeNumber`` : Globally available function for determining the Icon to use for a given scope number according to VERRA VCU types.

EmptyAccount
~~~~~~~~~~~~

The EmptyAccount component renders when a user does not have any NFTs within their account to display and displays the option
to either subscribe or purchase one off offsets.

It is a pure component and does not have any parameters.

LoadingBox
~~~~~~~~~~

The LoadingBox component is displayed when the NFTs are being calculated for the user.

It is a pure component and does not have any parameters.

CheckoutView
^^^^^^^^^^^^

CheckoutView is displayed to the user when a user bwishes to checkout a subscription or a set of one off offsets.

It has a default props for the plan and address set to known Creol Addresses so we can revert them if the need arises.

Props:
``stripe`` : Stripe object for redirecting to Checkout module
``drizzleState``: DrizzleState object that holds address information from the Web3 object
``isAuthenticated``: Boolean that decides whether or not a user is authenticated (web3 provider chosen in this context)


HorizontalStepper
~~~~~~~~~~~~~~~~~

The HorizontalStepper module guides new users through the checkout process and displays the appropriate step and specific stages.

Which step to render is determined by the internal step state of the component.

Props:
``stripe`` : Stripe object for redirecting to Checkout module
``drizzleState``: DrizzleState object that holds address information from the Web3 object
``isAuthenticated``: Boolean that decides whether or not a user is authenticated (web3 provider chosen in this context)
``plan``: Which Stripe Code to charge for (Subscription Code)


AccountStep
```````````

AccountStep prompts the user to create an "account" which is worded and displayed as such but what it really is the creation of a wallet.
Users are given the choice to use an internally detected provider like Metamask or choose from Torus, Fortmatic or Portis

PayStep
```````

PayStep prompts the user with a Pay Now button after they have created an account.

Props:
``stripe`` : Stripe object for redirecting to Checkout module
``drizzleState``: DrizzleState object that holds address information from the Web3 object
``isAuthenticated``: Boolean that decides whether or not a user is authenticated (web3 provider chosen in this context)
``plan``: Which Stripe Code to charge for (Subscription Code)

SuccessStep
```````````

This step is actually never shown to the user but is included in the event that the Stripe redirect fails.

FAQView
^^^^^^^

FAQ View is meant to display a standalone FAQ page. It is now deprecated and not used. Will be removed from next build.

LandingView
^^^^^^^^^^^

The LandingView is currently the homepage of beta.creol.io and is built using the Fullpage.js library. Each section is managed
by its own component. 

LoginView
^^^^^^^^^

This view is displayed to any user who wishes to log in and view their account. It is an interface to their web3 provider they chose.

The only check that is made is whether or not the user has already been authenticated.

NFTView
^^^^^^^

Module that is currently not used. It is meant to display particular NFT information as an explorer of CVCUs.

HomeView
^^^^^^^^

Old homepage. No longer used and is considered deprecated, will be removed in future release.

JourneyView (WIP)
^^^^^^^^^^^^^^^^^

The JourneyView is meant to display the user's carbon journey, although at this time it is a WIP

PromoView
^^^^^^^^^

RoadmapView
^^^^^^^^^^^

Roadmap View is meant to display a standalone Roadmap page. It is now deprecated as the Roadmap is found within LandingView.
Will be removed from the next build.

SettingsView
^^^^^^^^^^^^

Template holder page for now. SettingsView is a WIP.

SuccessView
^^^^^^^^^^^

The SuccessView is shown to users upon successful Stripe Checkout authorization and payment. It is a pure component with
no props or conditions as is generally just a happy place to be with confetti.



OfficeQuestionnaireView
^^^^^^^^^^^^^^^^^^^^^^^

OfficeQuestionnaireView contains the React framework for the business calculator. The business calculator is broken into
5 distinct sections (Employees, Energy and premises, Equipment, Travel, Goods). The fullpage library is used to
split the calculator into the various question pages.

``setFullpage`` stores a version of fullpage in the state so that components outside the fullpage wrapper can control slide movement
.. note:: Storing fullpage in a state results in the 'Cannot update during an existing state transition' warning
Parameters:
``fullpageApi`` The fullpage object
Returns:

``StartQuestionnaire`` function to move to the first question and sets the 'ProgressOn' state to display the progress components
Parameters:
``fullpageApi`` The fullpage object

``MoveQuestionnaire`` function to move to the next appropriate slide. If this is the last question, calculator moves to result page
Parameters:
``fullpageApi`` The fullpage object
``QuestionNumber`` Number indicating position in the questionnaire
Returns:

``MoveToPreviousQuestion`` function to move to the previous slide. If this is the first question, calculator doesn't move
Parameters:
``fullpageApi`` The fullpage object
``QuestionNumber`` Number indicating position in the questionnaire
Returns:

``UpdateRegion`` function to set the user's location
Parameters:
``RegionID`` Numerical representation of the user's geographical location (1:UK, 2:EU, 3: US, 2: World)
Returns:

``UpdateCategory`` function update the category display state based on question number
Parameters:
``QuestionNumber`` Number indicating position in the questionnaire
Returns:
Callbacks:
``UpdateProgress``

``UpdateProgress`` function to update the appropriate circular progress component
Parameters:
``QuestionNumber`` Number indicating position in the questionnaire
``Category`` String specifying current question category
Returns:

``ReturnQuestion`` function to return the React components based on the question number
Parameters:
``QuestionNumber`` Number indicating position in the questionnaire
``RegionID`` Numerical representation of user's geographical region
``fullpageApi`` The API necessary to move to different slides
Returns:
The React components based on the specified regionID and QuestionNumber


``UpdateQuestionFootprint`` function to handle the outputs from each question
Parameters:
``footprintAddition`` The output from the given question
``QuestionNumber`` Numerical representation of current question
``fullpageApi`` The API necessary to move to different
Returns:
Callbacks:
``UpdateTotalFootprint``


``UpdateTotalFootprint`` function to calculate the total calculator footprint
Parameters:
Returns:
Callbacks: ``ScaleResults``


``ScaleResults`` function to appropriately scale the graph results based on the previous months results
``ResultArray`` An array of the results for each category (Employee,Energy,Equipment,Food)
Parameters:
Returns:

EmailSignup
~~~~~~~~~~~
A number of calculated results from the parent component are required to populate the email with the user's results.
This includes:

Parameters:

* TotalFootprint: The user's total footprint from the calculator, number
* EmployeeFootprint: The footprint per employee, number
* EnergyFootprint: The user's energy footprint, number
* RecyclingPercentage: The user's percentage of waste recycled, number
* GreenSupplierReduction: The user's footprint reduction factor from using a green supplier, number
* LightingType: Footprint determined by type of lighting used, number
* OfficeImprovements: The footprint reduction due to office improvements, number
* TechPurchases: The footprint from the purchase of new equipment, number
* DeviceReplacementRate: Footprint scale factor based on how frequently devices are replaced, number
* MeatFreeDays: Reduction factor if the business has meat-free days, number
* LocallySourced: Reduction factor for locally sourcing food, number
* FoodWasted: Increase factor for wasting food, number
* RegionID: Numerical representation of user's geographic location, number

Returns:

A text field which only accepts a valid email. Upon the user submitting an email, a Zapier link is triggered, sending an email with the user's personalised results.

``handleChange`` event handler to set the email state depending on user input
Parameters:
Returns:

``handleSubmit`` function to send out an automated email on the submission of the email address using a Zapier hook
Parameters:
``TotalFootprint``
``EmployeeFootprint``
``EnergyFootprint``
``RecyclingPercentage``
``GreenSupplierReduction``
``LightingType``
``OfficeImprovements``
``TechPurchases``
``DeviceReplacementRate``
``MeatFreeDays``
``LocallySourced``
``FoodWasted``
``RegionID``
Returns:

``GetAverageFootprint`` function to return the average footprint of a given region
Parameters:
``RegionID`` A number corresponding to the region of the user
Returns:
The average footprint for that region

``GetRegionName`` function to return the name of a given region
Parameters:
``RegionID`` A number corresponding to the region of the user
Returns:
Name of chosen region

QuestionContainer
~~~~~~~~~~~~~~~~~

// TODO: FIND WHERE THIS GOES
Parameters:

* QuestionOptions: The array of options for the multiple choice and their associated footprints, array

Returns
The multiple choice question component

Parameters:

* QuestionNumber: The numerical position of the question in the questionnaire
* RegionID: Numerical representation of user's geographic location

Behaviour:

The QuestionContainer component parses OfficeQuestionnaireData to determine the appropriate component, options and
associated footprint for every question.
There are currently nine distinct component types:

* Number Input
* Question
* Selection
* Counter
* Checkbox
* Multiple Number Input
* Counter and Select
* Multiple Inputs
* Info

Returns:

The question title and the relevant component

``ReturnQuestionTitle`` function to parse the question title from the JSON
Parameters:
``QuestionNumber`` Number indicating position in the questionnaire
Returns:
React heading of the question title

``DetermineRegionOptions`` function to return the appropriate question options based on region
Parameters:
``QuestionNumber`` Number indicating position in the questionnaire
``RegionID`` A numerical representation of the user's geographical location
Returns:
An array specifying which component to use and what the associated options are


``UpdateQuestionFootprint`` function to update state based on the question answer
Parameters:
``footprintAddition`` Result from the question
``footprintMultiplier`` Carbon footprint impact of the chosen option
``props`` Required to pass state to parent component
Returns:

``ReturnQuestionComponents`` function to return the appropriate component based on the component name
Parameters:
``RegionOptions`` An array containing component type and the appropriate question options for that region
``props`` Required to pass state to parent component
Returns:
The React framework for the specified component

``ReturnQuestionContent`` function to combine the different content for the question
Parameters:
``QuestionNumber`` Number indicating position in the questionnaire
``RegionID`` A numerical representation of the user's geographical location
``props`` Required to pass state to parent component
Returns:
Entire React component for the question

``handleFootprintChange`` pass state to parent component when an option is chosen
Parameters:
``props`` Required to pass state to parent component
Returns:

``handleArrayChange`` pass state to parent component when an option is chosen
Parameters:
``props`` Required to pass state to parent component
Returns:

NumberInput
~~~~~~~~~~~

Provides an input field for users to input exact numerical answers to questions
Parameters:

* InputLabel: The placeholder text for the input field

Behaviour
Checks if the input is a number - only passes the state up if it meets this criteria

Returns
The number input field

``handleChange`` pass state to parent component when an option is entered if the option is a number
Parameters:
``props`` Required to pass state to parent component
``input`` input from the number input field
Returns:
Callbacks:
``handleInputChange``

``handleInputChange`` pass state to parent component when an option is entered if the option is a number
Parameters:
``props`` Required to pass state to parent component
``input`` input from the number input field
Returns:

Question
^^^^^^^^
Parameters:

* QuestionNumber: The numerical position of the question in the questionnaire, number
* RegionID: Numerical representation of user's geographic location, number

Returns:

The multiple choice question component

``ReturnQuestionContent`` function to map and return the question options
Parameters:
Returns:
Callbacks:
``UpdateFootprint``


``UpdateFootprint`` function to update state after a multiple choice option is selected
Parameters:
``OptionValue`` footprint value of the chosen option
``props`` Required to pass state to parent component
Returns:
Callbacks:
``handleFootprintChange``


``handleFootprintChange`` function to pass the state up to the parent component
Parameters:
``props`` Required to pass state to parent component
Returns:


Selection
~~~~~~~~~

Parameters:

* SelectOptions: The array of options for the dropdown menu and their associated footprint, array
* DefaultValue: Placeholder selected value, string
* DefaultBool: Boolean to determine whether the component should be full width or not (Used in Counter and Select), boolean

Returns
A dropdown selection component


``ReturnSelectArray`` function to map and return the select options
Parameters:
``CheckboxOptions`` Array of data corresponding to the region's select options
Return:
React component of the select option mapping


``UpdateFootprint``
Parameters:
``Value`` associated footprint value of the selected option
``props`` Required to pass state to parent component
Return:
Callbacks:
``handleSelectChange``


``handleSelectChange``
Parameters:
``props`` Required to pass state to parent component
Return:

Counter
~~~~~~~

Parameters:

* CounterOptions: The array of options for the counter buttons and their associated footprint, array
* SelectOptions: The array of options for the dropdown menu and their associated footprint (Optional - used in Counter and Select), array

Returns:

The array of counter components and (optional) an adjacent selection dropdown component for each counter

``ReturnCounterComponents`` function to map and return the counter components
Parameters:
``CounterOptions`` Array of data corresponding to the region's counter options
``SelectOptions``Array of data corresponding to the region's select options (optional)
``props`` Required to pass state to parent component
Return:
React component of the counter option mapping, also returns select components if SelectOptions are provided
Callbacks:
``UpdateCounter``
``DetermineStateName``
``ReturnSelect``

``ReturnSelect`` function to return the select component
``SelectOptions``Array of data corresponding to the region's select options (optional)
``OptionName`` string used to update footprint in later function
``props`` Required to pass state to parent component
Parameters:
Return: React component of the select option mapping
Callbacks:
``UpdateSelectFootprint``

``UpdateSelectFootprint``
Parameters:
``footprintAddition`` Output from the Select component
``OptionName`` String used to update appropriate footprint
``props`` Required to pass state to parent component
Return:
Callbacks:
``UpdateQuestionFootprint``

``DetermineStateName``
Parameters:
``OptionName`` string used to identify relevant state
Return:
Appropriate state

``UpdateCounter`` function to update the relevant counter display
Parameters:
``Array`` Array containing option name and associated footprint
``Value`` amount to update counter display by
``props`` Required to pass state to parent component
Return:
Callbacks:
``UpdateQuestionFootprint``


``UpdateQuestionFootprint`` function to update the total footprint of this question
Parameters:
``CounterValue`` Associated value of chosen counter option
``SelectValue`` Associated value of chosen select option
``props`` Required to pass state to parent component
Return:
Callbacks:
``handleCounterChange``

``handleCounterChange`` function to pass the state up to the parent component
Parameters:
``props`` Required to pass state to parent component
Return:

Checkbox
~~~~~~~~

Parameters:

* CheckboxOptions: The array of options for the checkbox component and their associated footprint, array

Returns:

A set of toggleable checkbox options


``ReturnCheckboxContent`` function to map and return the checkbox options
Parameters:
``CheckboxOptions`` Array of data corresponding to the regions checkbox options
``props`` Required to pass state to parent component
Returns:
React component of the checkbox option mapping
Callbacks:
``UpdateFootprint``

``UpdateFootprint`` function to update the state when a checkbox item is selected or deselected
Parameters:
``Value`` The associated footprint value of that selection
``CheckboxState`` Boolean of whether that particular checkbox is currently selected or not
``props`` required for updating the total question state
Returns:
Callbacks:
``handleCheckboxChange``


``handleCheckboxChange`` function to pass the state up to the parent component
Parameters:
``props`` required for updating the total question state
Returns:


Multiple Number Input
~~~~~~~~~~~~~~~~~~~~~

Parameters:

* InputData: The array of options for the various number inputs including name, description and associated footprint, array

Returns:

An array of number input options - each displaying an image, input field and a description


``ReturnComponents`` function to map and return the multiple input options
Parameters:
``InputData`` Relevant data for populating the multiple input component
``props`` required for updating the total question state
Returns:
React component containing multiple inputs
Callbacks:
``DetermineImage``
``UpdateQuestionFootprint``


``DetermineImage`` function to return the appropriate image
Parameters:
``TierName`` String used to determine the appropriate image
Returns:
Relevant option image

``UpdateQuestionFootprint`` function to update the appropriate state upon input change
Parameters:
``footprintAddition`` result from the chosen input field
``InputType`` String used to determine the appropriate state
``props`` required for updating the total question state
Returns:
Callbacks:
``handleOutputChange``


``handleOutputChange`` function to pass the state up to the parent component
Parameters:
``props`` required for updating the total question state
Returns:


QuestionnaireView
^^^^^^^^^^^^^^^^^

QuestionnaireView contains the React framework behind the individual carbon footprint calculator. The individual
calculator is broken into four different sections (Transport, Energy, Food, Extras). The fullpage library is used to
split the calculator into the various question pages.

The mechanics behind each question are controlled from this parent component: Each question has its own logic which
controls which component is displayed, how the question result is handled and which slide is to be moved to next .

This component also handles the subcomponents for displaying the user's progress.

This calculator relies on four distinct question types:

* Question
* FlightCounter
* AccommodationSelect
* Checkbox

And three supplementary components:

* EmailSignup
* DialogContent
* RegionSelection


``UpdateRegion`` function to set the user's location
Parameters:
``RegionID`` Numerical representation of the user's geographical location (1:UK, 2:EU, 3: US, 2: World)
Returns:


``UpdateQuestionNumber`` function to update the progress display and section title based on change in question number
Parameters:
``QuestionNo`` A number corresponding to the user's progress in the Questionnaire
Returns:


``StartQuestionnaire`` function to render content and move to the appropriate slide on beginning the questionnaire
Parameters:
``fullpageApi`` The API necessary to move to different slides
Returns:


``EndQuestionNumber`` function to render content and move to the appropriate slide on finishing the questionnaire
Parameters:
``fullpageApi`` The API necessary to move to different slides
Returns:


`HandleTransportChoice`` function to move the user to the appropriate question based on the answer to the first transport question
Parameters:
``fullpageApi`` The API necessary to move to different slides
``value`` The user's answer to the relevant question
Returns:


``UpdateStateVariable`` function to perform the appropriate actions for a given question
``QuestionNumber`` A number corresponding to the user's progress in the Questionnaire
``fullpageApi`` The API necessary to move to different slides
``value`` The user's answer to the relevant question
Parameters:
Returns:
Callbacks:
`HandleTransportChoice``
``UpdateTotalFootprint``


``UpdateCarFootprint`` function to update the state of the CarFootprint based on the combination of answers
Parameters:
Returns:
Callbacks:
``UpdateTotalFootprint``


``UpdateTotalFootprint`` function to update the user's total footprint based on their questionnaire answers
Parameters:
Returns:
Callbacks:
``SubscriptionRecommended``


``setFullpage`` stores a version of fullpage in the state so that components outside the fullpage wrapper can control slide movement
.. note:: Storing fullpage in a state results in the 'Cannot update during an existing state transition' warning
Parameters:
``fullpageApi`` The fullpage object
Returns:



``setPreviousSlide`` function to store the index of the preceding slide
Parameters:
Returns:


``handlePreviousQuestion`` function to move to the preceding question or, if slide is first of a section, moves to the last of a previous question
Parameters:
Returns:


``SubscriptionRecommended`` function to suggest subscription tier based on calculated footprint
Parameters:
Returns:


``componentDidMount`` removes the header artifact from the main page
Parameters:
Returns:


Question
~~~~~~~~
Parameters:

* QuestionNumber: The numerical position of the question in the questionnaire, number
* RegionID: Numerical representation of user's geographic location, number

Returns:

The multiple choice question component


``formatQuestionTitle`` function to parse the question from the JSON file for a given question
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
Returns:
A string of the question


``formatQuestionOptions`` function to return the appropriate values for the question based on the region
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
Returns:
An array of arrays containing the question options


``returnRadioArray`` function to map the appropriate question options
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
``props`` Required to pass state up to parent component
Returns:


``updateFootprint`` function to update the state when a question option is chosen
Parameters:
``value`` Associated footprint of the chosen option
``props`` Required to pass state up to parent component
Returns:


``handleRadioChange`` function to pass the state up to the parent component
Parameters:
``props`` Required to pass state up to parent component
Returns:


FlightCounter
~~~~~~~~~~~~~
Parameters:

* QuestionNumber: The numerical position of the question in the questionnaire, number
* RegionID: Numerical representation of user's geographic location, number

Behaviour:

The JSON data is parsed to separate the question, options and related footprints. Each counter is tied to its own
separate state which is incremented on the press of the '+' or '-' button. This updates the total footprint based on the
associated value of that counter.

Returns:

An array of counter buttons relating to the different flight options


``getFlightFootprintValue`` function get the associated value for a given flight
Parameters:
``props`` Required to pass state up to parent component
Returns:



``formatFlightFootprints`` function to return the appropriate options based on the region (1:UK, 2:EU, 3:US, 4:World)
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
Returns:
An array of arrays containing the flight question options


``formatQuestionTitle`` function to parse the question from the JSON file for a given question
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
Returns:
A string of the question


``updateFlights`` function to update the footprint state of the total and the corresponding option
Parameters:
``OptionName`` string of the selected options name
``value`` associated footprint value of that option
``currentQuestionState`` The quantity of flights displayed by the counter
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
``props`` Required to pass state up to parent component
Returns:
Callbacks:
``updateFootprintTotal``


``handleFlightFootprint`` function to pass the state up to the parent component
Parameters:
``props`` Required to pass state up to parent component
Returns:


``updateFootprintTotal`` function update the total footprint state
Parameters:
``OptionName`` string of the selected options name
``value`` associated footprint value fo that option
``currentQuestionState`` The quantity of flights displayed by the counter
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
``props`` Required to pass state up to parent component
Returns:
Callbacks:
``handleFlightFootprint`


``lookupButtonStateName`` function to return corresponding state for a given option
Parameters:
``OptionName`` string of the selected options name
Returns:
The relevant state


``constructFlightQuestions`` function update the footprint state of the total and the corresponding option
Parameters:
``Options`` An array of options containing the flight question info
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
``props`` Required to pass state up to parent component
Returns:
The React framework for the flights question


AccommodationSelect
~~~~~~~~~~~~~~~~~~~
Parameters:

* QuestionNumber: The numerical position of the question in the questionnaire, number
* RegionID: Numerical representation of user's geographic location, number

Behaviour:

The JSON data is parsed to separate the question, options and related footprints. Populates the question with the three
select components - each option having an associated footprint tied to it. Upon change of one of the select options,
the question footprint is recalculated and passed up to the parent component.

Returns:

The accommodation question and three select dropdowns relating to the three elements of the accommodation calculation


``formatQuestionTitle`` function to return the appropriate values for the question based on the region
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
Returns:
A string of the question

``formatQuestionOptions`` function to return the appropriate answers for the first select dropdown
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
Returns:
An array of arrays containing the question options

``formatQuestionOptions2`` function to return the appropriate answers for the second select dropdown
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
Returns:
An array of arrays containing the question options

``formatQuestionOptions3`` function to return the appropriate answers for the third select dropdown
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
Returns:
An array of arrays containing the question options

``returnSelectArray`` function to return the appropriate dropdown options
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
Returns:
A React component of the individual dropdown options

``UpdateFootprint`` function to update the state when a select option is chosen
Parameters:
``value`` associated footprint value of that option
``selectNumber`` The numerical index of the chosen select component
``props`` Required to pass state up to parent component
Returns:
Callbacks:
``UpdateQuestionValue``

``UpdateQuestionValue`` function to update the total footprint for the question
Parameters:
``props`` Required to pass state up to parent component
Returns:
Callbacks:
``handleSelectChange``

``handleSelectChange`` function to pass the state up to the parent component
Parameters:
Returns:


Checkbox
~~~~~~~~
Parameters:

* QuestionNumber: The numerical position of the question in the questionnaire, number
* RegionID: Numerical representation of user's geographic location, number

Behaviour:

Each checkbox option has an associated footprint - the toggled options are all added together, this question footprint
is then passed up to the parent component

Returns:

A set of toggleable checkbox options


``formatQuestionTitle`` function to return the appropriate values for the question based on the region
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
Returns:
A string of the question


``formatQuestionOptions`` function to return the appropriate options for the checkbox questions
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
Returns:
An array of arrays containing the checkbox options


``returnCheckboxArray`` A function to map the appropriate checkbox options
Parameters:
``QuestionJSON`` the entire JSON data for the individual carbon footprint calculator
``QuestionNumber`` the current question number
``RegionID`` A number relating to the user's geographical location
``props`` Required to pass state up to parent component
Returns:
A React component of the individual checkbox options
Callbacks:
``formatQuestionOptions``


``updateFootprint`` function to update the state when a checkbox item is selected or deselected
Parameters:
``value`` The associated footprint value of that selection
``checkboxState`` Boolean of whether that particular checkbox is currently selected or not
``props`` Required to pass state up to parent component
Returns:
Callbacks:
``handleCheckboxChange``


``handleCheckboxChange`` function to pass the state up to the parent component
Parameters:
``props`` Required to pass state up to parent component
Returns:


EmailSignup
~~~~~~~~~~~

Parameters:

* TotalFootprint: The user's total footprint from the calculator, number
* CarFootprint: The user's footprint from the Car question, number
* MotorcycleFootprint: The user's footprint from the Motorcycle question, number
* BusFootprint: The user's footprint from the Bus question, number
* TrainFootprint: The user's footprint from the Train question, number
* FlightFootprint: The user's footprint from the Flight question, number
* HomeFootprint: The user's footprint from the Home question, number
* HomeImprovements: The user's footprint from the Home Improvements question, number
* FoodFootprint: The user's footprint from the Food question, number
* RestaurantFootprint: The user's footprint from the Restaurant question, number
* HotelFootprint: The user's footprint from the Hotel question, number
* FashionFootprint: The user's footprint from the Fashion question, number
* AccessoryFootprint: The user's footprint from the Accessory question, number

Returns:

A text field which only accepts a valid email. Upon the user submitting an email, a Zapier link is triggered,
sending an email with the user's personalised results.


``handleChange`` event handler to set the email state depending on user input
Parameters:
Returns:

``handleSubmit`` function to send out an automated email on the submission of the email address using a Zapier hook
Parameters:
``TotalFootprint``
``EmployeeFootprint``
``EnergyFootprint``
``RecyclingPercentage``
``GreenSupplierReduction``
``LightingType``
``OfficeImprovements``
``TechPurchases``
``DeviceReplacementRate``
``MeatFreeDays``
``LocallySourced``
``FoodWasted``
``RegionID``
Returns:

``GetAverageFootprint`` function to return the average footprint of a given region
Parameters:
``RegionID`` A number corresponding to the region of the user
Returns:
The average footprint for that region

``GetRegionName`` function to return the name of a given region
Parameters:
``RegionID`` A number corresponding to the region of the user
Returns:
Name of chosen region

DialogContent
^^^^^^^^^^^^^
Parameters:

* ModalOn: Boolean to determine whether the modal should be open or not, boolean
* TotalFootprint: The user's total footprint from the calculator, number
* CarFootprint: The user's footprint from the Car question, number
* MotorcycleFootprint: The user's footprint from the Motorcycle question, number
* BusFootprint: The user's footprint from the Bus question, number
* TrainFootprint: The user's footprint from the Train question, number
* FlightFootprint: The user's footprint from the Flight question, number
* HomeFootprint: The user's footprint from the Home question, number
* HomeImprovements: The user's footprint from the Home Improvements question, number
* FoodFootprint: The user's footprint from the Food question, number
* RestaurantFootprint: The user's footprint from the Restaurant question, number
* HotelFootprint: The user's footprint from the Hotel question, number
* FashionFootprint: The user's footprint from the Fashion question, number
* AccessoryFootprint: The user's footprint from the Accessory question, number


Behaviour:

Upon clicking the backdrop or submitting their email, the modal will close. The ModalOn prop controls whether the
modal is open or not.

Returns:

A popup modal advertising a discount with the EmailSignup component embedded within.


``handleClose`` function to change the state controlling whether the modal is open
Parameters:
``props`` Required to pass state up to parent component
Returns:


``handleChange`` function to pass state to parent component
Parameters:
``props`` Required to pass state up to parent component
Returns:

RegionSelection
^^^^^^^^^^^^^^^
Parameters:

Behaviour:

Each region has a numerical index assigned:
1. UK
2. EU
3. US
4. World
The default selection is UK. The selected region controls the currency display and the question options provided.

Returns:

A dropdown selection for the user to choose their location. Regional flags display upon selection. (Optional) Also returns ``DialogBox`` underneath


``SetRegion`` function to set the state for the current region
Parameters:
``NewValue`` A number corresponding to a user's geographical location
``props`` Required to pass state up to parent component
Returns:
Callbacks:
``handleRegionChange``
``GetRegionImage``


``handleRegionChange`` function to pass state to parent component
Parameters:
``props`` Required to pass state up to parent component
Returns:


``GetRegionImage`` function to return the corresponding image based on the region
Parameters:
``NewValue`` A number corresponding to a user's geographical location
Returns:
The relevant flag svg image


``GetAverageFootprint`` function to return the corresponding average footprint based on the region
Parameters:
Returns:
The average carbon footprint per capita in a given region



DialogBox
^^^^^^^^^

Parameters:

Returns:
A button offering more info on the questionnaire. Upon clicking, a modal pops up and explains the methodology

``handleClickOpen`` function to set the state controlling whether the modal visibility to open
Parameters:
Returns:


``handleChange`` function to set the state controlling whether the modal visibility to closed
Parameters:
Returns:








Core
----
-

Data
====

BenefitsData
------------
This data is used to populate the 'Benefits' section in LandingView. The data is structured to make the parsing of this
data easier. The hierarchy for this data goes as follows:
* 1st level - Benefits
* 2nd level - Benefit number (e.g. 1, 2, 3, etc.)
* 3rd level - Benefit data (panelHeading, panelDetails)
* 3rd level (Heading) - Contains a string which dictates the heading of a given benefit panel
* 3rd level (Details) - Contains a string which controls the given benefit panel's description


FAQData
------------
This data is used to populate the 'FAQ' section in LandingView. The data is structured to make the parsing of this
data easier. The hierarchy for this data goes as follows:
* 1st level - FAQs
* 2nd level - FAQ number (e.g. 1, 2, 3, etc.)
* 3rd level - FAQ data (panelHeading, panelDetails)
* 3rd level (Heading) - Contains a string which dictates the heading of a given FAQ panel
* 3rd level (Details) - Contains a string which controls the given FAQ panel's description


NFTInfo
-------
This data is used to populate the example NFT. The data is structured to make the parsing of this
data easier. The hierarchy for this data goes as follows:
* 1st level - projects
* 2nd level - 868
* 3rd level (title) - Contains a string controlling the title of the NFT
* 3rd level (summaryText) - Contains a string summarising the NFT description
* 3rd level (expandedText) - Contains a string with the full NFT description
* 3rd level (imgPath) - Contains a string with a path to the appropriate image for this NFT
* 3rd level (imgAlt) - Contains a string with a short description of the image
* 3rd level (vcsLink) - Contains a string with a link to the associated project

OfficeQuestionnaireData
-----------------------
This data is used to populate the business questionnaire with questions, components and associated footprints. The data
is structured to make the parsing of this data easier. The general hierarchy for this data goes as follows:
* 1st level - Questions
* 2nd level - Question number (e.g. 1, 2, 3, etc.)
* 3rd level - Question data (Question, Regional Options, Component, Category)
* 4th level (Question) - Contains a string of the question (e.g. "How many employees would you like to offset?")
* 4th level (Options) - This section contains the option name and the associated value of that option for each
individual region. The structure changes depending on the chosen component - the behaviour of the options section is
detailed below
* 4th level (Component) - Contains a string specifying the component to use for this question
* 4th level (Category) - Contains a string specifying the category that question belongs to

Options
The options section is first separated into the four different regions (UKOptions, EUOptions, USOptions and
WorldOptions). Different regions are required since the associated values of some of the options may be different in
different countries.
Within these region options, the data is structured to suit the chosen component:
``Question`` This component returns a multiple choice question - as such, the data is formatted so one parent array
contains arrays of each individual option
[["Option 1 name" (string), Option 1 value (number)], ["Option 2 name" (string), Option 2 value (number)]...]
``Select`` This component returns a dropdown choice with multiple options. The data is structured exactly the same way
as the 'Question' component
[["Option 1 name" (string), Option 1 value (number)], ["Option 2 name" (string), Option 2 value (number)]...]
``Checkbox`` This component returns a checkbox with multiple options. The data is structured exactly the same way
as the 'Question' component
[["Option 1 name" (string), Option 1 value (number)], ["Option 2 name" (string), Option 2 value (number)]...]
``Counter`` This component returns multiple 'counter' components which allow a value to be incremented (See counter
component). The data is structured exactly the same way as the 'Question' component
[["Option 1 name" (string), Option 1 value (number)], ["Option 2 name" (string), Option 2 value (number)]...]
``Number Input`` This component returns a field allowing for a numerical input. The data is structured exactly the same way
as the 'Question' component, however only requires one option to populate the component.
[["Option 1 name" (string), Option 1 value (number)]]
``Counter and Select`` This component combines the 'counter' and 'select' components. The data is structured so that
within each region, there are full options provided for both the Counter and the Select component, the data for both
these subcomponents is structured the same way as the 'Question' component
"Counter":[["Option 1 name" (string), Option 1 value (number)], ["Option 2 name" (string), Option 2 value (number)]...]
"Select":[["Option 1 name" (string), Option 1 value (number)], ["Option 2 name" (string), Option 2 value (number)]...]
``Multiple Number Input`` This component returns a number of the 'Number Input' component. The data is structured
exactly the same way as the 'Number Input' component, however, a parent array contains the arrays of all the 'Number
Input' children
[["Number Input 1"], ["Number Input 2"], ["Number Input 3"]]
``Multiple Inputs`` This component returns two 'Number Input' components providing different options, so that the user
can choose which they want to fill out. Within the region, two sets of data are required for the two 'Number Input'
components. This data is structured exactly the same way as the 'Number Input' component
"Input1": [["Option 1 name" (string), Option 1 value (number)]]
"Input2": [["Option 2 name" (string), Option 2 value (number)]]
``Info`` This component is purely used to display a block of text to the user, as such, there is no data given for any
of the regions
[]

Data
.. note:: The data used in this calculator has been sourced from the most appropriate and up-to date sources available at the
time of building the calculator. However, given the complexities of carbon footprint calculation and the necessary
simplification for the purposes of this questionnaire, the calculated result may not be fully representative. However,
through improvement of data capture methods, we hope to improve the calculator further.

The sources used for the various sections can be found here:
``Employees``
https://climatesmartbusiness.com/wp-content/uploads/2013/05/CS-IND-BRIEF-OFFICES-FINAL-DIGITAL.pdf
``Energy, Premises and Recycling``
https://www.ledonecorp.com/using-led-lighting-to-reduce-your-carbon-footprint/#:~:text=Lighting%20alone%20creates%2017%25%20of,162%20coal%2Dfired%20power%20plants!
https://ie.unc.edu/files/2016/03/green_public_housing_presentation.pdf
http://publications.lib.chalmers.se/records/fulltext/136412.pdf
``Equipment``
https://www.researchgate.net/publication/267414572_A_Carbon_Footprint_of_an_Office_Building
http://www.arcom.ac.uk/-docs/proceedings/ar2002-129-136_Aye_et_al.pdf
``Travel``
https://www.carbonfootprint.com/calculator.aspx
https://mapmyemissions.com/home
https://www.icao.int/environmental-protection/Carbonoffset/Pages/default.aspx


BusinessOneOffData
------------------
This data is used to populate the business side of the one-off purchases panel. The hierarchy of this data goes as follows:
* 1st level - Region (UK, EU, US, World)
* 2nd level - Card number (e.g. 1, 2, 3, etc.)
* 3rd level - This section contains the card data which is detailed in two parts:
* 3rd level(1) - The first piece of data at this level specifies a string to be used as the card title
* 3rd level(2) - The second piece of data at this level is an array made up of 3 pieces of data:
1) Option Name (string)
2) Option footprint (number)
3) Required component (number) - the component is specified by an index (0: Counter, 1: Number Input, 2: Info message. See QuestionnaireView component for more info)
4) Input field label (string)
Structure: CardTitle, [Option Name, Option Footprint, Required Component, Input field label (Optional)]
.. note:: The input field label only needs to be included when the required component is a 'Number Input' (Required Component = 1)


OneOffData
----------
This data is used to populate the individual side of the one-off purchases panel. The hierarchy of this data goes as follows:
* 1st level - Region (UK, EU, US, World)
* 2nd level - Card number (e.g. 1, 2, 3, etc.)
* 3rd level - This section contains the card data which is detailed in two parts:
* 3rd level(1) - The first piece of data at this level specifies a string to be used as the card title
* 3rd level(2) - The second piece of data at this level is an array made up of 4 pieces of data:
1) Option Name (string)
2) Option footprint (number)
3) Required component (number) - the component is specified by an index (0: Counter, 1: Number Input, 2: Info message. See QuestionnaireView component for more info)
4) Input field label (string)
Structure: CardTitle, [Option Name, Option Footprint, Required Component, Input field label (Optional)]
.. note:: The input field label only needs to be included when the required component is a 'Number Input' (Required Component = 1)


ProjectData
-----------
This data is used to populate the 'Projects' section in the LandingView. The hierarchy for this data goes as follows:
* 1st level - Projects
* 2nd level - Project number (e.g. 1, 2, 3, etc.)
* 3rd level (Title) - Contains a string specifying the project title
* 3rd level (Subtitle) - Contains a string specifying the project subtitle
* 3rd level (Description) - Contains a string specifying the project description


QuestionnaireData
-----------------
This data is used to populate the individual questionnaire with questions, and associated footprints. The data
is structured to make the parsing of this data easier. The general hierarchy for this data goes as follows:
* 1st level - Questions
* 2nd level - Question number (e.g. 1, 2, 3, etc.)
* 3rd level - Question data (Question, Options)
* 4th level (Question) - Contains a string of the question (e.g. "How many employees would you like to offset?")
* 4th level (Options) - This section contains the option name and the associated value of that option for each
individual region. The structure of this data stays consistent for every question:
[["Option 1 name" (string), Option 1 value (number)], ["Option 2 name" (string), Option 2 value (number)]...]

.. note::The options section is first separated into the four different regions (UKOptions, EUOptions, USOptions and
WorldOptions). Different regions are required since the associated values of some of the options may be different in
different countries.

Data
.. note:: The data used in this calculator has been sourced from the most appropriate and up-to date sources available at the
time of building the calculator. However, given the complexities of carbon footprint calculation and the necessary
simplification for the purposes of this questionnaire, the calculated result may not be fully representative. However,
through improvement of data capture methods, we hope to improve the calculator further.
The sources used for the various sections can be found here:
``Travel``
https://www.carbonfootprint.com/calculator.aspx
``Energy``
https://www.ukpower.co.uk/home_energy/compare_electricity
https://www.ovoenergy.com/guides/energy-guides/how-much-electricity-does-a-home-use.html
https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/587337/DECC_factsheet_11.11.16_GLAZING_LOCKED.pdf
https://www.yesenergysolutions.co.uk/advice/how-much-energy-solar-panels-produce-home
``Food``
https://blogs.ei.columbia.edu/2012/09/04/how-green-is-local-food/
``Extras``
https://theconversation.com/how-smartphones-are-heating-up-the-planet-92793


RoadmapData
-----------
This data is used to populate the 'Roadmap' section in LandingView. The data is structured to make the parsing of this
data easier. The hierarchy for this data goes as follows:
* 1st level - Roadmap
* 2nd level - Slide number (e.g. 1, 2, 3, etc.)
* 3rd level - Slide data (panelHeading, panelDetails)
* 3rd level (Heading) - Contains a string which dictates the heading of a given roadmap panel
* 3rd level (Details) - Contains a string which controls the given roadmap panel's description


SubscriptionData
----------------
This data is used to handle the subscription panel component. The data is structured to make the parsing of this
data easier. The hierarchy for this data goes as follows:
* 1st level - subscriptions
* 2nd level - Subscription Tier (Eco Burner, Eco Warrior, Eco Saviour)
* 3rd level - FAQ data (panelHeading, panelDetails)
* 3rd level (name) - String specifying name of a chosen subscription
* 3rd level (image) - String containing path to image for a chosen subscription
* 3rd level (imageAlt) - String description of the image
* 3rd level (subscriptionText) - String specifying benefits of subscribing
* 3rd level (subscriptionSubText) - String with stating price equivalence
* 3rd level (subPrice) - String price of a chosen subscription in GBP
* 3rd level (subPriceEU) - String price of a chosen subscription in €
* 3rd level (subPricePromo) - String price of a chosen subscription at promo cost in GBP
* 3rd level (subPricePromoEU) - String price of a chosen subscription at promo cost in €
* 3rd level (stripeCode) - String product ID for standard subscription in GBP
* 3rd level (stripeCodeEU) - String product ID for standard subscription in €
* 3rd level (stripeCodePromo) - String product ID for promo subscription in GBP
* 3rd level (stripeCodePromoEU) - String product ID for promo subscription in €
* 3rd level (stripeTestCode) - String productID for testing
* 3rd level (stripeTrialCode) - String productID for free trial
* 3rd level (stripeTrialCodeEU) - String productID for free trial
* 3rd level (stripeTaxCodeVAT) - String productID for VAT
* 3rd level (stripeCouponCode) - String productID for coupon discount
* 3rd level (tierID) - String of number indicating subscription tier
* 3rd level (treesEquivalent) - String of tree equivalence for a chosen subscription
* 3rd level (carMilesEquivalent) - String of car miles equivalence for a chosen subscription
