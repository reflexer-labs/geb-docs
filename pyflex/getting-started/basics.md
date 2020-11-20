---
description: Examples of querying the GEB contracts
---

# GEB Basics

## SAFE Engine

Show global debt and the global debt ceiling:

```python
>>> geb.safe_engine.global_debt()
Rad(5208615869764014400330809431631271622010234720384)
>>> geb.safe_engine.global_debt_ceiling()
Rad(10200000000000000000000000000000000000000000000000)
```

Get total debt available to generate:

```python
>>> str(geb.safe_engine.global_debt_ceiling() - geb.safe_engine.global_debt())
'4991.375324317972058031697987632361257179694960552'
```

Get a `SAFE`'s status:

```python
>>> collateral_type = geb.collaterals['ETH-A'].collateral_type
>>> safe = geb.safe_engine.safe(collateral_type, Address('0xdD1693BD8E307eCfDbe51D246562fc4109f871f8'))
>>> safe.locked_collateral
Wad(550000000000000000)
>>> safe.generated_debt
Wad(85000000000000000000)
```

Get updated `CollateralType` info:

```python
>>> collateral_type = geb.safe_engine.collateral_type('ETH-A')
>>> print(collateral_type)
CollateralType('ETH-A')[accumulated_rate=1.001032213690254860731418088 safe_collateral=0.000000000000000000 safe_debt=5203.253805869738490471 safety_price=157.366296298604006512381213706 liquidation_price=157.366296298604006512381213706 debt_ceiling=10200.000000000000000000000000000000000000000000000 debt_floor=85.000000000000000000000000000000000000000000000]
>>> collateral_type.liquidation_price
Ray(157366296298604006512381213706)
```

## Oracle Relayer

Get the `redemption_price` and the`redemption_rate` . Note that fetching the latest redemption price requires you to first update it and then return the value:

```python
>>> geb.oracle_relayer.redemption_price()
Ray(2026411234986175268208847109)
>>> geb.oracle_relayer.redemption_rate()
Ray(999999954662032624407551326)
```

## Tax Collector

Get the per-second stability fee applied to `SAFEs` :

```python
>>> geb.tax_collector.stability_fee(geb.safe_engine.collateral_type('ETH-A'))
Ray(1000000000472114805215157978)
```

## Liquidation Engine

Check if a `SAFE`can be liquidated:

```python
>>> collateral_type = geb.collaterals['ETH-A'].collateral_type
>>> safe = geb.safe_engine.safe(collateral_type, Address('0xdD1693BD8E307eCfDbe51D246562fc4109f871f8'))
>>> geb.liquidation_engine.can_liquidate(collateral_type, safe)
False
```

And then liquidate:

```python
>>> geb.liquidation_engine.liquidate_safe(collateral_type, safe).transact()
```

These are just a few examples. To see all supported functions,  view the source  code:

{% embed url="https://github.com/reflexer-labs/pyflex/blob/master/pyflex/gf.py" %}

