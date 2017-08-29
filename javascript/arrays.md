### Flatten array of arrays

```
const nested = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
const flattened = Array.prototype.concat.apply([], nested)
```

### Flatten and make them unique

```
const nested = [[1, 2, 3], [3, 4, 5], [5, 6, 7]]
const unique = [...(new Set(Array.prototype.concat.apply([], nested)))]
```
