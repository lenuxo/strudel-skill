# Strudel Copy/Paste Snippets

These are deliberately small and composable.

For longer, fully-explained examples, see:
- `references/cookbook.md`
- `references/walkthroughs.md`

## 1) Drums (mini-notation)

```js
s("[bd bd] <hh oh hh> [hh hh?] <bd sd> hh? <hh oh>").gain(1)
```

## 2) Anchor Framework Skeleton (4/4/12/12)

```js
const g_scale = "D:minor"

const base = stack(
  n("0 3 4 0").scale(g_scale).sound("gm_pad_warm").gain(0.6),
  n("2 5 6 2").scale(g_scale).sound("gm_string_ensemble_1").gain(0.4),
  n("4 ~ ~ 7 ~ ~ 8 ~ ~ 4 ~ ~").scale(g_scale).sound("gm_electric_bass_finger").gain(0.7),
  n("-7 ~ ~ -4 ~ ~ -3 ~ ~ -7 ~ ~").scale(g_scale).sound("supersaw").gain(0.8)
)
```

## 3) Common FX Combo (Small Steps)

```js
// 先 gain 再加 room/shape；lpf 用于给低频留空间
n("0 2 4 6").scale("C:minor").sound("sawtooth")
  .gain(0.7)
  .lpf(1200)
  .room(0.3)
  .shape(0.1)
```

## 4) Structure (arrange)

```js
arrange(
  [2, intro],
  [2, build],
  [8, main]
).slow(1.6)
```

## 5) Euclidean Hat Layer

```js
s("hh(5,8,2)").gain(0.5)
```

## 6) Simple Bass With Scale Degrees

```js
n("0 ~ 3 ~ 0 ~ 5 ~")
  .scale("E:minor")
  .sound("sawtooth")
  .lpf(800)
  .gain(0.7)
```
