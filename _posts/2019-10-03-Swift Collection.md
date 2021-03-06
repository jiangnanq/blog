---
title: Swift Collection
date: 2019-10-03
layout: post
tags: Swift, iOS
---


# Collection

* To iterate an array with Struct
You cannot change property of struct in loop. 
During the loop, you are operate the copy of the struct.
Old loop method should be used for it. 

```swift
for index in 0..<nearbyTrolleys.count {
    nearbyTrolleys[index].distance = nearbyTrolleys[index].location.distance(from: currentlocation)
}
```

* Check item exist in array

```swift
var b:[String] = ["a", "b", "c", "d", "e"]
b.contains("a")
```

* Filter item in array

```swift
var a = [1, 2, 2, 3, 4, 5]
let a3 = a.filter{$0 == 2}
print(a3)
```

* Sort array

```swift
var a = [1, 2, 2, 3, 4, 5]
let d = a.sorted(by: >)
```

* Map 

```swift
let a4 = [1,3,5,7]
let b4 = a4.map { (item:Int) -> Int in
    return item * 2
}
```

* Reduce

```swift
let status = [true, true, true, true]
var s1 = status.reduce(true) { (r, s) -> Bool in
  return r && s
}
```

* To check an item exist in Array

```swift
if Array(sensorsid.keys).contains(messageid){
              processSensorMessage(json: json, index: messageid)
}
```

* To find the index of element

```swift
let arr = ["a", "b", "c"]
let indexofA = arr.firstIndex("a")
```
