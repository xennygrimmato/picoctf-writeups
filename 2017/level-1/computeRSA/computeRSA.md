# computeRSA

Problem URL: https://2017game.picoctf.com/game/level-1/challenge/computeRSA

e = 150815
d = 1941
N = 435979

## Solution

```python
print pow(e, d, N)
```

We get: `133337`
