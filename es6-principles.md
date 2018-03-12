#Principles of Readability
### String matching
Since precision and speed always matter, and Regular Expressions are both more precise and faster,
String matching really should be*Regular Expression matching*.

### Strings as key maps

When provided a string variable `s`, `s` may be one of a multitude of values that predicate a value or set of values.
In this situation, the semantic keyword to think about is `mapping`. Further, if you find yourself repeating yourself, remember
the Don't Repeat Yourself (DRY) coding principle.

**Bad:**
```
let output;
output = s === "keycodeA" ? "expectedValueA": null;
output = s === "keycodeB" ? "expectedValueB": null;
output = s === "keycodeC" ? "expectedValueC": null;
output = s === "keycodeD" ? "expectedValueD": null;  

```

**Good option:**

```
codes = {
    keycodeA: "expectedValueA"
    keycodeB: "expectedValueA"
    keycodeC: "expectedValueA"
    keycodeD: "expectedValueA"
}
...
let output = this.codes[s];

```

**Good option:**

```
getOutput (s) => {
    switch(s) {
        case "keycodeA": 
            return "expectedValueA"
        case "keycodeB": 
             return "expectedValueB"
        case "keycodeC": 
             return "expectedValueC"
        case "keycodeD": 
             return "expectedValueD"
         default:
            return '';
    }
}
...
const output = this.getOutput(s)

```

The second option may be ideal if you're not sure you'll get a string you haven't accounted for. But
there are options you could include for the first option.

