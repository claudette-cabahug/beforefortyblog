## Working With 2-Dimensional Integer Array

### Introduction

Multidimensional arrays have always been a little bit tricky for me. So when I encountered a challenge involving 2-dimensional arrays in [HackerRank](https://www.hackerrank.com/), I thought of it as a good opportunity to sharpen and scale up my problem solving skills with this kind of arrays .

### Problem
The function to write is called `truckTour(petrolpumps)`. Imagine a series of petrol pumps and a petrol truck will have to go to each one of them in a circle. At every pump, you need to account for 2 things to make sure that the truck would be able to circle around all the pumps:

> 1. the amount of petrol that the current petrol pump will give
> 2. the distance from the current petrol pump to the next one (one kilometer for each liter of petrol)

These 2 information are given in the parameter `petrolpumps`. Thus, in the test case `[ [ 1, 5 ], [ 10, 3 ], [ 3, 4 ] ]`, each one of the inner arrays represents a pump. It follows that each index `[0]` of the inner arrays stands for the petrol amount, and index `[1]` refers to the distance between the current pump to the next.

The expected output is an integer representing the smallest index of the pump from which the truck can start and complete the tour.

### Solution
```
let pp0 = [ [ 1, 5 ], [ 10, 3 ], [ 3, 4 ] ] // [petrol, distance]

function truckTour(petrolpumps) {
  
  for (let i = 0; i < petrolpumps.length; i++) {  // track which index you are trying - start
    let petrolTracker = 0
    for (let j = i; true; j++) {

      if (j === petrolpumps.length) {
        j = 0
      }

      if (j === i - 1 || (i === 0 && j === petrolpumps.length - 1)) {
        return i
      }

      petrolTracker = (petrolTracker + petrolpumps[j][0]) - petrolpumps[j][1]
      if (petrolTracker < 0) {
        break
      }
    }
  }

  return -1 // return this if nothing satisfies condition
}

console.log(truckTour(pp0)) // 1
```
### Explanation
The solution above iterates through each item of the outer array to check if starting in a particular pump will enable the truck to complete the tour of all the pumps. Thus, the outer for loop iterates through the length of `petrolpumps` array like so:
```
for (let i = 0; i < petrolpumps.length; i++) {  // track which index you are trying - start
    let petrolTracker = 0
    for (let j = i; true; j++)
```

Then you would need a second pointer that would move around the pumps to check the previously stated conditions (petrol, distance). For this, I created a variable to track the current amount of petrol when it reaches a particular pump, taking into account the petrol consumption to get there; thus, `petrolTracker`. At the instance that `petrolTracker` falls below `0`, the inner loop ends and goes back to the outer for loop to check the next element in the outer array.
```
petrolTracker = (petrolTracker + petrolpumps[j][0]) - petrolpumps[j][1]
      if (petrolTracker < 0) {
        break
      }
```

You might notice that the inner for loop's condition is set to true: `for (let j = i; true; j++)`. The length here is not as simple as setting it to `petrolpumps.length` because the loop goes back to the index where it started. Also, the length mutates every time the outer for loop moves to the next element. Remember that the truck needs to go to each pump in a circle, meaning that where `i` starts at index `[0]`, the iteration needs to go back to index `[0]`, the pump where it started. 

```
      if (j === petrolpumps.length) {
        j = 0
      }

      if (j === i - 1 || (i === 0 && j === petrolpumps.length - 1)) {
        return i
      }
```

Thus, the above conditions are added. If the inner for loop reaches index `[petrolpumps.length]`, technically an index beyond `petrolpumps.length`, it will be set to index `[0]` to make the truck do a full circle of all the pumps. 

The second if statement returns the index of `i` where the loop started after iterating through to the last index  of `j`. This means that the truck has completed the circle and the current index of `i` is the smallest index of the pump from which you can start and finish the tour.

Finally, in the event that the for loops do not find any index that meets the conditions of the problem, the function will return -1, which is a convention to say nothing was found.

### Conclusion
I found iterating through an array in a circle really challenging. It helps to understand the problem thoroughly before jumping into a solution. What works for me is writing down the test case in a sheet of paper and then comparing it with the expected output. This way, I can translate an abstract problem into a more concrete illustration. In this problem, I made use of nested for loops and unlocked how to loop through an array in a circle. I also experienced using `true` as a condition in a for loop, but emphasis must be made on providing further conditions to avoid an infinite loop.








