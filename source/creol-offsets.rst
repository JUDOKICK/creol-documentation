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

AccountView
^^^^^^^^^^^


AccountGrid
~~~~~~~~~~~


AccountGrid is used to render the

ListHeader
~~~~~~~~~~

ListHeader is used to render List Headers

OfficeQuestionnaireView
-----------------------
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
^^^^^^^^^^^

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
^^^^^^^^^^^^^^^^^

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
^^^^^^^^^^^

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
^^^^^^^^^

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
^^^^^^^

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
^^^^^^^^

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
^^^^^^^^^^^^^^^^^^^^^

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
-----------------

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
^^^^^^^^
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
^^^^^^^^^^^^^
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
^^^^^^^^^^^^^^^^^^^
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
^^^^^^^^
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
^^^^^^^^^^^

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
====
-

Data
====
-
