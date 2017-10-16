# Fetching Data with ReactJS

## Fetch

> ```javascript
> fetch('products.json')
>     .then(function(response) { return response.json(); })
>     .then(function(json) {
>        console.log(json.products.length);
>     });
> ```

or for URLs:

> ```javascript
> fetch("https://api.github.com/repos/facebook/react")
>     .then(function(response) {
>       return response.json();
>     }).then(function(json) {
>         console.log(json);
>     });
> ```

Lets explain what is happening in the chaining.

The *<u>first then</u>*  returns a *<u>Promise</u>* object which we chain with <u>*second then*</u> which returns the return of the first promise.

## ES6 flavour

> ```javascript
>   fetch("https://api.github.com/repos/facebook/react")
>     .then((response) => response.json())
>     .then(function(json) {
>         console.log(json);
>     });
> ```

Or even better

> ```javascript
> fetch("https://api.github.com/repos/facebook/react")
>     .then((response) => response.json())
>     .then((json) => {
>       console.log(json)
>     });
> ```