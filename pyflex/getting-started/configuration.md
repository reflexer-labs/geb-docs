# Configuration

Import all the necessary files:

```python
>>> from web3 import Web3, HTTPProvider
>>> from pyflex import Address
>>> from pyflex.deployment import GfDeployment
>>> from pyflex.keys import register_keys
>>> from pyflex.numeric import Wad 
```

And then connect to an Ethereum node:

```python
>>> ETH_RPC_URL = "http://13.59.107.140:8545"
>>> web3 = Web3(HTTPProvider(endpoint_uri=ETH_RPC_URL, request_kwargs={"timeout": 60}))
```

Configure a `geb` object. This object allow you to access most contracts in the `GEB` system.

```python
>>> geb = GfDeployment.from_node(web3)
```



