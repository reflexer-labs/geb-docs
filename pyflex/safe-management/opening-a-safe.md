---
description: Steps to open a SAFE and withdraw system coins
---

# Opening a SAFE

Import all the necessary dependencies:

```python
>>> from web3 import Web3, HTTPProvider
>>> from pyflex import Address
>>> from pyflex.deployment import GfDeployment
>>> from pyflex.keys import register_keys
>>> from pyflex.numeric import Wad 
```

Connect to an Ethereum node:

```python
>>> ETH_RPC_URL = "http://localhost:8545"
>>> web3 = Web3(HTTPProvider(endpoint_uri=ETH_RPC_URL, request_kwargs={"timeout": 60}))
```

Set your account and keystore file and then enter your keystore password:

```python
>>> web3.eth.defaultAccount ='0xdD1693BD8E307eCfDbe51D246562fc4109f871f8'
>>> register_keys(web3, ['key_file=key.json'])
Password for key.json: 
>>>
```

Instantiate an `Address` object to use later. Then, initialize a GEB object:

```python
>>> our_address = Address(web3.eth.defaultAccount)
>>> geb = GfDeployment.from_node(web3=web3)
```

Currently `ETH-A` is the only supported collateral:

```python
>>> collateral = geb.collaterals['ETH-A']
```

Setup your approvals in order to `join/exit` collateral and system coins in and out of the system.

{% hint style="info" %}
These `approve` calls only need to be done once per address!
{% endhint %}

```python
>>> collateral.approve(our_address)
>>> geb.approve_system_coin(our_address)
```

Set the amount of collateral to deposit and amount of debt to withdraw:

```python
>>> collateral_amount = Wad.from_number(2.0)
>>> debt_amount = Wad.from_number(85)
```

`deposit` collateral and `join` it to the system:

```python
>>> collateral.collateral.deposit(collateral_amount).transact()
>>> collateral.adapter.join(our_address, collateral_amount).transact()
```

Open a `SAFE` depositing the collateral and increasing your system coin balance in the `SAFEEngine` :

```python
>>> geb.safe_engine.modify_safe_collateralization(collateral_type, our_address, delta_collateral=collateral_amount, delta_debt=debt_amount).transact()
```

Check your coin balance in the `SAFEEngine` :

```python
>>> geb.safe_engine.coin_balance(our_address)
Rad(85000000000000000000000000000000000000000000)
```

`exit` system coin in ERC20 form:

```python
>>> geb.system_coin_adapter.exit(our_address, debt_amount).transact()
```

