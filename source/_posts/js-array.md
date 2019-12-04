---
title: JavaScript Array
date: 2016-11-22 22:26:02
categories: JavaScript
tags: [ES5, ES6]
---

# Array manipulation

**`w3cschool`** is a good place to learn basic things, and [mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) documents are more detailed.
<!--more-->

## Array.prototype.filter()
The filter() method creates a new array with all elements that pass the test implemented by the provided function.
``` js
// Filtering invalid entries from JSON
// The following example uses filter() to create a filtered json of all elements with non-zero, numeric id.

var arr = [
  { id: 15 },
  { id: -1 },
  { id: 0 },
  { id: 3 },
  { id: 12.2 },
  { },
  { id: null },
  { id: NaN },
  { id: 'undefined' }
];

var invalidEntries = 0;

function filterByID(obj) {
  if (obj.id !== undefined && typeof(obj.id) === 'number' && !isNaN(obj.id)) {
    return true;
  } else {
    invalidEntries++;
    return false;
  }
}

var arrByID = arr.filter(filterByID);

console.log('Filtered Array\n', arrByID); 
// Filtered Array
// [{ id: 15 }, { id: -1 }, { id: 0 }, { id: 3 }, { id: 12.2 }]

console.log('Number of Invalid Entries = ', invalidEntries); 
// Number of Invalid Entries = 4
```

## Array.prototype.sort()
The sort() method sorts the elements of an array in place and returns the array. 
``` js
// Remove duplicates id
function uniqueObject(a, compareFunc){
	a.sort(compareFunc);
	for(var i = 1; i < a.length; ){
	    if(compareFunc(a[i-1], a[i]) === 0){
	        a.splice(i, 1);
	    } else {
	        i++;
	    }
	}
	return a;
}

var uniq = uniqueObject(temp.concat(ret), function(a, b){
	if (a.id > b.id) {
	  return 1;
	}
	if (a.id < b.id) {
	  return -1;
	}
	return 0;
});

// Sort by time
uniq.sort((a, b)=>{
	if (a.id > b.id) {
	  return 1;
	}
	if (a.id < b.id) {
	  return -1;
	}
	return 0;
});

uniq.forEach((v,i)=>{
	let detail = {
	  id:v.id,
	  message: v.SUMMARY,
	  date: new Date(Date.now() + (10 * 1000)),
	  number: i+1
	};
	LocalNotification.localNotificationSchedule(detail);
});
```

## Array.prototype.splice()
The splice() method changes the content of an array by removing existing elements and/or adding new elements.

Remove 0 elements from index 2, and insert "drum"
``` js
var myFish = ["angel", "clown", "mandarin", "surgeon"];
var removed = myFish.splice(2, 0, "drum");

// myFish is ["angel", "clown", "drum", "mandarin", "surgeon"] 
// removed is [], no elements removed
```
Remove 1 element from index 3
``` js
var myFish = ["angel", "clown", "drum", "mandarin", "surgeon"];
var removed = myFish.splice(3, 1);

// removed is ["mandarin"]
// myFish is ["angel", "clown", "drum", "surgeon"]
```
Remove 1 element from index 2, and insert "trumpet"
``` js
var myFish = ["angel", "clown", "drum", "surgeon"];
var removed = myFish.splice(2, 1, 'trumpet');

// myFish is ["angel", "clown", "trumpet", "surgeon"]
// removed is ["drum"]
```
Remove 2 elements from index 0, and insert "parrot", "anemone" and "blue"
``` js
var myFish = ["angel", "clown", "trumpet", "surgeon"];
var removed = myFish.splice(0, 2, "parrot", "anemone", "blue");

// myFish is ["parrot", "anemone", "blue", "trumpet", "surgeon"] 
// removed is ["angel", "clown"]
```
Remove 2 elements from index 2
``` js
var myFish = ["parrot", "anemone", "blue", "trumpet", "surgeon"]
var removed = myFish.splice(myFish.length -3, 2);

// myFish is ["parrot", "anemone", "surgeon"] 
// removed is ["blue", "trumpet"]
```
Remove 1 element from index -2
``` js
var myFish = ["angel", "clown", "mandarin", "surgeon"];
var removed = myFish.splice(-2, 1);

// myFish is ["angel", "clown", "surgeon"] 
// removed is ["mandarin"]
```
## Array.prototype.slice()
``` js
var a = ["zero", "one", "two", "three"];
var sliced = a.slice(1,3);

console.log(a);      // [ "zero", "one", "two", "three" ]
console.log(sliced); // [ "one", "two" ]
```
## Array.prototype.slice()
``` js
var a = ["zero", "one", "two", "three"];
var sliced = a.slice(1,3);

console.log(a);      // [ "zero", "one", "two", "three" ]
console.log(sliced); // [ "one", "two" ]
```
## Array.prototype.shift()
The shift() method removes the first element from an array and returns that element. This method changes the length of the array.
``` js
var a = [1, 2, 3];
a.shift();

console.log(a); // [2, 3]
```
## Array.prototype.unshift()
The unshift() method adds one or more elements to the beginning of an array and returns the new length of the array.
``` js
var a = [1, 2, 3];
a.unshift(4, 5);

console.log(a); // [4, 5, 1, 2, 3]
``` js
## Array.prototype.push()
The push() method adds one or more elements to the end of an array and returns the new length of the array.
``` js
var a = [1, 2, 3];
a.push(4, 5);

console.log(a); // [1, 2, 3, 4, 5]
```
## Array.prototype.pop()
The pop() method removes the last element from an array and returns that element. This method changes the length of the array.
``` js
var a = [1, 2, 3];
a.pop();

console.log(a); // [1, 2]
```

## Array.prototype.indexOf()
The indexOf() method returns the first index at which a given element can be found in the array, or -1 if it is not present.
``` js
var a = [2, 9, 9]; 
a.indexOf(2); // 0 
a.indexOf(7); // -1

if (a.indexOf(7) === -1) {
  // element doesn't exist in array
}

static unique(array){
    var n = [];//临时数组
    for(var i = 0;i < array.length; i++){
      if(n.indexOf(array[i]) == -1) 
        n.push(array[i]);
    }
    return n;
}
```

# What is the difference between .map, .every, and .forEach in JavaScript array?

.map() returns a new Array of objects created by taking some action on the original item.

.every() returns a boolean - true if every element in this array satisfies the provided testing function. An important difference with .every() is that the test function may not always be called for every element in the array. Once the testing function returns false for any element, no more array elements are iterated. Therefore, the testing function should usually have no side effects.

.forEach() returns nothing - It iterates the Array performing a given action for each item in the Array.

.find() method returns a value of the first element in the array that satisfies the provided testing function. Otherwise undefined is returned.


