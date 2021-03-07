# Numerics

`GEB` uses different numbers representing various levels of precision.

| Type | Precision |
| :--- | :--- |
| `Wad` | 1E-18 |
| `Ray` | 1E-27 |
| `Rad` | 1E-45 |

You can import them from pyflex:

```python
>>> from pyflex.numeric import Wad, Ray, Rad
```

Converting `Wad`, `Ray`, `Rad` to a `str` shows the numbers in a friendly format:

```python
>>> Wad.from_number(1.2)
Wad(1200000000000000000)
>>> str(Wad.from_number(1.2))
'1.200000000000000000'
```

{% hint style="warning" %}
Constructors will add a specific precision to a number  eg. `Wad(1)` is not equal to `1` but to 10^18.
{% endhint %}

```python
>>> Wad(10) == Wad.from_number(10)
False
>>> Wad(10) == Wad.from_number(10 * 1E-18)
True
```

### Operations: Addition, Subtraction, Division

`Wad`, `Ray`, and `Rad` can only perform addition, subtraction and division with another `Wad`, `Ray`, or `Rad`

```python
>>> Rad(10) + Wad(10)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Users/georgekellerman/reflexer/pyflex/lib/python3.8/site-packages/pyflex/numeric.py", line 320, in __add__
    raise ArithmeticError
ArithmeticError
>>> Ray(10) - Ray(5)
Ray(5)
>>> Rad(10) / Rad.from_number(2)
Rad(5)
>>> 

```

### Operations: Multiplication

`Wad`, `Ray`, and `Rad`  can be multiplied by any `Wad`, `Ray`, and `Rad`  and `int`.

The result is the type of the first number:

```python
>>> x = Wad.from_number(1.2) * Rad.from_number(1)
>>> type(x)
<class 'pyflex.numeric.Wad'>
>>> y = Rad.from_number(1) * Wad.from_number(1.2)
>>> type(y)
<class 'pyflex.numeric.Rad'>
```

### Conversion

`Wad`, `Ray`, and `Rad` all accept `Wad`, `Ray`, and `Rad` in the constructors. This is the canonical way to convert  numbers:

```python
>>> Rad(Wad(10))
Rad(10000000000000000000000000000)
```

{% hint style="warning" %}
During conversion \(`Rad` to `Ray`/`Wad` or `Ray` to `Wad`\) you may lose precision!
{% endhint %}

```python
>>> Wad(Rad(10))
Wad(0)
>>> Ray(Rad(20))
Ray(0)
```



