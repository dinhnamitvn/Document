# Js note

Hẳn bạn đã nghe đến `tính bất biến của dữ liệu (immutability)`. Đặc tính này, nói một cách đơn giản, là khả năng giá trị của dữ liệu `không bị thay đổi sau khi đã được khai báo`. Tính bất biến giúp cho chương trình trở nên dễ dự đoán, ít xảy ra lỗi và trong một số trường hợp còn tăng hiệu suất của ứng dụng.

## Table of Contents

1. [Variables](#1-variables)
1. [Objects](#2-objects)
1. [Arrays](#3-arrays)
1. [Destructuring](#4-destructuring)
1. [Strings](#5-strings)
1. [Arrow Functions](#6-arrow-functions)
1. [Functions](#7-functions)
1. [Modules](#8-modules)
1. [Iterators](#9-iterators)
1. [Properties](#10-properties)
1. [Comparison Operators & Equality](#11-comparison-operators--equality)
1. [Others](#12-others)

## 1.Variables

### 1.1. Luôn dùng `const` khi khai báo dữ liệu.

Luôn dùng `const` khi khai báo. `let` và `const` được giới thiệu từ phiên bản `ES6`, cho phép khai báo biến có tầm vực theo khối và không thực hiện `hoisting`. Điểm khác biệt giữa let và const là bạn có thể thay đổi giá trị của biến khai báo với `let`, trong khi không thể với `const`.

```javascript
let azoom = 1
azoom = 2 // Không thành vấn đề

const azoom = 1
azoom = 2 // Error: Assignment to constant variable.
```

Do đó, trong hầu hết các trường hợp bạn nên khai báo bằng `const` để tránh xảy ra lỗi khi giá trị của khai báo bị thay đổi bất ngờ. Cũng cần lưu ý là khi khai báo `object` với `const`, mặc dù bạn không thể gán giá trị mới cho `object` nhưng `giá trị của các thuộc tính vẫn có thể bị thay đổi`.

```javascript
const object = { name: 'azoom' }
object = { name: 'azoom 2020' } // Error: Assignment to constant variable.

// Nhưng bạn có thể...
object.name = 'azoom T18'
console.log(object) // { name: 'azoom T18' }
```

## 2. Objects

### 2.1. Dùng `{}` để tạo một đối tượng.

```javascript
// bad

const azoom = new Object()

// good

const azoom = {}
```

### 2.2. Không sử dụng các từ khoá mặc định để làm thuộc tính.

```javascript
// bad

const superman = {
  default: { clark: 'kent' },
  private: true,
}

// good

const superman = {
  defaults: { clark: 'kent' },
  hidden: true,
}
```

### 2.3. Dùng những từ ngữ đồng nghĩa với thuật ngữ dành riêng.

```javascript
// bad

const superman = {
  class: 'alien', // class từ khoá của hệ thống
}

// bad

const superman = {
  klass: 'alien', // klass không có ỹ nghĩa gì trong trường hợp này
}

// good

const superman = {
  type: 'alien',
}
```

### 2.4. Sử dụng tên thuộc tính khi tạo đối tượng với tên thuộc tính động.

> Việc này giúp chúng ta định nghĩa tất cả các thuộc tính của đối tượng một lần duy nhất.

```javascript
function getKey(k) {
  const location = {
    vietnam: 'Azoom T18',
    japan: 'Azoom T20',
  }
  return location[k]
}

// bad
const obj = {
  id: 5,
  name: 'Azoom 2020',
}
obj[getKey('vietnam')] = 'Happy'

// good
const obj = {
  id: 1,
  name: 'HaNoi',
  [getKey('vietnam')]: 'Happy',
}
```

### 2.5. Sử dụng cách khai báo phương thức rút ngắn (Property Value Shorthand).eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

```javascript
// bad
const azoom = {
  value: 1,

  addValue: function (value) {
    return azoom.value + value
  },
}

// good
const azoom = {
  value: 1,

  addValue(value) {
    return azoom.value + value
  },
}
```

### 2.6. Sử dụng cách khai báo rút ngắn cho thuộc tính. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

> Cách viết ngắn gọn và dể hiểu hơn.

```javascript
const azoom = 'Azoom 2020'

// bad
const obj = {
  azoom: azoom,
}

// good
const obj = {
  azoom,
}
```

### 2.7. Gom nhóm các thuộc tính được khai báo rút ngắn đặt lên đầu của mỗi đối tượng.

```javascript
const anakinSkywalker = 'Anakin Skywalker'
const lukeSkywalker = 'Luke Skywalker'

// bad
const obj = {
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  lukeSkywalker,
  episodeThree: 3,
  mayTheFourth: 4,
  anakinSkywalker,
}

// good
const obj = {
  lukeSkywalker, // Thuộc tính được khai báo ngắn gọn
  anakinSkywalker, // Thuộc tính được khai báo ngắn gọn
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  episodeThree: 3,
  mayTheFourth: 4,
}
```

### 2.8. Thay đổi giá trị của `object` mà vẫn đảm bảo tính chất bất biến.

Để thay đổi giá trị của `object` mà vẫn đảm bảo tính chất bất biến, chúng ta cần sử dụng `Object.assign(target, ...sources)`. Hàm này có tác dụng sao chép thuộc tính của các đối tượng `sources` vào `target`.

```javascript
const baseAzoom = { name: 'Azoom INC' }
const azoom = Object.assign(
  {},
  baseAzoom,
  { name: 'Azoom Vietnam INC', age: 1 },
  { id: 1 }
)
console.log(azoom) // { name: 'Azoom Vietnam INC', age: 1, id: 1 }
```

Cần lưu ý để đảm bảo tính bất biến thì tham số `target` nên luôn là `{}`, vì khi đó các giá trị của `sources` sẽ được `sao chép vào đối tượng mới`. Một cách dùng sai là:

```javascript
const baseAzoom = { name: 'Azoom INC' }

const azoom = Object.assign(
  baseAzoom,
  { name: 'Azoom Vietnam INC', age: 1 },
  { id: 1 }
)
console.log(azoom) // { name: 'Azoom Vietnam INC', age: 1, id: 1 }
console.log(baseAzoom) // Giá trị của baseAzoom cũng bị thay đổi thành { name: 'Azoom Vietnam INC', age: 1, id: 1 }
console.log(baseAzoom === azoom) // true
```

Ngoài `Object.assign()`, bạn cũng có thể dùng cú pháp [`Spread operator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) trong `ES6` cho `object`.

```javascript
const baseAzoom = { name: 'Azoom INC' }
const azoom = { ...baseAzoom, name: 'Azoom Vietnam INC', age: 1, id: 9 }
console.log(azoom) // { name: 'Azoom Vietnam INC', age: 1, id: 1 }
console.log(baseAzoom === azoom) // false
```

> Lưu ý là cú pháp này hiện vẫn đang được đề xuất và chưa được hỗ trợ trên hầu hết các trình duyệt. Trình duyệt cũ phải sử dụng `Babel` để chuyển đổi.

### 2.9. Một số thao tác thường gặp khác trên `object`.

_Lấy tên các thuộc tính của một object_

```javascript
const obj = { name: 'Azoom', age: 1, id: 1 }
Object.keys(obj) // ['name', 'age', 'id']
```

_Lấy giá trị của các thuộc tính của một object_

```javascript
const obj = { name: 'Azoom', age: 1, id: 1 }
Object.values(obj) // ['Azoom', 1, 1]
```

_Lấy cặp [thuộc tính, giá trị] của một object_

```javascript
const obj = { name: 'Azoom', age: 1, id: 1 }
Object.entries(obj) // [ ['name', 'Azoom'], ['age', 1], ['id', 1] ]
```

_Xóa một thuộc tính ra khỏi object_

```javascript
const baseAzoom = { name: 'Azoom', age: 1, id: 1 }

// Xóa thuộc tính age
const azoom = Object.entries(baseAzoom).reduce((acc, [key, value]) => {
  return key === 'age' ? acc : { ...acc, [key]: value }
}, {})

console.log(azoom) // { name: 'Azoom', id: 1 }
```

## 3. Arrays

### 3.1. Sử dụng `[]` để khai báo một mảng.

```javascript
// bad
const items = new Array()

// good
const items = []
```

### 3.2. Dùng `...` (Spreads) để sao chép mảng.

```javascript
// bad
const itemsCopy = []

for (let i = 0; i < items.length; i++) {
  itemsCopy[i] = items[i]
}

// good

const itemsCopy = [...items]
```

### 3.3. Dùng `Array.from` để chuyển đổi từ đối tượng sang mảng.

```javascript
const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 }

// bad
const arr = Array.prototype.slice.call(arrLike)

// good
const arr = Array.from(arrLike)

console.log(arr) // ["foo", "bar", "baz"]
```

### 3.4. Thao tác trên mảng.

Bên cạnh `object`, mảng là cấu trúc dữ liệu rất thường gặp khi làm việc trong JavaScript. Để thay đổi dữ liệu của một mảng mà vẫn đảm bảo tính bất biến, bạn có thể sử dụng cú pháp `spread`, được giới thiệu từ `ES5`. Với một số yêu cầu khác, chúng ta có thể áp dụng các hàm có sẵn của lớp Array, như [`map()`](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Global_Objects/Array/map), [`filter()`](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Global_Objects/Array/filter), [`reduce()`](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce). Một đặc điểm của các hàm này là chúng luôn trả về `mảng/giá trị mới` chứ không thay đổi mảng ban đầu.

_Áp dụng cú pháp spread_

Bạn có thể dùng spread để nhân bản một mảng.

```javascript
const a = [1, 2, 3, 4, 5]
const b = a
console.log(a === b) // true

const c = [...a]
console.log(a === c) // false
```

_Thêm một phần tử vào mảng_

```javascript
const a = [1, 2, 3]

// Không nên: a.push(4)
const b = [...a, 4] // [1, 2, 3, 4]

// Không nên: a.unshift(0)
const c = [0, ...a] // [0, 1, 2, 3]
```

_Nối hai mảng với nhau_

```javascript
const a = [0, 1]
const b = [2, 3]

// Hoặc a.concat(b)
const c = [...a, ...b]
```

_Xóa một phần tử ra khỏi mảng các đối tượng_

```javascript
const a = [
  { id: 1, name: 'Foo' },
  { id: 2, name: 'Bar' },
  { id: 3, name: 'Baz' },
]

const b = a.filter((obj) => obj.id !== 2)
console.log(b) // [ { id: 1, name: 'Foo' }, { id: 3, name: 'Baz' } ]
```

_Xóa một phần tử ở đầu mảng, cuối mảng hay ở bất cứ vị trí nào_

```javascript
const a = [0, 1, 2, 3, 4]

// Xóa phần tử ở đầu mảng
// Không nên: a.shift()
const b = a.filter((value, index) => index !== 0) // [1, 2, 3, 4]

// Xóa phần tử ở cuối mảng
// Không nên: a.pop()
const c = a.filter((value, index, arr) => index != arr.length - 1) // [0, 1, 2, 3]

// Xóa phần tử ở vị trí bất kỳ
// Không nên: a.splice(3, 1)
const d = a.filter((value, index) => index !== 3) // [0, 1, 2, 4]
```

_Thay đổi dữ liệu của mảng_

```javascript
const a = [1, 2, 3]
const b = a.map((x) => x * 2) // [2, 4, 6]

const c = [
  { id: 1, name: 'Foo' },
  { id: 2, name: 'Bar' },
  { id: 3, name: 'Baz' },
]
const d = c.map((obj) => Object.assign(obj, { name: obj.name.toUppercase() }))
console.log(d) // [ { id: 1, name: 'FOO' }, { id: 2, name: 'BAR' }, { id: 3, name: 'BAZ' } ]
```

_Sắp xếp mảng: tránh dùng phương thức `.sort` để sắp xếp mảng, vì phương thức này thay đổi thứ tự của các phần tử trong mảng được sắp xếp._

```javascript
const a = [
  { id: 1, name: 'Foo' },
  { id: 2, name: 'Bar' },
  { id: 3, name: 'Baz' },
]
const b = [...a].sort((x, y) => y.id - x.id)
console.log(b) // [ { id: 3, name: 'Baz' }, { id: 2, name: 'Bar' }, { id: 1, name: 'Foo' } ]
```

_Cũng tương tự khi bạn muốn đảo ngược (reverse) mảng._

```javascript
const a = [0, 1, 2, 3, 4]
const b = [...a].reverse() // [4, 3, 2, 1, 0]
```

## 4. Destructuring

### 4.1. Sử dụng `destructuring` khi truy cập và sử dụng nhiều thuộc tính của một đối tượng.

> Tại sao lại phải sử dụng `destructuring`?

> `Destructuring` giúp bạn không phải tạo các tham chiếu tạm thời cho các thuộc tính đó.

```javascript
// bad
function getFullName(user) {
  const firstName = user.firstName
  const lastName = user.lastName

  return `${firstName} ${lastName}`
}

// good
function getFullName(user) {
  const { firstName, lastName } = user
  return `${firstName} ${lastName}`
}

// best
function getFullName({ firstName, lastName }) {
  return `${firstName} ${lastName}`
}
```

### 4.2. Sử dụng `destructuring` trong mảng.

```javascript
const arr = [1, 2, 3, 4]

// bad
const first = arr[0]
const second = arr[1]

// good
const [first, second] = arr
```

## 5. Strings

### 5.1. Sử dụng `''` cho chuỗi.

```javascript
// bad
const name = 'Azoom Vietnam INC'

// bad - chỉ được dùng trong trường hợp chèn biến vào chuỗi
const name = `Azoom Vietnam INC`

// good
const name = 'Azoom Vietnam INC'
```

### 5.1. Hiển thị các biến trong chuỗi.

```javascript
// bad
function sayHi(name) {
  return 'How are you, ' + name + '?'
}

// bad
function sayHi(name) {
  return ['How are you, ', name, '?'].join()
}

// good
function sayHi(name) {
  return `How are you, ${name}?`
}
```

### 6. Arrow Functions

> Khuyến khích sử dụng `arrow functions`

```javascript
const array = [1, 2, 3]

// bad
array.map(function (x) {
  const y = x + 1
  return x * y
})

//good
array.map((x) => {
  const y = x + 1
  return x * y
})

// bad
array.map((number) => {
  const nextNumber = number + 1`A string containing the ${nextNumber}.`
})

// good
array.map((number) => `A string containing the ${number + 1}.`)

// good
array.map((number) => {
  const nextNumber = number + 1
  return `A string containing the ${nextNumber}.`
})

// good
array.map((number, index) => ({
  [index]: number,
}))

// bad
array.map((x) => x * x)

// good
array.map((x) => x * x)
```

_Tránh nhầm lẫn cú pháp hàm mũi tên (=>) với toán tử so sánh (<=,> =)_

```javascript
// bad
const itemHeight = (item) =>
  item.height <= 256 ? item.largeSize : item.smallSize

// bad
const itemHeight = (item) =>
  item.height >= 256 ? item.largeSize : item.smallSize

// good
const itemHeight = (item) =>
  item.height <= 256 ? item.largeSize : item.smallSize

// good
const itemHeight = (item) => {
  const { height, largeSize, smallSize } = item
  return height <= 256 ? largeSize : smallSize
}
```

## 7. Functions

### 7.1. Sử dụng biểu thức hàm được đặt tên thay vì khai báo hàm.

```javascript
// bad
function getAccounts() {
  // ...
}

// good
const accounts = () => {
  // ...
}
```

### 7.2. Luôn sử dụng tham số mặc định.

```javascript
// really bad
const toggleMenu = (isOpen) => {
  isOpen = isOpen || false
}

// good
const toggleMenu = (isOpen = false) => {
  // ...
}
```

### 7.2. Luôn để tham số mặc đinh cuối cùng.

```javascript
// bad
function handleThings(opts = {}, name) {
  // ...
}

// good
function handleThings(name, opts = {}) {
  // ...
}
```

## 8. Modules

### 8.1. Luôn luôn sử dụng `import/export`.

```javascript
// bad
const azoom = require('./azoom')
module.exports = azoom.location

// ok
import azoom from './azoom'
export default azoom.location

// best
import { location } from './azoom'
export default location
```

### 8.2. Gom import và khai báo nó ở trên đầu.

```javascript
// bad
import foo from 'foo'
foo.init()

import bar from 'bar'

// good
import foo from 'foo'
import bar from 'bar'

foo.init()
```

### 8.3. Không cần thiết thêm đuôi mở rộng cho file javascript.

```javascript
// bad
import foo from './foo.js'
import bar from './bar.jsx'
import baz from './baz/index.jsx'

// good
import foo from './foo'
import bar from './bar'
import baz from './baz'
```

## 9. Iterators

### 9.1. Không sử dụng trình lặp(`Iterators`) như `for-in`, `for`, `forEach`, `for-of`.

> Sử dụng `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / `...` cho Arrays, và `Object.keys()` / `Object.values()` / `Object.entries()` cho Objects.

```javascript
const numbers = [1, 2, 3, 4, 5]

// bad
let sum = 0
for (let num of numbers) {
  sum += num
}
sum === 15

// bad
let sum = 0
numbers.forEach((num) => {
  sum += num
})
sum === 15

// best
const sum = numbers.reduce((total, num) => total + num, 0)
sum === 15

// bad
const increasedByOne = []
for (let i = 0; i < numbers.length; i++) {
  increasedByOne.push(numbers[i] + 1)
}

// bad
const increasedByOne = []
numbers.forEach((num) => {
  increasedByOne.push(num + 1)
})

// best
const increasedByOne = numbers.map((num) => num + 1)
```

## 10. Properties

### 10.1. Sử dụng `.` để truy cập thuộc tính

```javascript
const luke = {
  gender: 'male',
  age: 28,
}

// bad
const isGender = luke['gender']

// good
const isGender = luke.gender
```

### 10.2. Sử dụng `[]` để truy cập thuộc tính bằng một biến.

```javascript
const luke = {
  gender: 'male',
  age: 28,
}
const gender = 'male'

const isGender = luke[gender]
```

## 11. Comparison Operators & Equality

### 11.1. Sử dụng === và !== thay thế cho == và !=.

### 11.2. Các câu lệnh có điều kiện như câu lệnh if đánh giá biểu thức của chúng bằng cách sử dụng phương thức trừu tượng ToBoolean và luôn tuân theo các quy tắc đơn giản sau:

- `Objects` = `true`
- `Undefined` = `false`
- `Null` = `false`
- `Booleans` giá trị là `true` hoặc `false`
- `Numbers` nếu `+0`, `-0`, or `NaN` = `fasle`, còn lại là `true`
- `Strings` nếu là chuỗi `''` = `false`, còn lại là `true`

```javascript
if ([0] && []) {
  // true
  // một array (thậm chí là một array trống) là một object, các object sẽ đánh giá là true
}
```

### 11.3. Sử dụng cách gọi tắt cho Boolean.

```javascript
// bad
if (isValid === true) {
  // ...
}

// good
if (isValid) {
  // ...
}

// bad
if (name) {
  // ...
}

// good
if (name !== '') {
  // ...
}

// bad
if (collection.length) {
  // ...
}

// good
if (collection.length > 0) {
  // ...
}
```

```javascript
// bad
const foo = a ? a : b
const bar = c ? true : false
const baz = c ? false : true

// good
const foo = a || b
const bar = !!c
const baz = !c

// bad
const foo = (a && b < 0) || c > 0 || d + 1 === 0

// bad
const bar = a ** b - (5 % d)

// bad
if (a || (b && c)) {
  return d
}

// bad
const bar = a + (b / c) * d

// good
const foo = (a && b < 0) || c > 0 || d + 1 === 0

// good
const bar = a ** b - (5 % d)

// good
if (a || (b && c)) {
  return d
}

// good
const bar = a + (b / c) * d
```

## 12. Others

### 12.1. Quy trình khi thay đổi tạo database.

- Trước khi bắt đầu thực hiện, phải có `sự chấp thuận` của `a Ishi`
- Trước khi quyết định tên cột, xác nhận trong terminology(Trong trường hợp tạo trong terminology là tốt thì nên đề xuất)
- `carparking-constants` là nơi đăng ký tất cả những cột sử dụng kiểu `int` và gửi `Pull-Request` đến cho `a Ishi`
  (ví dụ sử dụng: https://github.com/azoom/bengal/blob/master/app/models/inquiries.js)
- Từ nay trở đi tất cả các bảng mới được tạo sẽ tuần theo quy ước đặt tên sau đây
- Từ nay trở đi `API` mới được tạo phải tuân thủ theo quy ước đặt tên sau đây ngay cả khi sử dụng các bảng hiện có. (ví dụ: https://github.com/azoom/bengal-api/blob/master/routes/sublease-applications/get.js)
- Carparking constants
  https://github.com/azoom/carparking-constants

  **Quy ước đặt tên database**

- Tên bảng là `số ít`
- Tên cột của `ID` duy nhất `auto increment` là `id`.
- Thời gian upload (更新日時)・thời gian create (作成日時) = kiểu `timestamp`
- Ngoài hai cái trên thì sẽ là = kiểu `datetime`.
- Tên cột của nhóm `datetime` và nhóm `timestamp` sẽ là `xxxx_datetime` (ví dụ: `created_datetime`).
- Tên cột của dạng `date` sẽ là `xxxx_date`.
- Trong kiểu `int`, trong trường hợp `select` không chọn cái gì thì không phải là `0` mà là `NULL`
- Kiểu `boolean(tinyint)` nên được hiểu là `boolean` như `is_xxxx`, `has_xxxx`, `xxxx_exists`. Trong trường hợp không phải `true` hay cũng không phải `false` thì là `NULL`.
- Trong trường hợp `true` , `false` là bắt buộc thì thành `NOT NULL`
- Trong `numeric` `type` trong trường hợp hiển thị `Yes-No-Uncertain` thì 1`: YES, 2: NO, 3: Uncertain`、trong trường hợp không `select` gì thì sẽ là `NULL`.
- Trong trường hợp hiển thị `type` thì sẽ là `xxxx_type` ( không sử dụng `class`, `kind`,...)
- Kiểu của `Int` thì không nhất thiết phải là `xxxx_type` (ví dụ: `cancellation_schedule`, `priority…`)
- Trong trường hợp sử dụng `JSON`, sẽ không thực hiện chức năng tìm kiếm bằng `JSON` đó.
- Kiểu của `Int` được đăng ký trong `carparking-constants`nhưng không cần thiết phải trùng với tên `class` (ví dụ: `documentType` <-> `ContractRequiredDocuments`)
- Trong trường hợp từ dài với chữ viết tắt phổ biến thì nói ngắn gọn cũng được. (ví dụ: `certificate` -> `cert`)
- `Người phụ trách (担当者)` được hiển thị là `staff` thay vì là `company_manager`.
- `Công ty/ cá nhân (法人/個人)` sẽ là `legal_entiry` hoặc là `user_legal_entity` chứ không phải là `user_class`
- Hiển thị `number` được viết tắt là `num`
- Phần tên bảng của khoá ngoài không được viết tắt là `suc_id` mà là `sublease_user_contract_id`
- `minimum` → `min`
- Không áp dụng cho các loại khác trong (`date`, `datetime`) ví dụ: `payment_date` chỉ là cái tên nhưng kiểu int là `NG`, `xxxx_date` phải là kiểu `date`.
- `latitude` → `lat`, `longitude` → `lon`
- Tên cột là không cho giới từ vào (vd: `of`, `in` , ...)

### 12.2. Hạn chế sử dụng `elseif` và `else`.

> Việc hạn chế sử dụng `elseif` và `else` để tránh việc code xử lý quá nhiều. nên trả về kết quả sớm nhất.

### 12.3. Luôn tách nhỏ code nhỏ nhất đến mức có thể,

> Không nên code quá nhiều dòng hoặc logic xử lý trong 1 hàm. Hãy tách nhỏ code ra thành nhiều hàm con vừa dễ hiểu, người khác maintain code đọc cũng hiểu rõ vấn đề hơn.

### 12.4. Thay vì `comment` để giải thích dòng code đó làm gì thì hãy đặt tên có ý nghĩa với nhiệm vụ mà đoạn code đó đảm nhiệm.

### 12.5. Hạn chế việc sử dụng `try...catch` để throw error ra client. Khuyến khích sử dụng `await...catch`.

### 12.6. Trước khi commit code, hãy review lại code xoá hết `comments` và `console.log()`. Việc trace log hãy để Sentry.

### 12.7. File `.env` chỉ được sử dụng dưới local. Không commit file này lên git.
