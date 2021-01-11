---
title: "Chunk Compact Concat"
date: 2021-01-09T13:57:13+03:00
draft: false
---

## Writing Lodash Part 1

While working on javascript most of the developers needs some changes on their `array` or `object`. So to make this changes on those parts often times we as developers install npm packages becuase they help alot.<!--more--> Spesificly today I'm talking about [Lodash](https://github.com/lodash/lodash) as you can see from the title. Often times I see in the projects developers use lodash for 1-2 functions and can be easily writeable functions.

In the projects sometimes we need 1 function to just merge some object or check something in array. And open source community has a big coverage of our needs. So this time I'm trying to show how you can write 3 of the lodash functions instead installing the package.

> Disclaimer: Functions may / may not cover all of the scenarios but they can be extended. I'm not saying don't use lodash. I'm just using the library for a reference and you can see my shitty code versus community code 😆.

## \_.chunk {#chunk}

Chunk function is for the dividing array with the given lenght. For example
`[1,2,3,4]` and given lenght of chunk is `2` the output will be `[[1,2],[3,4]`

The usage in lodash is `_.chunk([1,2,3,4],2)` so we are gonna try to replicate the function writing ourselves.

```javascript
Array.prototype.chunk = function (chunkSize = 1) {
  if (this.length < 1) {
    return [];
  }
  const copyArray = [...this];
  const arraySize = Math.ceil(this.length / chunkSize);
  return Array.from({ length: arraySize }, (v, k) => {
    return copyArray.splice(0, chunkSize);
  });
};
```

Function is used by array so we are extending array functions for ourselves to use it anywhere we want.

On the line with `copyArray` is to have a exact copy of the array so our methods will not change the original array. Creating `arraySize` is to find out the output size of the returning array.
By using `Array.from` we are creating an output array. 2nd Paramater of the `Array.from` is for iterating each element in the created array and returns the value it needs to be put in to that index. `splice` is extracting values from array and it's from index and count of how many from that index. `splice` function returns extracted values and changes original array so `copyArray` is changed but the original stays the same.

## \_.compact

Compact function is for the filtering any falsy value in the array and returning filtered array. For example `[0, 1, false]` as given array and it will return `[1]`.

> Disclaimer: falsy values are `0`, `""`, `undefined`, `false`, and `NaN`.

The usage in lodash is `_compact([0, 1, false])` so we are gonna try to replicate the function.

```javascript
Array.prototype.compact = function () {
  return this.filter((e) => !!e);
};
```

Function is used by array so we are extending array functions for ourselves to use it anywhere we want.

We have 1 liner solution in here with the `filter` function. `filter` function returns an array from the given array that returning boolean value from callback inside it. The boolean value returned is for: deciding if the value is kept or not. If it's `true` it'll be kept on returning array and if it's `false` it will be discarded. With `!!` we are converting any value to it's truthy value. So the filter function will return only `true` ones which are the values we want.

## \_.concat

Concat function is for merging all given paramaters into a 1 array and return. Since javasscript already has concat function the only difference between javascript concat and lodash concat:
lodash concat doesn't require any array to start concatting. But the sake of writing again lodash functions here's our solution.

```javascript
function concat(...values) {
  return values.reduce((state, val) => {
    return state.concat(val);
  }, []);
}
```

With the `spread, ...values` operator given parameters in function are merged in to an array. `reduce` is for the creating returning value from given array while iterating every value and making new output. We are merging our base array with every value it's given to use from the paramaters and returning the new merged array. At the end of reduce we'll have all values merged in 1 array.

And this is my shitt code as you see and I'm all open for feedback please leave a comment right [here](https://github.com/Ketcap/Ketchup-Blog/issues/2) if you have questions or points you wanna explain.

Thank you for reading the post.

Have a great day/night 😆
