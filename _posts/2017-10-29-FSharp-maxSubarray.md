---
layout: post
title: Maximum subarray problem
tags: F#
published: true
---

There is known [Maximum subarray problem](https://en.wikipedia.org/wiki/Maximum_subarray_problem).
I implemented F# algorithm for that.
<!-- more -->
There is actually one implementation of that in [FSSnip](http://www.fssnip.net/fh/title/Maximum-Sublist-Problem), but I wanted to return indexes as well, not maximum subarray by itself.

Here is code:

```F#
let leftFold item (leftSum, sum, maxLeft, i) = 
    let newSum = sum + item
    if newSum > leftSum 
    then (newSum, newSum, i, i)
    else (leftSum, newSum, maxLeft, i - 1)

let maxCrossingSubarrayLeft (arr: int list) low mid =
    let result = 
        List.foldBack leftFold arr.[low .. mid] (int.MinValue, 0, 0, mid - low)
    let leftSum, _, maxLeft, _ = result
    (maxLeft + low, leftSum)

//------------------------------------------------------

let rightFold (rightSum, sum, maxLeft, i) item = 
    let newSum = sum + item
    if newSum > rightSum 
    then (newSum, newSum, i, i)
    else (rightSum, newSum, maxLeft, i + 1)

let maxCrossingSubarrayRight (arr: int list) mid high =
    let result = 
        arr.[mid .. high] 
        |> List.fold rightFold (int.MinValue, 0, 0, 0)
    let leftSum, sum, maxRight, i = result
    (maxRight + mid + 1, leftSum)    


//------------------------------------------------------


let findMaxCrossingSubarray arr low mid high = 
    let maxLeft, leftSum = maxCrossingSubarrayLeft arr low mid
    let maxRight, rightSum = maxCrossingSubarrayRight arr (mid + 1) high
    (maxLeft, maxRight, leftSum + rightSum)

//------------------------------------------------------

let findMaximumSubaray arr =
    let rec findMaxSub (arr: int list)  low high = 
        if high = low then  
            (low, high, arr.[low])
        else
            let mid = (low + high) / 2
            let leftLow, leftHigh, leftSum = findMaxSub arr low mid
            let rightLow, rightHigh, rightSum = findMaxSub arr (mid + 1) high
            let crossLow, crossHigh, crossSum = findMaxCrossingSubarray arr low mid high
            if leftSum >= rightSum && leftSum >= crossSum then
                (leftLow, leftHigh, leftSum)
            elif rightSum >= leftSum && rightSum >= crossSum then
                (rightLow, rightHigh, rightSum)
            else
                (crossLow, crossHigh, crossSum)
    findMaxSub arr 0 (arr.Length - 1)

let data = [13; -3; -25; 20; -3; -16; -23; 18; 20; -7; 12; -5; -22; 15; -4; 7]
findMaximumSubaray data
```

It is based on assumption that if we divide the list to two parts, the maximum subarray will be on one of the parts or will be on the division point. What is left is to make some recursive functions to divide the divide parts and verify the same. 

I'd say that implementing that was not easy (in comparison to imperative languages), but again the List.fold rulez!