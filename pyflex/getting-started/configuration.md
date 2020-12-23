# Configuration

Import all the necessary files:

```python
>>> from web3 import Web3, HTTPProvider
>>> from pyflex.deployment import GfDeployment
```

And then connect to an Ethereum node:

```python
>>> ETH_RPC_URL = "http://13.59.107.140:8545"
>>> web3 = Web3(HTTPProvider(endpoint_uri=ETH_RPC_URL, request_kwargs={"timeout": 60}))
```

Finally, configure a `geb` object. This object allows you to access most contracts in the `GEB` system:

```python
>>> geb = GfDeployment.from_node(web3)
```



