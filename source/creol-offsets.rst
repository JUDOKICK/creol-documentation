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

Core
-

Data
-
