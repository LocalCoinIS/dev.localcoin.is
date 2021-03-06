
.. _howto-become-active-node:

*********************************
Become an Active Node
*********************************

.. contents:: Table of Contents
   :local:

--------------------

Activenode generating rewards
===========================
Each activenode gets paid with 0.065 LLC per block divided by total online Activenodes running the network.

``Example``::
>>>
Let's assume, the current total amount of activenodes equal 100.
0.065 LLC activenode reward per block is the LocalCoin blockchain constant
Total daily activenode payout equal 2808 LLC is a constant calculated by the formula 24h*60min*30blocks*0.065 LLC
Under this terms, each activenode will earn 2808/100=28.08 LLC per day

ActiveNode requirements
----------------

- A registered account in the corresponding network
- Recommended Minimum balance is 1200 LLC to pay for the registration fee(10 LLC) and to stay active node to receive the activenode reward
- Executable binary
- Lifetime Member (LTM) status

Active Node Duties
------------------------

- Maintaining a public seed-node
- Maintaining the CommonCloud

Active Node FAQ
------------------------
**1. What are min minimum system and hardware requirements?** -
*Windows 10, Linux or macOS/8GB RAM 2000Mhz/CPU-Core i5/100GbSSD/LAN1Gb/100Mbs outbound internet speed*

**2. Is it also possible to run multiple active nodes under the same ip?** -
*No, One activenode = One IP = One Computer = One account*

**3. Hey guys. Is it possible to run multiple active nodes under the same account?** -
*No, One activenode = One IP = One Computer = One account*

**4. Why your node doesn't work Windows 7?** -
*Windows 7 is more than 10 years old and it isn't supported by Microsoft anymore. We recommend using any Windows not older than Windows 10 or any Linux based OS or macOS*

**5. Haw many LLC do i need to hold on my account in order for active node to make profit?** -
*Minimum balance when activenode stopes getting rewards is 511 LLC. If Your balance goes lower than 511 you will stop getting paid/rewarded. Make sure you have enough LLC to keep your activenode running. When your node get it's turn to do the work, it must burn 0.1 LLC per block as a fuel to confirm the activity and support LocalCoin blockchain network. In return your node will get reward of 0.165 LLC pre block. So your node must have LLC(fuel) in it's "tank" to burn, before you can cash the vesting balance out.*


---------------------------

How to become an Active Node
============================================================

Steps
------------

1. Run the Active Node on the Network
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 We first run the node and connect it to the P2P network with the following command::

    $ programs/witness_node/witness_node --genesis-json genesis.json -s moscow.localcoin.is:11020 --rpc-endpoint 0.0.0.0:8090

 This command opens a RPC port 8090 for *localhost* so that we can later connect the CLI wallet with it. After the network was synced and periodically receives new blocks from other participants, we can go on to the next step.

2. Create a CLI Wallet
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 We now open up the cli_wallet. The following command connects to our plain Active Node::

    $ programs/cli_wallet/cli_wallet -s ws://0.0.0.0:8090

 After launched the wallet successfully, first thing to do is setting up a password for the newly created wallet prior to importing any private keys:

- ``set_password``::

    new >>> set_password <password>
    set_password <password>
    null
    locked >>>

- ``unlock``::

    locked >>> unlock "<password>"
    unlock "<password>"
    null
    unlocked >>>

 Wallet creation is now done.

3. Import your Account (and funds) into CLI Wallet
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 To gain access to Blockchain, we import the account name (owner key) and the balance containing (active key) into the CLI wallet:

- ``import_key`` (owner wifkey), ``import_key`` (active wifkey), ``list_my_accounts`` , ``list_account_balances``::

    >>> import_key <accountname> <owner wif key>
    true
    >>> import_key <accountname> <active wif key>
    true
    >>> list_my_accounts
    [{
        "id": "1.2.15",
        ...
        "name": <accountname>,
        ...
    ]
    >>> list_account_balances <accountname>
    XXXXXXX LLC

 Both keys can be exported from the web wallet.

4. Upgrade your Account to a Lifetime Member
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- ``upgrade_account``

 Since **only lifetime members can become activenode**, you must first upgrade to a lifetime member. This step costs the lifetime-upgrade fee::

    >>> upgrade_account <accountname> true
    [a transaction in json format]

5. Registering a New Activenode (become an activenode)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 To become an activenode, you first need to create an activenode object.

- ``create_activenode``::

    >>> create_activenode <accountname> true
    {
      "ref_block_num": 139,
      "ref_block_prefix": 3692461913,
      "expiration": "2018-12-05T18:27:48",
      "operations": [[
            47,{
               "fee": {
                  "amount": 0,
                  "asset_id": "1.3.0"
               },
               "activenode_account": "1.2.16",
            }
         ]
      ],
      "extensions": [],
      "signatures": [
          "1f2ad5597af2ac4bf7a50f1eef2db49c9c0f7616718776624c2c09a2dd72a0c53a26e8c2bc928f783624c4632924330fc03f08345c8f40b9790efa2e4157184a37"
      ]
    }

 Our activenode is registered, but it can’t get rewards because you need to edit config before that. You can see the current list of activenodes by the following command:

- ``get_global_properties``::

    >>> get_global_properties
    {
      "current_activenodes": [
         "1.16.0",
         "1.16.1"
      ],
      ...

7. Configuration of the Activenode
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You should add this line into your ``config.ini``::

    activenode-account = somename
    activenode-private-key = ["LLC5As5Lds81xuPevHswM1qDjAAyLCJgDcWXHLa16mZFtznHnYTL5", "5J1RfESiSGKpLYfSZG7oaVHGS4wtPBC3U2J9L6jqQJH5dVZTjA9"]

.. Note::  Make sure to use YOUR public/private keys instead of the once given above!

We need to wait until the next maintenance interval for activenode to be added to the list of current activenodes.
