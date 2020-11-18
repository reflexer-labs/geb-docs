# Opening a SAFE

```python
>>> from web3 import Web3, HTTPProvider
>>> from pyflex import Address
>>> from pyflex.deployment import GfDeployment
>>> from pyflex.keys import register_keys
>>> from pyflex.numeric import Wad 
```

```python
>>> ETH_RPC_URL = "http://localhost:8545"
>>> web3 = Web3(HTTPProvider(endpoint_uri=ETH_RPC_URL, request_kwargs={"timeout": 60}))
```

```python
>>> web3.eth.defaultAccount ='0xdD1693BD8E307eCfDbe51D246562fc4109f871f8'
>>> register_keys(web3, ['key_file=key.json'])
Password for key.json: 
>>>
```

```python
>>> our_address = Address(web3.eth.defaultAccount)
>>> geb = GfDeployment.from_node(web3=web3)
```

```python
>>> collateral = geb.collaterals['ETH-A']
```

```python
>>> collateral.approve(our_address)
>>> geb.approve_system_coin(our_address)
```

```python
>>> collateral_amount = Wad.from_number(2.0)
>>> debt_amount = Wad.from_number(85)
```

```python
>>> collateral.collateral.deposit(collateral_amount).transact()
>>> collateral.adapter.join(our_address, collateral_amount).transact()
```

```python
>>> geb.safe_engine.modify_safe_collateralization(collateral_type, our_address, delta_collateral=collateral_amount, delta_debt=debt_amount).transact()
```

```python
>>> geb.system_coin_adapter.exit(our_address, debt_amount).transact()
```

