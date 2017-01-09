## 1.2 Check Permutation

### 1) Listen
> Given two strings, write a method to decide if one is a permutation of the other.


### 2) Example
Given inputs `'something'` and `'thoseming'` the function should return `true`.


### 3) Brute Force
We could do something like the following:

| # |      Step       |              Runtime               |
|---|-----------------|------------------------------------|
| 1 | Sort string a   | O(log A)                           |
| 2 | Sort string b   | O(log B)                           |
| 3 | Compare a and b | O(1)                               |
| - | --------------- | ---------------------------------- |
|   | Total runtime   | O(log A + log B)                   |
|   | Total memory    | O(A+B)                             |



### 4) Optimize
We know that a and b can't be the permutations of one another if they aren't the same length. Therefore, if we compare string lengths first, we can reduce (or at least simplify) the runtime of the algorithm to O(log N), where N is length of either string.

If we don't care about preserving a and b, we could sort them destructively which would reduce memory footprint to O(1). For the purposes of this exercise, I will assume that we need to preserve the inputs.


### 5) Walk Through
So, adding the length check we're left with:

| # |      Step       |              Runtime               |
|---|-----------------|------------------------------------|
| 1 | Compare lengths | O(1)                               |
| 2 | Sort string a   | O(log A)                           |
| 3 | Sort string b   | O(log B)                           |
| 4 | Compare a and b | O(1)                               |
| - | --------------- | ---------------------------------- |
|   | Total runtime   | O(log N)                           |
|   | Total memory    | O(A+B)                             |


### 6) Implement

#### Ruby
```ruby
def is_permutation?(a, b)
  return false unless a.length == b.length
  a.split('').sort == b.split('').sort
end
```

#### JavaScript
This gets a little messier in JavaScript since we can't directly compare arrays in our return statement. We have a few options to overcome this, some include:
   1. Stringify both values
   2. Scan both arrays, comparing item-by-item
   3. Rejoin the arrays back into strings
   
For simplicity and readability's sake, I'll just rejoin the arrays.

```javascript
function isPermutation(a, b) {
  if(a.length != b.length) return false;
  
  var sorted_a = a.split("").sort().join();
  var sorted_b = b.split("").sort().join();
  return sorted_a === sorted_b;
}
```

### 7) Test

Using our example from earlier we'll walk through `is_permutation?('something', 'thoseming')`:

It passes through the length check at the beginning. Once sorted and split, `a` looks like `["e", "g", "h", "i", "m", "n", "o", "s", "t"]`, as does `b`, so the equality returns true.


