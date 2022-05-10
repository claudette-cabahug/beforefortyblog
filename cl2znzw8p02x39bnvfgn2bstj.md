## Optimizing Code to Meet Execution Limits

### Introduction
I have been progressing through my [HackerRank](https://www.hackerrank.com/) interview preparation kit and the problems have been getting harder and harder. One particular problem, though, demanded more time than most because, to put it bluntly, my code just wasn't good enough. I know, it is quite challenging when you feel like you have come up with the most elegant code, only for the compiler to tell you in the end that 4 out of 11 test cases failed because "Time Limit Exceeded -- Your code did not execute within the time limits. Please optimize." Thus, I was presented with another task -- how to make my code return an output within the preset time-limit. In short, I need to make it run faster and more efficient. 

So here is the problem, and it's called **New Year Chaos**.

> People are lining up for a rollercoaster ride. Each person has an initial position, and this identifies them as they move forward or backward in the queue, and as some bribe the person directly in front of them and positions are swapped. At most, a person can bribe two others.

> Given a queue order, determine the minimum number of bribes made and print the count. If the result is more than two, print 'Too chaotic'.

### First Solution
```
function minimumBribes(q) {
  let finalPosition = [...q]  // copy the array before you sort it -- The spread-syntax as array literal
  let initialPosition = q.sort((a, b) => a - b) 
  let bribesArr = []
  let positiveNums = []

  for (let i = 0; i < finalPosition.length; i++) {
    for (let j = 0; j < initialPosition.length; j++) {
      let difference
      
      if (i === j) {
        difference = finalPosition[i] - initialPosition[j] 
        bribesArr.push(difference)     
      }
    } 
  }

  positiveNums = bribesArr.filter(x => Math.sign(x) === 1) 

  let exceedBribeLimit = positiveNums.filter(x => (x >= 3)) 
  
  if (exceedBribeLimit.length !== 0) {
    return 'Too chaotic'
  } else {
    return positiveNums.reduce((a, b) => a + b)
  }
}

console.log(minimumBribes([2, 1, 5, 3, 4])) // 3
console.log(minimumBribes([2, 5, 1, 3, 4])) // Too chaotic
```

Here, I thought of having a reference for the `initialPosition` so I have something to compare with later on to the given `finalPosition`. After encountering many roadblocks in getting the correct array to sort, the use of the spread-syntax as array literal proved to be very helpful, and I must say it blew my mind, I might have to write another blog about it. Anyways, I believed I could get the number of positions moved by each element by getting the difference of the `finalPosition` and the `initialPosition` using their index and then pushing the values to `bribesArr`. Then I filtered the array to get the positive numbers and push them to `positiveNums` array. This represents the number of forward movements of each element in the queue. The `positiveNums` array is then filtered further to get the elements that moved more than or equal to `3`, and the values are stored in `exceedBribeLimit`. If the length of `exceedBribeLimit` is not equal to zero, it means that at least one element made more than three bribes, thus the function will return 'Too chaotic'. Otherwise, it will just return the count.
 
This solution satisfied most of the test cases, but one edge case stood out: `[1, 2, 5, 3, 7, 8, 6, 4]`. Apparently, my code does not account for instances when a person who bribed was also bribed in return. This is the case of `6` here. Also, I later realized that I could have just used the value of `q` directly in conjunction with methods and saved execution time by not having to loop through too many arrays. 

### Second Solution
```
function minimumBribes(q) {
  let numbersBribed
  let totalBribes = []
  let sum
  
  for (let i = 0; i < q.length; i++) {
    numbersBribed = q.slice(i).filter(x => x < q[i]).length
    totalBribes.push(numbersBribed)
  }

  if (totalBribes.find(element => (element >= 3))) {
    return 'Too chaotic'
  } else {
    return sum = totalBribes.reduce((a, b) => a + b)
  }
}

console.log(minimumBribes([1, 2, 5, 3, 7, 8, 6, 4])) // 7
```
Now this is the part that got me really excited, because I finally got the edge case above covered. To fix my first solution, I thought of getting the numbers that moved forward in the index and then proceeding to slice the array at those numbers to create new arrays with different lengths, which I will then filter to get `numbersBribed`. The logic is that any number lower than a number that bribed and positioned after it in the index would have been bribed. 

Where `[1, 2, 5, 3, 7, 8, 6, 4]` is the given queue, the swapping is as follows:

- `5` bribes `3` and `4`
- `7` bribes `6` and `4`
- `8` bribes `6` and `4`
- `6` bribes `4`
  
Here, `6` bribed `4` but, in turn, it was also bribed by `7` and `8`.

However, the next challenge for me is how to get all the remaining 4 test cases that return 'Terminated due to timeout' to pass.

### Final Solution
```
function minimumBribes(q) {
  let sum = 0

  for (let i = 0; i < q.length; i++) {
    if (q[i] - (i + 1) > 2) {
      console.log('Too chaotic')
      return
    }
    
    for (let j = Math.max(0, q[i] - 2); j < i; j++) {
      if (q[j] > q[i])
        sum++
    }
  }

  console.log(sum)
}

minimumBribes([5, 1, 2, 3, 7, 8, 6, 4]) // Too chaotic
minimumBribes([1, 2, 5, 3, 7, 8, 6, 4]) // 7
``` 
Here is the sweet code that passes all tests, fast and furious. It involves looping through every element in the given queue and asking two questions:

1. Has the element moved more than 2 positions forward (bribed another)?
2. How many times has the element been pushed backward (bribed by another)?

The essence of this approach is to catch first the elements that would make the 'Too chaotic' requirement. As shown below, once the outer for loop finds an element that moved more than 2 positions forward, the `return` will stop the execution of the function immediately and specify the string 'Too chaotic' as the value to be returned. Thus, there is no need to proceed further and it saves execution time. It also helped to use index-matching when setting the condition to make the values apples to apples for doing operations and comparisons.

```
for (let i = 0; i < q.length; i++) {
    if (q[i] - (i + 1) > 2) {
      console.log('Too chaotic')
      return
    }
```
If the first condition is not satisfied, function execution will proceed to the inner for loop,  which essentially counts the number of times elements bribed by another have been pushed backward. 

```
for (let j = Math.max(0, q[i] - 2); j < i; j++) {
      if (q[j] > q[i])
        sum++
    }
```
### Conclusion
There are many ways to speed up code performance. What is illustrated here is the reduction of activities in loops. When iterating, each statement inside the loop is executed, so if you could find a way to minimize iterations or stop it once a required limit is achieved, use it. Another thing I realized I could do to arrive at an optimized code is to avoid unnecessary variables (arrays in my first solution), as this only creates loading problems. An efficient code is surely ideal, but it bears stressing that it starts with a code that works, many, many tweaks in the process, and a lot of patience.






