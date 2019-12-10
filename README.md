https://fihis.github.io/presentation/

# JavaScrip Closures by Anton Ausiyevich (transcript)

JavaScript is a very function-oriented language. And closures are important in functional programming.

To explain closures, I will start with the fundamental terms: scope and lexical scope. These concepts are crucial to closures.

### 1. The scope

When you define a variable, you want it accessible within some boundaries. 

[next slide]

The accessibility of variables is managed by scope. You are free to access the variable defined within its scope. But outside of that scope, the variable is inaccessible.

In JavaScript, a scope is created by a function or code block.

[next slide]

Let’s see how the scope affects the availability of a variable count. This variable belongs to a scope created by the function foo():

[next slide]

function foo() {
  // The function scope
  let count = 0;
  console.log(count); // logs 0}

foo();
console.log(count); // ReferenceError: count is not defined
count is freely accessed within the scope of foo().

[next slide]

However, outside of the foo() scope, count is inaccessible. If you try to access count from outside anyways, JavaScript throws error.

So scope isolates its variables. That’s great because different scopes can have variables with the same name.

You can reuse common variables names (count, index, current, value, etc) in different scopes without collisions.

[next slide]

In the next example    foo() and bar() function scopes have their own, but same named variables count:

function foo() {

  // "foo" function scope

  let count = 0;

  console.log(count); // logs 0}



function bar() {

  // "bar" function scope

  let count = 1;

  console.log(count); // logs 1}



foo();

bar();

[next slide]

count variables from foo() and bar() function scopes do not collide.

### 2. Scopes nesting

[next slide]

Also, we can put one scope into another.

[next slide]

The function innerFunc() is nested inside an outer function outerFunc().

function outerFunc() {

  // the outer scope

  let outerVar = 'I am outside!';



  function innerFunc() {

    // the inner scope

    console.log(outerVar); // => logs "I am outside!"  }



  innerFunc();

}



outerFunc();

[next slide]

Despite that, outerVar variable is accessible inside innerFunc() scope. So the variables of the outer scope are accessible inside the inner scope.

### 3. The lexical scope

But how does JavaScript understand that outerVar inside innerFunc() corresponds 
to the variable outerVar of outerFunc()?

[next slide]

It’s because JavaScript implements a scoping mechanism named lexical scoping (or static scoping). 
Lexical scoping means that the accessibility of variables is determined by the position of the variables 
in the source code inside the nesting scopes.

Simpler, the lexical scoping means that inside the inner scope you can access variables of its outer scopes.

It’s called lexical (or static) because the engine determines the nesting of scopes just by looking at the JavaScript source code, without executing it.

[next slide]

Here’s how the engine understands the previous code snippet:

1.	I can see you define a function outerFunc() that has a variable outerVar.
2.	
3.	Inside the outerFunc(), I can see you define a function innerFunc().
4.	
5.	Inside the innerFunc(), I can see a variable outerVar without declaration. Since I use lexical scoping, I consider the variable outerVar inside innerFunc() to be the same variable as outerVar of outerFunc().
6.	
The distilled idea of the lexical scope:

The lexical scope consists of outer scopes determined statically. 

[next slide]

### 4. The closure

Ok, the lexical scope allows to access the variables statically of the outer scopes. 

[next slide]

function outerFunc() {

  let outerVar = 'I am outside!';



  function innerFunc() {

    console.log(outerVar); // => logs "I am outside!"

  }



  innerFunc();}



outerFunc();

Note that innerFunc() invocation happens inside its lexical scope (the scope of outerFunc()).

[next slide]

Let’s make the adjustments to the code snippet:

function outerFunc() {

  let outerVar = 'I am outside!';



  function innerFunc() {

    console.log(outerVar); // => logs "I am outside!"

  }



  return innerFunc;}



const myInnerFunc = outerFunc();

myInnerFunc();

Now innerFunc() is executed outside of its lexical scope. And what’s important:

innerFunc() still has access to outerVar from its lexical scope, even being executed outside of its lexical scope.

In other words, innerFunc() closes over (a.k.a. captures, remembers) the variable outerVar from its lexical scope.

Now we can define: 

The closure is a function that has access to it’s lexical scope even executed outside of its lexical scope.

Simpler, the closure is a function that remembers the variables from the place where it is defined, regardless of where it is executed later.

[next slide]

In the previous code snippet, outerVar is a variable inside the closure innerFunc() captured from outerFunc() scope.

### 5. Closure examples

[next slide]

I will continue with examples that demonstrate why the closure is useful.







5.1 Event handler

[next slide]

let countClicked = 0;



myButton.addEventListener('click', function handleClick() {

  countClicked++;

  myText.innerText = `You clicked ${countClicked} times`;

});

In this example of event handler
when the button is clicked, handleClick() is executed somewhere inside of the DOM code. 
The execution happens far from the place of definition.

[next slide]

But being a closure, handleClick() captures countClicked from the lexical scope and updates it when a click happens. 

[next slide]

5.2 Callbacks

Capturing variables from the lexical scope is useful in callbacks.

const message = 'Hello, World!';



setTimeout(function callback() {

  console.log(message); // logs "Hello, World!"

}, 1000);

[next slide]

The callback() is a closure because it captures the variable message.

[next slide]

5.3 Functional programming

Currying happens when a function returns another function until the arguments are fully supplied.

[next slide]

function multiply(a) {

  return function executeMultiply(b) {

    return a * b;

  }

}



const double = multiply(2);

double(3); // => 6

double(5); // => 10



const triple = multiply(3);

triple(4); // => 12

multiply is a curried function that returns another function.

Currying is also possible thanks to closures.

[next slide]

executeMultiply is a closure that captures a from its lexical scope. When the closure is invoked, the captured variable a and the parameter b are used to calculate a * b.

[next slide]

### 6. Closure analogy

The closure contains all the variables that are in the scope at the time of creation of the function. It is analogous to a backpack.

[next slide]

 A function definition comes with a little backpack. And in its pack it stores all the variables that were in scope at the time that the function definition was created.

[next slide]

That’s all. Thank you for attention.


