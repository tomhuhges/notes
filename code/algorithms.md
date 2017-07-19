### Insertion sort

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
