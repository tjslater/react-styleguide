# Principles of readability
### Ternaries in templates
#### Part A
While a nested ternary in standard javascript is difficult to read:

**Bad:**
`const x = value !== 5 ? value === 4 ? 3 : 2 : 1;`

Inside of a react template, it is that much more difficult to read and maintain for other people.
If a portion of a template is predicated on such values, consider moving the test to a method and expand the logic into
elseif syntax.

**Better:**
```
getXValue(value) => {
    if (value === 5){
        return 1;
    } else if (value === 4){
        return 3;
    } else {
        return 2;
    }
}
...
const x = this.getXValue(value); 
```

If the block is significantly large, you may even consider moving that portion of a template to a child component, 
and pass its dependent values and methods in. It may take more lines of code, but having as few lines of code as possible *is not the goal of readable, maintainable code.*

`<ChildComponent x={x} getXValue={this.getXValue} />`

This way, not only is it easier to maintain for yourself, it's easier for others to read.

#### Part B

Due to javascript's typing system, multiple falsey tests are not required.

**Bad:**
`(value !== ''  value !== undefined && value !== null) ? <div>{value}</div>: <div>{anotherValue}</div>`

**Good:**
`(value) ? <div>{value}</div>: <div>{anotherValue}</div>`


#### Part C
If a portion of a template is predicated on whether a variable matches a given value, and the alternate option is `null` or `''`,
*There is no need for a ternary operation*.

`truthyValue && <div>...</div>`


## Variable declaration
#### Part A
It is a common principle to declare as many of your variables as you can at the top of a function or block scope.
In Javascript classes, at the top of the class or assigned values in the constructor.
React is no different with its constructor and its state object. A good thing to remember for keeping code readable is
to keep it semantically correct. a `render` method isn't called `initializeDeclareAssignAndRender` method, so the business
logic of determining values should be kept out of the render method as much as possible. This is in keeping with [Clean Code Javascript](https://github.com/ryanmcdermott/clean-code-javascript):

- Keep functions simple
- Avoid side effects
- Function name and what it does should be semantically transparent.

**Bad:**

```
class SmartComponent extends React.Component {

  render(){
    const {props} = this;
    const newValue = props.constant * 2 
    return (
        <div>
        {newValue}
        
        </div>
    )
  }
}
```

**Good:**
```
class SmartComponent extends React.Component {

  constructor(props) {
    super(props);
    const newValue = this.descriptiveFunctionName();
    this.state = {
        newValue: newValue    
    }
  }  
  descriptiveFunctionName() { 
    const {constant} = this.props;
    return constant * 2;  
  }

  render(){
    const {newValue} = this.state;
    return (
        <div>
        {newValue}
        </div>
    )
  }
}
```

#### Part B
In keeping with the variable declaration at the top of the scope, remember also to keep as many of your declared values
to a Smart Component that's typically the parent scope and pass those values into  its children, keeping as many of them
as Dumb Components as you can. This is so you can not only keep track of all your dynamic variables in one place, but it
it removes the possibility for unintended side effects in the children, and also allows other people on your team to see
more clearly exactly what's going on.*Keep in mind, this isn't true for generic containers that hold elements that are consistent between pages.
Such as the navigation and the footer.*

**Bad:**
```
class SmartComponent extends React.Component {

constructor(props) {
  super(props);
  this.state = {};
}

render() {
    return (
        <ChildComponent value={this.props.value} />    
    )

}
...
class ChildComponent extends React.Component {
    constructor(props) {
    const newValue = props.value * props.value;
     this.state = {
        value: newValue
     }
    }
    ...
}

```

**Good:**
```
class SmartComponent extends React.Component {
    super(props);
    const newValue = props.value * props.value;
    this.state = {
        value: newValue;
    }
    render() {
        const {newValue} = this.state;
        return <ChildComponent newValue={newValue/>
    }
}

const ChildComponent = (newValue) => {...}
```

### Documenting code
Consider[JSDoc](http://usejsdoc.org/howto-es2015-classes.html) standards.
One doesn't need to use all of them, but a short description plus @param and @return documentation is a big help.
