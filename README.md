# React Event Handling
Event Handling with Parameter.

There are two ways to bind event handler in jsx. 

**1. Using bind function in constructor.**
```
  export default class SomeClass extends React.Component{
    constructor(props){
      super(props)
      this.clickMe = this.clickMe.bind(this); 
    }

    clickMe(event){
      //do stuff
    }

    render(){
      <div>
        <button onClick={this.clickMe}>Click</button>
      </div>
    }
  }
```
   
**2. Using arrow function in code.**
```
  export default class SomeClass extends React.Component{
    constructor(props){
      super(props)
    }
      
    clickMe = (event) =>{
      //do stuff
    }
      
    render(){
      <div>
        <button onClick={this.clickMe}>Click</button>
      </div>
    }
  }
```
  
**But What if we want to pass some parameters in the bound function?**
  
We can use currying in arrow function.
  
```
  export default SomeClass extends React.Component{
    constructor(props){
      super(props)
    }

    clickMe = (param) => (event) =>{
      //do stuff
    }

    render(){
      <div>
        <button onClick={this.clickMe('someParam')}>Click</button>
      </div>
    }
  }
```
However in this scenario, each time ```render()``` gets called, ```clickMe()``` returns a new function.

We can solve this issue with a memoization function.

```
  export function memoiz(funct){
    let cache = {};
    return function (){
      let aArgs = Array.prototype.slice.call(arguments);
      if(cache[aArgs]){
        return cache[aArgs];
      }else{
        return (cache[aArgs] = funct.apply(this, aArgs));
      }
    }
  }
```
Now we can use this function into our component.

```
  export default SomeClass extends React.Component{
    constructor(props){
      super(props)
    }

    clickMe = memoiz((param) => (event) =>{
      //do stuff
    })

    render(){
      <div>
        <button onClick={this.clickMe('someParam')}>Click</button>
      </div>
    }
  }
```
