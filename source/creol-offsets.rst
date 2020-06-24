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

Parameters:

* QuestionOptions: The array of options for the multiple choice and their associated footprints, array

Returns
The multiple choice question component

OfficeQuestionnaireView
-----------------------
OfficeQuestionnaireView contains the React framework for the business calculator. The business calculator is broken into
5 distinct sections (Employees, Energy and premises, Equipment, Travel, Goods). The fullpage library is used to
split the calculator into the various question pages.


EmailSignup
^^^^^^^^^^^
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

Returns
A text field which only accepts a valid email. Upon the user submitting an email, a Zapier link is triggered, sending an email with the user's personalised results.

QuestionContainer
^^^^^^^^^^^^^^^^^

Parameters:

* QuestionNumber: The numerical position of the question in the questionnaire
* RegionID: Numerical representation of user's geographic location

Behaviour
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

Returns
The question title and the relevant component

NumberInput
^^^^^^^^^^^

Provides an input field for users to input exact numerical answers to questions
Parameters:

* InputLabel: The placeholder text for the input field

Behaviour
Checks if the input is a number - only passes the state up if it meets this criteria

Returns
The number input field


Selection
^^^^^^^^^

Parameters:

* SelectOptions: The array of options for the dropdown menu and their associated footprint, array
* DefaultValue: Placeholder selected value, string
* DefaultBool: Boolean to determine whether the component should be full width or not (Used in Counter and Select), boolean

Returns
A dropdown selection component

Counter
^^^^^^^

Parameters:

* CounterOptions: The array of options for the counter buttons and their associated footprint, array
* SelectOptions: The array of options for the dropdown menu and their associated footprint (Optional - used in Counter and Select), array

Returns
The array of counter components and (optional) an adjacent selection dropdown component for each counter

Checkbox
^^^^^^^^

Parameters:
* CheckboxOptions: The array of options for the checkbox component and their associated footprint, array

Returns:
A set of toggleable checkbox options


Multiple Number Input
^^^^^^^^^^^^^^^^^^^^^

Parameters:
* InputData: The array of options for the various number inputs including name, description and associated footprint, array

Returns
An array of number input options - each displaying an image, input field and a description


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

And four supplementary components:

* EmailSignup
* DialogContent
* Progress
* RegionSelection




Question
^^^^^^^^
Parameters:
* QuestionNumber: The numerical position of the question in the questionnaire, number
* RegionID: Numerical representation of user's geographic location, number

Returns:
The multiple choice question component

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



Core
====
-

Data
====
-
