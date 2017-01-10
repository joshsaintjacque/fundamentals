## Most Ones
An interesting problem suggested by a friend of mine, [Paul Taladay](https://github.com/PTaladay).

### 1) Listen
> Given a 2D matrix return the highest number of 1s in a given row. The matrix is nxm and each row is always sorted. Assume it also only contains 0 and 1.


### 2) Example
Given a matrix `[[0,0,0,1,1,1],[0,0,0,0,0,0],[0,0,1,1,1,1],[0,0,0,0,1,1]]`, the function should return `4`.


### 3) Brute Force
We could do something like the following:

| # |                 Step                 |     Runtime      |
|---|--------------------------------------|------------------|
| 1 | Iterate over each row                | O(N)             |
| 2 | Iterate over each column in the row  | O(M)             |
| 3 | Keep count of `1`s found in each row | O(1)             |
| 4 | Return the highest count value       | O(N)             |
|---|--------------------------------------|------------------|
|   | Total runtime                        | O(N<sup>M</sup>) |
|   | Total memory                         | O(N)             |

Where N is the length of the matrix, and M is the length of the rows (which I will assume to be the same for all rows).


### 4) Optimize
I struggled with this for nearly an hour before admitting I was stuck at O(N^M). I couldn't figure out what the significance of the rows being sorted was to the performance of the algorithm. After walking through it again with Paul he suggested something similar to the following:

As we iterate through each row we no longer care about indices higher than last one we found containing a one. So we can simplify this to O(N+M) by ignoring indices higher than we previously found for `1`.


### 5) Walk Through
So, now we're left with:

| # |                 Step                   |      Runtime      |
|---|----------------------------------------|-------------------|
| 1 | Walk each row                          | O(N)              |
| 2 | Start each new row at the highest `1`  | O(M)              |
|   | index found                            |                   |
| 3 | Return the highest `1` index           | O(1)              |
|---|----------------------------------------|-------------------|
|   | Total runtime                          | O(N+M)            |
|   | Total memory                           | O(1)              |


### 6) Implement

#### Ruby
```ruby
def max_ones(matrix)
  smallest_index_of_one = nil
  matrix.each do |row|
    term = smallest_index_of_one || row.length - 1
    
    # Look for the lowest index containing 1
    row.each_with_index do |col, i|
      break if i > term
      if row[i] == 1
        smallest_index_of_one = i
        break
      end 
    end
    break if smallest_index_of_one == 0
  end  
  
   matrix[0].length - smallest_index_of_one
end
```

#### JavaScript
```javascript
function maxOnes(matrix) {
  var smallestIndexOfOne = null;
  for(var i = 0; i < matrix.length; i++) {
    var row = matrix[i];
    var maxIndex = smallestIndexOfOne || row.length - 1;
    
    // Look for the lowest index containing 1
    for(var j = 0; j < maxIndex; j++) {
      if(row[j] === 1) {
        smallestIndexOfOne = j;
        break;
      }  
    }
    if(smallestIndexOfOne === 0) break;
  }
  
  return matrix[0].length - smallestIndexOfOne;
}
```

#### CSharp
```csharp
public static int MaxOnes(int[,] matrix)
{
  int smallestIndexOfOne = matrix.GetLength(1) - 1;
  for (int i = 0; i < matrix.GetLength(0); i++)
  {
    for (int j = 0; j < smallestIndexOfOne; j++)
    {
      if (matrix[i, j] == 1)
      {
        smallestIndexOfOne = j;
        break;
      }
    }
    if (smallestIndexOfOne == 0) break;
  }
  return matrix.GetLength(1) - smallestIndexOfOne;
}
```

### 7) Test

Using the test input `max_ones([[0,0,0,1,1,1],[0,0,0,0,0,0],[0,0,1,1,1,1],[0,0,0,0,1,1]])`:

We enter the outer block and begin with the first row in the inner loop, `[0,0,0,1,1,1]`.

| # | j | row[j] | smallestIndexOfOne | maxIndex |     result     |
|---|---|--------|--------------------|----------|----------------|
| 1 | 0 |      0 | null               |        5 | Continue       |
| 2 | 1 |      0 | null               |        5 | Continue       |
| 3 | 2 |      0 | null               |        5 | Continue       |
| 4 | 3 |      1 | 3                  |        3 | Go to next row |

The inner loop is repeated for `[0,0,0,0,0,0]`:

| # | j | row[j] | smallestIndexOfOne | maxIndex |     result     |
|---|---|--------|--------------------|----------|----------------|
| 1 | 0 |      0 | 3                  |        3 | Continue       |
| 2 | 1 |      0 | 3                  |        3 | Continue       |
| 3 | 2 |      0 | 3                  |        3 | Continue       |
| 4 | 3 |      1 | 3                  |        3 | Go to next row |

And the next row `[0,0,1,1,1,1]`:

| # | j | row[j] | smallestIndexOfOne | maxIndex |     result     |
|---|---|--------|--------------------|----------|----------------|
| 1 | 0 |      0 | 3                  |        3 | Continue       |
| 2 | 1 |      0 | 3                  |        3 | Continue       |
| 4 | 2 |      1 | 2                  |        2 | Go to next row |

Finally, the last row `[0,0,0,0,1,1]`:

| # | j | row[j] | smallestIndexOfOne | maxIndex |     result     |
|---|---|--------|--------------------|----------|----------------|
| 1 | 0 |      0 | 2                  |        2 | Continue       |
| 2 | 1 |      0 | 2                  |        2 | Continue       |
| 4 | 2 |      0 | 2                  |        2 | Break, since a previous row had `1` at a lower index |

`matrix[0].length` is `6` and `smallestIndexOfOne` is `2`, so we return `6 - 2` or `4`.


