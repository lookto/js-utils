# Rounding float values

Working with float values is likely to introduce rounding errors.
If you are trying to round for two decimal places for example, the obvious JS implementations are proun to error.

## Error when working with .toFixed(2)

```js
const no1 = 12312214.124124124;
const roundedNo1 = no1.toFixed(2);
console.log(roundedNo1); // 12312214.12, correct

const no2 = 1.005;
const roundedNo2 = no2.toFixed(2);
console.log(roundedNo2); // 1.00, wrong
```

## Error when working with Math.round()

```js
const no1 = 12312214.124124124;
const roundedNo1 = Math.round(no1 * 100) / 100;
console.log(roundedNo1); // 12312214.12, correct

const no2 = 1.005;
const roundedNo2 = Math.round(no2 * 100) / 100;
console.log(roundedNo2); // 1, wrong
```

## Correct result

```js
const round = (num, precision) =>
  Number(Math.round(num + "e+" + precision) + "e-" + precision);

const no1 = 12312214.124124124;
const roundedNo1 = round(no1, 2);
console.log(roundedNo1); // 12312214.12, correct

const no2 = 1.005;
const roundedNo2 = round(no2, 2);
console.log(roundedNo2); // 1.01, correct
```
