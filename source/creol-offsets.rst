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

Configs
-------

Theme
^^^^^

Drizzle
^^^^^^^

Main
^^^^

Stripe
^^^^^^

Web3Modal
^^^^^^^^^


Components
----------

AppBar
^^^^^^

AppBarInverse
^^^^^^^^^^^^^


Containers
----------

AccountView
^^^^^^^^^^^

AccountGrid
"""""""""""

ListHeader
""""""""""

Containers
-Different views
*

EmailSignup
^^^^^^^^^^^
Parameters
A number of calculated results from the parent component are required to populate the email with the user's results. This includes:
*TotalFootprint: The user's total footprint from the calculator
*EmployeeFootprint: The footprint per employee
*EnergyFootprint: The user's energy footprint
*RecyclingPercentage: The user's percentage of waste recycled
*GreenSupplierReduction: The user's footprint reduction factor from using a green supplier
*LightingType: Footprint determined by type of lighting used
OfficeImprovements: The footprint reduction due to office improvements
*TechPurchases: The footprint from the purchase of new equipment
*DeviceReplacementRate: Footprint scale factor based on how frequently devices are replaced
*MeatFreeDays: Reduction factor if the business has meat-free days
*LocallySourced: Reduction factor for locally sourcing food
*FoodWasted: Increase factor for wasting food
*RegionID: Numerical representation of user's geographic location

Returns
A text field which only accepts a valid email. Upon the user submitting an email, a Zapier link is triggered, sending an email with the user's personalised results.

QuestionContainer
^^^^^^^^^^^^^^^^^
Parameters
*QuestionNumber: The numerical position of the question in the questionnaire
*RegionID: Numerical representation of user's geographic location

Behaviour
The QuestionContainer component parses OfficeQuestionnaireData to determine the appropriate component, options and
associated footprint for every question.
There are currently nine distinct component types:
*Number Input
*Question
*Selection
*Counter
*Checkbox
*Multiple Number Input
*Counter and Select
*Multiple Inputs
*Info

Returns
The question title and the relevant component

NumberInput
^^^^^^^^^^^
Provides an input field for users to input exact numerical answers to questions
Parameters
*InputLabel: The placeholder text for the input field

Behaviour
Checks if the input is a number - only passes the state up if it meets this criteria

Returns
The number input field

Question
^^^^^^^^
Parameters
*QuestionOptions: The array of options for the multiple choice and their associated footprints

Returns
The multiple choice question component

Selection
^^^^^^^^^
Parameters
*SelectOptions: The array of options for the dropdown menu and their associated footprint
*DefaultValue: Placeholder selected value
*DefaultBool: Boolean to determine whether the component should be full width or not (Used in Counter and Select)

Returns
A dropdown selection component

Counter
^^^^^^^
Parameters
*CounterOptions: The array of options for the counter buttons and their associated footprint
*SelectOptions: The array of options for the dropdown menu and their associated footprint (Optional - used in Counter and Select)

Returns
The array of counter components and (optional) an adjacent selection dropdown component for each counter

Checkbox
^^^^^^^^
Parameters
*CheckboxOptions: The array of options for the checkbox component and their associated footprint

Returns
A set of toggleable checkbox options

Multiple Number Input
^^^^^^^^^^^^^^^^^^^^^
Parameters
*InputData: The array of options for the various number inputs including name, description and associated footprint

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
*Question
*FlightCounter
*AccommodationSelect
*Checkbox

And four supplementary components:
*EmailSignup
*DialogContent
*Progress
*RegionSelection

Question
^^^^^^^^




Core
====
-

Data
====
-
