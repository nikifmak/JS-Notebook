# High Order Javascript Functions

>  **High Order Functions** are called the functions that accept functions as arguments. The passed functions are sometimes referred to as the *callback functions.*

## <u>IndexOf()</u>

```javascript
var array = ['apple', 'kiwi' , 'orange'];

console.log(array.indexOf('apple')); // 0
console.log(array.indexOf('lemon')); // -1

var basket = ['apple', 'apple', 'orange']; 

console.log(basket.indexOf('apple')); // 0
```

## <u>map()</u>

> The map operator accepts three values in the callback function, namely:
>
> - The current item of the array
> - The current index of the current item
> - The entire array

### ∀ element —> same operation

### @return —> new array with same number of elements

Example 1:

```javascript
var numbers = [1, 5, 10, 15];
var double_numbers = numbers.map((x) => x * 2); // [2, 10, 20, 30]
// Or without arrow function
var double_numbers = numbers.map(function(x) { return x * 2; });
```

Example 2:

```javascript
const animals = [
    {
        "name": "cat",
        "size": "small",
        "weight": 5
    },
    {
        "name": "dog",
        "size": "small",
        "weight": 10
    },
    {
        "name": "lion",
        "size": "medium",
        "weight": 150
    },
    {
        "name": "elephant",
        "size": "big",
        "weight": 5000
    }
];

let animal_names = [];

for (let i = 0; i < animals.length; i++) {
    animal_names.push(animals[i].name);
}
```

Example 2-map:

```javascript
let animal_names2 = [];

animals.map((x) => animal_names2.push(x.name));
console.log(animal_names2); // [ 'cat', 'dog', 'lion', 'elephant' ]
```

Example 2-map-better: 

```javascript
let animal_names3 = animals.map((animal, index, animals) => {
    return animal.name
})
console.log(animal_names3);  // [ 'cat', 'dog', 'lion', 'elephant' ]
```

While this is a very easy example, there are some key improvements in our code readability when we use the `map`:

1. Using the `map`, we don’t have to manage the state of our for-loop.
2. We don’t have to use the index of the for-loop to access the correct item in the array.
3. We don’t have to create a new array and push our values into it. `Map`returns the finished array in one go, so we can simply assign it to a variable.

There is very important thing which you may never forget when using the map. Use a `return` statement, otherwise your array will end up with all items as `undefined`.

## <u>filter()</u>

The filter operator accepts the same parameters (current item, index and entire array) in the callback function. But since we don’t use the index and the entire array, i’ve left them out. 

### ∀ element —> same criteria

### @return —> new array with the elements that match the criteria

Example 1-for:

```javascript
let small_animals = [];

for (let i = 0; i < animals.length; i ++) {
    if (animals[i].size === "small") {
        small_animals.push(animals[i])
    }
}
console.log(small_animals); // [ { name: 'cat', size: 'small', weight: 5 },
						   //  { name: 'dog', size: 'small', weight: 10 } ]
```

Example 1-filter:

```Javascript
let small_animals = animals.filter((animal) => {if(animal.size === "small") return animal;});
console.log(small_animals); // same as before
```

Example 1-filter-better:

```javascript
let small_animals = animals.filter((animal) => {
    return animal.size === "small"
})
```

Example 2:

```javascript
let fruits = [
  { name: 'apple', color: 'red', origin: 'Europe'},
  { name: 'raspberry', color: 'red', origin: 'Europe'},
  { name: 'damson', color: 'blue', origin: 'Europe'},
  { name: 'pear', color: 'green', origin: 'Europe' },
  { name: 'pomegranate', color: 'red', origin: 'Europe' },
  { name: 'banana', color: 'yellow', origin: 'Asia' },
  { name: 'orange', color: 'orange', origin: 'Asia' },
  { name: 'apricot', color: 'apricot', origin: 'Africa' },
  { name: 'cherimoya', color: 'green', origin: 'Americas'},
  { name: 'avocado', color: 'dark green', origin: 'Americas'}

];
let redEuroFruits = fruits.filter(function(fruit) {
  return (fruit.color === 'red' && fruit.origin === 'Europe');
});

console.log(redEuroFruits);
/* [ { name: 'apple', color: 'red', origin: 'Europe' },
  { name: 'raspberry', color: 'red', origin: 'Europe' },
  { name: 'pomegranate', color: 'red', origin: 'Europe' } ]*/
```

Example 3-filter-map-chaining:

```javascript
// es6
let greenFruitNames =
  fruits
  	.filter((fruit) => (fruit.color === 'green'))
  	.map((fruit) => fruit.name);

  console.log(greenFruitNames);

// not es6
let greenFruitNames = 
  fruits
    .filter(function(fruit) {
      return fruit.color === 'green'
    })
	.map(function(fruit) {
      return fruit.name
	});
```

Example 4-find-non-dublicate-values-of-array

```javascript
for(var i = 0; i < array.length; i++) {
    if(array.indexOf(array[i]) === i) {
        models.push(array[i]);
    }
}
```



The `filter-operator` also has the same other benefits as the `map`. We also have to use the `return` again to make the `filter` work properly. But this time we have to make sure that the `return` returns a boolean value. If you don’t do this the filter will always return false.

## <u>reduce()</u>

Let’s say that we want to calculate the combined weight of all the animals in our array. Using a `for-loop` we can write something like this:

Example 1-for:

```javascript
let total_weight = 0;
for (let i=0; i<animals.length; i++) {
  total_weight += animals[i].weight;
}
console.log(total_weight); // 5165
```

Example 1-reduce:

```javascript
let total_weight = animals.reduce((weight, animal, index, animals) => {
    return weight += animal.weight
}, 0)
```

Example 2:

```javascript
var animals = [
  { name: ‘Waffles’, type: ‘dog’, age: 12 },
  { name: ‘Fluffy’, type: ‘cat’, age: 14 },
  { name: ‘Spelunky’, type: ‘dog’, age: 4 },
  { name: ‘Hank’, type: ‘dog’, age: 11 },
];

var totalDogYears = animals
  .filter((x) => x.type === ‘dog’)
  .map   ((x) => x.age)
  .reduce((prev, cur) => { prev + cur }, 0);
  // totalDogYears will be the integer 27 (Because Waffles 12 +  
  // Spelunky 4 + Hank 11)
```

The parameters in the callback function work a bit different than `map` and `filter`. It works as follows:

- The first parameter is the current value of the end value. We have to set the initial value at the end of the function. In this case we set it to `0`. This could be any value though.
- The second parameter is the current item in the array.
- The third is the index again.
- The last is the full array.

Again we have all the readability improvements of the `map` and `filter`. We also need to use the `return` again. This time it’s important the we return the end value, the first parameter, at the end of each `reduce` function.

*Reduce* sounds scary, but it’s actually a ridiculously simple function — it just iterates over all values in the array, passing in the value returned from the prior run as the first value (*prev* in the above example) and the currently iterated value (*cur* in the example) as the second. It’s handy for summing things up like we do a above — the name of the function comes from *reducing *a list to a single value. For the first iteration, there is no previous value of course, so *prev* will be the second argument passed to *reduce*, 0 in the above case.
