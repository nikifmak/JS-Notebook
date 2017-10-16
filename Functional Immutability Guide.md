# Functional Immutability Guide For ReactJS

One the most important aspects of functional programming is immutability. 

State in ReactJS is immutable.

All examples with use the following ReactJS code:

> ````jsx
> function NumberList({numbers}) {
>   return (
>     <ul>
>     {numbers.map(n => <li key={n.id}>{n.id}</li>)}
>     </ul>
>   );
> }
>
> function Button({action}) {
>   return (
>     <button onClick={action}>+</button>
>   );
> }
>
> class TestComponent extends React.Component {
>   constructor(props) {
>     super(props);
>     this.state = {
>       showEven: true,
>       numbers: [
>         {id: getRandomNumber1To100()},
>         {id: getRandomNumber1To100()},
>         {id: getRandomNumber1To100()}
>       ]
>     }
>     this.doSomethingToState = this.doSomethingToState.bind(this);
>   }
>   doSomethingToState() {
>   }
>   render() {
>     const {numbers} = this.state;
>     return (
>       <div>
>         <h1>Random Numbers generator</h1>
>         <Button action={this.doSomethingToState}/>
>         <NumberList numbers={numbers}/>
>       </div>
>     );
>   }
> }
> ReactDOM.render(<TestComponent />, document.getElementById("application"));
>
> function intObject() {
>   return {id: getRandomInt()}
> }
>
> function getRandomArbitrary(min, max) {
>   return Math.floor(Math.random() * (max - min + 1)) + min;
> }
>
> function getRandomNumber1To100() {
>   return getRandomArbitrary(1,100);
> }
> ````

so lets explore the different ways to maniplutate immutable data only by changing **`doSomethingToState()`** function. 

## Adding elements to Array

### 1 - completely wrong

The `push` will mutate the state directly and that could potentially lead to error prone code. 

- Also, `setState` is async and React can queue several state changes into a single render pass. 
- This approach has a major flaw. If you call `setState` twice before the render cycle, the second call will clobber the first call.

> ````javascript
> doSomethingToState() {
>   this.state.numbers.push({id: getRandomInt()})
>   this.setState({numbers: this.state.numbers});
> }
> ````

### 2 - correct with array *Array.prototype.slice()*

The **`slice()`** method returns a shallow copy of a portion of an array into a new array object selected from `begin` to `end`(`end` not included). The original array will not be modified.	

> ```javascript
>   doSomethingToState() {
>     const newNumbers = this.state.numbers.slice();
>     newNumbers.push(intObject());
>     this.setState({numbers: newNumbers});
>   }
> ```

In this way we don't mutate the initial state. Instead we create a new shallow copy of the initial array. The memory "waste" is not an issue compared to the errors you might face using non-standard state modifications.

### 3 - correct with array *Array.from()*

The **`Array.from()`** method creates a new `Array` instance from an array-like or iterable object.	

> ```javascript
>   doSomethingToState() {
>     let numbers = Array.from(this.state.numbers);
>     numbers.push(intObject());
>     this.setState({numbers});  // ES6 syntax for ({numbers: numbers})
>   }
> ```

### 4 - correct with array *ES6 destructing*

> ```javascript
>   doSomethingToState() {
>     this.setState({numbers: [...this.state.numbers, {id: getRandomInt()}]});
>   }
> ```

Lets see what is happening behind the scenes with the spread operator ( … ).

1. this.state.numbers initially is an array of objects: [ {...}, {…}, …. ]
2. with the spread operator … —> …this.state.number = {...}, {…}, …. 
3. we add the new object on the array of objects and assign it to *property* numbers.

**Important: that fails on batch updates**: 

>```javascript
>alterState = () => {
>    this.addNewSome("121");
>    this.addNewSome("321");
>    this.addNewSome("213");
>  }
>  
>  addNewSome = (obj) => {
>    console.log(obj);
>    this.setState({array: [...this.state.array, obj]})
>  }	
>```

 ### 5 - with *Array.prototype.concat()*

The **`concat()`** method is used to merge two or more arrays. This method does not change the existing arrays, but instead returns a new array.

> ```javascript
> doSomethingToState() {
>   this.setState({
>     numbers: this.state.numbers.concat({id: getRandomInt()})
>   });
> }
> ```

### 6 - with *callback function on setState()* 

Now we also gonna rewrite our initial code to be more concise:

> ```jsx
> class List extends React.Component {
>   constructor(props) {
>     super(props);
>
>     this.state = {
>       array: []
>     }
>     setTimeout(this.addToArray, 500);
>   }
>
>   addToArray = () => {
>     this.doSomethingToState("a");
>     this.doSomethingToState("b");
>     this.doSomethingToState("c");
>   }
>
>   doSomethingToState = (something) => {
>       
>   }
>
>   render() {
>     const array = this.state.array.map((item, i) => <li key={i}>{item}</li>);
>     return (
>       <div>
>         <h2>Hahah</h2>
>         <button onClick={this.addToArray}></button>
>         <ul>
>           {array}
>         </ul>
>       </div>
>     );
>   }
> }
>
> ReactDOM.render(<List />, document.getElementById("application"));
> ```

React's setState function can get a callback function:

> ```javascript
> doSomethingToState = (something) => {
>   this.setState((state) => ({array: state.array.concat(something)}));
> }
> // or ES5
> doSomethingToState = (newObj) => {
>       this.setState(function(state) {
>         return {array: state.array.concat(newObj)};
>       });
>   }
> ```

We pass a function that takes as parameter the state (this.state) and we return a new array with the new object concatinated.

Alternatively, because *.concat()* concatinates arrays:

> ```typescript
> doSomethingToState = (something) => {
>   this.setState((state) => ({array: state.array.concat([something])}));
>  }
> ```

## Removing elements from state

### 1 - with *Array.prototype.filter()* 

We remove the element with given id. By using *filter()* only the objects that dont have the given id remain on the array because they return ***true*** when tested for *item.id !== id*.

> ```javascript
> // es6
> doSomethingToState = (id) => {
> 	this.setState({array: this.state.array.filter(item => item.id !== id)})
> }
>
> // es5
> removeObject = function(id) {
>     this.setState({array : this.state.array.filter(function(item) {
>         return item.id !== id;
>     });
> }
> ```

