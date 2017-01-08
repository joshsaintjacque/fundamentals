## 1.1 Is Unique

### 1) Listen
> Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?


### 2) Example
Given some input `'quickness'` the function should return `true`. I'll use this example since it's long enough to get a sense that the code works correctly and won't terminate until the last character.


### 3) Brute Force
We could:
   1. Walk over the string
   2. Keep track of all characters found in a hash
   3. Terminate when a char is reached that's already in the hash (returning false), or when we reach the end of the string (returning true)

This would run in O(N) time and space, where N is the length of the string.


### 4) Optimize
For very large lengths of the string, the best conceivable runtime for this problem is O(N), since we have to touch every char in the string. However, a string can only have so many unique characters and, therefore, has a maximum N. If we assume this is  standard ASCII than there are 128 possible characters, and any string longer than that must be non-unique since it would have to repeat chars.

So, we can write this algorithm in such a way that, for large values of N, it runs in O(1) time and space.

We could get rid of the hash by sorting the string, I could go either way but I'll go with a sorting mechanism in order to write slightly less complicated code.


### 5) Walk Through
I'll implement a method that:
   1. Terminates and returns false if the length of the string is > 128.
   2. Sort the string.
   3. Walk over the string
   4. Terminate when a char is reached that's the same as the last char (returning false), or when we reach the end of the string (returning true)


### 6) Implement

#### Ruby
```ruby
def is_unique?(str)
  return false if str.length > 128 # Assuming ASCII char-set
  
  prev = ''
  str.chars.sort.each do |curr|
    return false if curr == prev
    prev = curr
  end
  
  true
end
```

#### JavaScript
```javascript
function isUnique(str) {
  if(str.length > 128) return false; // Assuming ASCII char-set
  
  var sorted = str.split("").sort();
  var prev = '';
  for(var i = 0; i < str.length; i++) {
    var curr = sorted[i];
    if(curr === prev) return false;
    prev = curr;
  }
  
  return true;
}
```

### 7) Test

Using our example from earlier we'll walk through is_unqiue?('quickness'):

It passes through the length check at the beginning. The execution of the loop looks like this:

| round | prev | curr |    result    |
|-------|------|------|--------------|
|     1 |      | c    | continue     |
|     2 | c    | e    | continue     |
|     3 | e    | i    | continue     |
|     4 | i    | k    | continue     |
|     5 | k    | n    | continue     |
|     6 | n    | q    | continue     |
|     7 | q    | s    | continue     |
|     8 | s    | s    | return false |


