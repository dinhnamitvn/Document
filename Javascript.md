# Js note

## Init object

```javascript
const mObj = {}
```

## Spred syntax (...)

```javascript
// bad
const mArr1 = [1,2,3];

const mCpArr = [...mArr1]
```

## Convert object to array

```javascript
const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 }

const arr = Array.from(arrLike)

console.log(arr) // ["foo", "bar", "baz"]
```


