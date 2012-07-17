#cfscript styleguide

##Goals
* Be easy to read.
* Be consistent with other languages. Mostly JavaScript (or ECMAScript) given its similarities to cfscript syntax.
* Be consistent with other developers - locally and in the community at large.
* Be concise.

##Variables

###General
Use camelCase for most variables. Use PascalCase for components. Use camelCase for an instance of a component.

```
import "lib.objs.*";
var myObj = new MyObject();
```

Do not abbrev. words. Favor `myDescriptiveVariableName` over `myDescVar`.

Do not use hungarian notation. Favor `notes` over `arrNotes` and `redButton` over `btnRed`.

Variables that will only contain a boolean should begin with is or the plural form has such as `isActive` or `hasItems`.

###Scope Variables
ColdFusion has several scopes like other languages built for the web such as `application`, `session`, `url`, `form`, `cgi`, and `cookie`. It might seem to make sense to capitalize these special scope variables to make them stand out, but given that ColdFusion is case insensetive, we use lowercase to save time, especially given that references to `application` and `session` can be quite common. Do not use the arguments scope. Variables that exist in the arguments scope will be found without referencing their scope directly, and the omission of direct references to the arguments scope keeps the syntax consistent with other languages and enhances code brevity.

##Functions

###General
A function declaration should specify the accessibility and return type before the function name, mostly for purposes of documentation. There should always be a space before the first curly brace and it should be on the same link of the function declaration. Likewise each argument should always specificy it's type and requiredness.

```
private void function myFunction(required array arr) {
    
}
```

###Built-in Functions
You will see in the ColdFusion documentation the global that the built-in ColdFusion functions are PascalCase. This is meant to differentiate the built-in functions from user-defined functions. This is wrong. PascalCase is for classes, components, and possibly modules, but never for functions.

###Primitive Functions
Some functions that do simple operations such as refactor data structures do not need complex and descriptive variable names. In these cases the appropriate names for variables are as follows:

string: str
numeric: num
array: arr
struct: s
query object: q

##Loops
Loop iterators variables should be ordered as follows: `i`, `j`, `k`. If you need more than three nested loops, you can use `m` and `n`. `len` is a common variable for the length of an array but I shorten this to `l`. A length variable for an inner loop can be `len2` or `l2`. Remember, this is ColdFusion so most times we need to be consistent with a zero-based array index. `arrayLen(arr)` and `arr.size()` (the java equivalent) will give the same result, but since the `size()` method is undocumented and therefore uncommon in third-party code, opt for the `arrayLen()` function instead. Also, given that CF does not have block scope, we can define all our length variables above the loop. You should always cache the length of an array before using it in a loop (unless the lengths will change within the loop).

```
var l = arrayLen(arr);
var l2 = arrayLen(arr2);
var l3 = arrayLen(arr3);
for (var i = 1; i <= l; i++) {
    for (var j = 1; j <= l2; j++) {
        for (var k = 1; k <= l3; k++) {
            // do something
        }  
    }
}
```

Many times we need to find something in a loop. The appropriate variable for this is `found`, not `isFound` as most booleans would be named. `found` should always default to false. If we need to keep track of a count that is not the number of iterations in the loop, the correct variable to use for this is `count`.

```
var found = false;
var count = 1;
var i = 1;
while (!found) {
    if (condition1) {
        count++;
    }
    if (condition2) {
        found = true;
    }
    i++;
}
```

##Formatting

###Indentation
There are many benefits to indenting using spaces or "soft tabs" which include avoidance of an unweildy mix of tabs and spaces. Currently most of the ColdFusion community uses real tabs so we still use real tabs. Any tabs or soft tabs should be set at four spaces, always. It is generally a good idea to be consistent with this spacing in your html, as well.

###Quotes
Always use double quotes for strings and string literals. Single quotes are only appropriate when having to use double quotes within a string. Creating markup with tag attributes is a good example of an appropriate use of single quotes.

```
var html = '<div class="red"></div>';
```

##Comments

###General
Comment your code well, but there's no need to go crazy. Good code should be able to be understood without a comment describing each line. Comments should appear as a short, correctly formatted sentence above the block of code it references. Periods are optional but capitalization is encouraged.

```
// Delete the user information from the session
structDelete(session, "currentUser");
```
Comments can occasionally appear at the end of the line to describe variable declarations, loops, or conditional statements. In this case, the comment would not be capitalized.

```
var foobar = 53; // a foobar
```

###Functions
Comments should appear at the top of each function or method. Use a style similar to javadocs but with the @hint line declaring the a description of the function since ColdFusion recognizes the hint attribute internally as the function description. A function (hint) should always begin with a plural verb.

```
/**
 * @hint Finds a list of videos.
 */
public void function findAll() {
    
}
```

For more complex functions, or for instances where more documentation is needed, a full javadocs style comment may be preferred.

```
/**
 * @hint Converts a query object to an array of structs.
 *
 * @param  query A query object.
 * @return array An array of structs.
 */
public array function queryToArrayOfStructs(required query q) {
    
}
```

##Custom Tags
Do not ever use custom tags. Porting them between projects requires unnecessary overhead and their functionality should almost always be built into custom reusable components instead.







