# takor

Flexible and composable runtime type assertion for Javascript. Syntax inspired by prop-types. Supports:
- ES6 data structures
- inner elements in data structures
- nested assertions 
- inverse assertions
- custom validators

## 📦 Quick Examples
```javascript
// takor.oneOf
const isNumOrStr = takor.oneOf(Number, String)
isNumOrStr(10) // true
isNumOrStr(new Set) // false

// takor.shape basic
const checkShape = takor.shape({
    key1: takor.oneOf(Number, String)
})
checkShape({ // true
    key1: 'string'
})

// takor.shape with nested assertions
const checkNestedElement = takor.shape({
    key1: takor.shape({
        key2: takor.arrayOf(Number, Set, String)
    })
})
checkNestedElement({ // true
    key1: {
        key2: [0, new Set, '']
    }
})

checkNestedElement({ // false
    key2: {
        key1: [0, new Set, '']
    }
})

// supports literal number or string
const isValidDogBreed = takor.oneOf('terrier', 'greyhound', 'golden retriever')
isValidDogBreed('terrier') // true
isValidDogBreed('persian') // false

// custom validator(s)
const lessThanTen = (el) => el < 10
const greaterThanThree = (el) => el > 3
const goodNumberRange = takor.allOf(lessThanTen, greaterThanThree)
// compose existing validator into another
const validNumRangeOrStr = takor.arrayOf(goodNumberRange, String)

validNumRangeOrStr([8, 4, 3.5, 5]) // true
validNumRangeOrStr([8, 4, '100', 5]) // true
validNumRangeOrStr([8, 4, 100, 5]) // false
validNumRangeOrStr(10) // false 
validNumRangeOrStr(new Map) // false

// takor.mapOf
const validMap = takor.mapOf(
    [Number, takor.oneOf(Array, Set)],
    [String, String]
)
validMap(new Map([ // true
    [10, []],
    [10, new Set],
    ['10', '10']
]))

validMap(new Map([ // false
    [10, []],
    [10, new Set],
    ['10', new Set]
]))

// takor.not
const nonNullOrArray = takor.not.oneOf(null, Array)
nonNullOrArray(10) // true
nonNullOrArray([]) // false
```
