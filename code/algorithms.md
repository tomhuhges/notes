### Contents

- [Insertion sort]()  
- [Loop invariants]()  

----

### Insertion sort

**best case:** O(n) - data is already sorted  
**worst case:** O(n²) - data is reverse of sorted order  
**average case:** O(n²) - insertion sort performs well on very small arrays, even better than quicksort. implementations of quicksort may switch to insertion sort when data size is below a certain threshold.  

**comments:**  Although it is one of the elementary sorting algorithms with O(n²) worst-case time, insertion sort is the algorithm of choice either when the data is nearly sorted (because it is adaptive) or when the problem size is small (because it has low overhead).

For these reasons, and because it is also stable, insertion sort is often used as the recursive base case (when the problem size is small) for higher overhead divide-and-conquer sorting algorithms, such as merge sort or quick sort.

```js
const insertionSort = (arr, compareFunction) => {
  // loop through the array starting at index 1
  for (let i = 1; i < arr.length; i++) {
    // cache the current element value
    const curr = arr[i];
    // j will keep track of the element's position in the array, which at the moment is i
    let j = i;
    // our compareFunction will compare our current element and the one before it in the array (arr[j - 1])
    // the while loop will run so long as we haven't hit the beginning of the array and so long as
    // the compareFunction returns true, indicating the element needs swapping with the one before it
    while (j > 0 && compareFunction(arr[j - 1], curr)) {
      // if the element needs swapping, put the element before our current element in our current element's place
      // it doesn't matter if we overwrite our current element because we've already cached it's value
      arr[j] = arr[j - 1];
      // decrement j to go one further down the array and rerun the loop to compare that element
      j--;
    }
    // once we've broken out of the loop, j represents the final position where our current element should be placed
    // so we put our cached value in arr[j]
    arr[j] = curr;
  }
  // once we've looped through every element, our array is sorted.
  return arr;
}

```

----

### Loop invariants

we can infer that our insertion sort implementation is correct with a loop invariant. the invariant must be true at 3 stages in the loop's lifecycle. these are:  

1. at the start of the loop, before it has run.  
2. after each iteration of the loop.  
3. after the loop has ended.  

for our insertion sort implementation, we can us the following as our loop invariant:  

> subarray `arr[0]` to `arr[i - 1]` is sorted

we should be able to prove this at each stage.

1. before the loop runs, we set `i` equal to 1. since `i - 1` is 0, we can prove that the subarray `arr[0]` - `arr[0]` is sorted because there is only one element in that range and therefore it must be sorted.  
2. during the loop, the algorithm moves `arr[i - 1]` to `arr[i]` so long as the compare function requires it. it then inserts the original `arr[i]` in `arr[i - 1]` in order to sort them. otherwise the two elements remain where they are because they're already sorted. so we can say that the subarray `arr[0]` to `arr[1]` is sorted and the invariant holds after the first iteration of the loop. since we're only moving one element into place during each loop and only incrementing `i` by one each time, we can also say that the invariant holds after each loop.  
3. the loop terminates when `i` is equal to `arr.length`. we need to prove that the subarray `arr[0]` to `arr[arr.length - 1]` is sorted. based on our observations of the subarray's state after each loop iteration, we know this subarray is sorted. we also know that the subarray is actually the whole array, and therefore the entire array is sorted and the algorithm is correct.

----
