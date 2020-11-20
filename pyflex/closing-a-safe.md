---
description: Steps to close a SAFE and withdraw collateral.
---

# Closing a SAFE

`join` system coins inside the `SAFEEngine`:

```python
>>> geb.system_coin_adapter.join(our_address, Wad.from_number(40)).transact()
```

Pay back system coins to the `SAFE` and withdraw collateral:

```python
>>> geb.safe_engine.modify_safe_collateralization(collateral_type, our_address, delta_collateral=Wad.from_number(-0.2), delta_debt=Wad.from_number(-4
```

`exit` collateral from the system:

```python
>>> geb.collateral.adapter.exit(our_address, Wad.from_number(0.2)).transact()
```

