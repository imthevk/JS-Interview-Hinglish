## Table of Contents

| No. | Questions                                                                                   |
| --- | ------------------------------------------------------------------------------------------- |
| 1   | [Event delegation](#1-event-delegation--इवेंट-डेलिगेशन-क्या-है)                             |
| 2   | [Null and Undefined](#2-null-and-undefined)                                                 |
| 3   | [What is Garbage Collection?](#3-what-is-garbage-collection-गार्बेज-कलेक्शन-क्या-है)        |
| 4   | [What is Hoisting?](#4-what-is-hoisting)                                                    |
| 5   | [Event Propagation](#5-event-propagation)                                                   |
| 6   | [The Importance of true in addEventListener](#6-the-importance-of-true-in-addeventlistener) |
| 7   | [What is Scoping? (स्कोपिंग क्या है?)](#7-what-is-scoping-स्कोपिंग-क्या-है)                 |
| 8   | [What are Closures? (क्लोजर्स क्या हैं?)](#8-what-are-closures-क्लोजर्स-क्या-हैं)           |
| 9   | [NaN](#9-nan)                                                                               |
| 10  | [Value vs. Reference](#10-value-vs-reference)                                               |
| 11  | [new and this keyword](#11-new-and-this-keyword)                                            |
| 12  | [call, apply and bind](#12-call-apply-and-bind)                                             |
| 13  | [var, let and const](#13-var-let-and-const)                                                 |
| 14  | [Temporal Dead Zone (TDZ)](#14-temporal-dead-zone-tdz)                                      |
| 15  | [Callback Function](#15-callback-function)                                                  |
| 16  | [Promises](#16-promises)                                                                    |

# Javascript Questions for Interviews

## 1. Event delegation / इवेंट डेलिगेशन क्या है?

`Event delegation` is a technique in web development where instead of attaching an event listener to each individual element, you attach a single event listener to a common ancestor. This way, you can handle events for multiple elements efficiently. Event delegation is particularly useful when dealing with dynamically generated content or a large number of elements.

मान लीजिए कि आपके पास एक सूची (list) है जिसमें हर आइटम पर क्लिक करने से कुछ होना चाहिए। इवेंट डेलिगेशन एक ऐसी तकनीक है जो इस काम को बेहतर तरीके से करने में मदद करती है। आइए देखते हैं कैसे:

### फ़ायदे:

- Traditional Approach (without delegation): You could attach an event listener directly to each element you want to respond to a particular event (e.g., a click). However, this becomes inefficient when you have many such elements or if you dynamically add new elements to the page.

- Event Delegation Approach: Instead of attaching listeners to individual elements, you attach a single event listener to a parent element that contains them. Then, due to a phenomenon called event bubbling, you can figure out which specific child element triggered the event and act accordingly.
- प्रदर्शन (Performance): कम इवेंट लिसनर का मतलब ब्राउज़र को घटनाओं (events) के समय कम काम करना, यानी बेहतर प्रदर्शन।
- डायनामिक एलिमेंट्स: बाद में जोड़े गए बटनों या तत्वों पर भी ये अपने आप काम करती है। आपको बार-बार अलग से इवेंट लिसनर जोड़ने की ज़रूरत नहीं।
- मेमोरी की बचत: कम इवेंट लिसनर होने से याददाश्त (memory) बचती है।

### Examples

```html
<ul id="myButtonList">
  <li>
    <button>बटन 1</button>
  </li>
  <li>
    <button>बटन 2</button>
  </li>
  <li>
    <button>बटन 3</button>
  </li>
</ul>
```

### पारंपरिक तरीका:

```js
const buttons = document.querySelectorAll("#myButtonList button");
buttons.forEach((button) => {
  button.addEventListener("click", () => {
    console.log("आपने एक बटन पर क्लिक किया!");
  });
});
```

### इवेंट डेलिगेशन का तरीका:

```js
const myButtonList = document.getElementById("myButtonList");
myButtonList.addEventListener("click", (event) => {
  if (event.target.tagName === "BUTTON") {
    console.log("आपने एक बटन पर क्लिक किया!");
  }
});
```

### Importance

- इवेंट बबलिंग: अधिकतर इवेंट्स DOM में ऊपर की ओर "बबल" होते हैं। यानी किसी चाइल्ड आइटम का इवेंट उसके सब पैरेंट्स पर भी ट्रिगर होगा।
- event.target: यह प्रॉपर्टी बताती है कि असल में इवेंट किस एलिमेंट ने पैदा किया था।

**[⬆ Back to Top](#table-of-contents)**

## 2. Null and Undefined

`Null` represents an intentional absence of value. It's an assignment value indicating that a variable does not point to any object.

`Null` का मतलब जानबूझकर किसी भी value का न होना है। यह एक ऐसा मान है जो यह दर्शाता है कि कोई वेरिएबल किसी वस्तु (object) की तरफ संकेत नहीं कर रहा है।

```js
let emptyValue = null;
console.log(emptyValue); // Output: null
```

`Undefined` signifies a variable that has been declared but hasn't been assigned a value yet.

`Undefined` का मतलब है कि कोई वेरिएबल घोषित तो किया गया है पर अभी तक उसे कोई मूल्य (value) नहीं दिया गया है।

```js
let unassignedVariable;
console.log(unassignedVariable); // Output: undefined
```

### Common Scenarios

#### Null

- Clearing the value of a variable.
- Representing "no data" from a database or API response.
- For optional function parameters.

#### undefined

- Unassigned variables
- Missing object properties
- Missing function return values

```js
let student = {
  name: "Alice",
  age: 25,
};

console.log(student.name); // Output: Alice
console.log(student.phoneNumber); // Output: undefined (property doesn't exist)

function calculateArea() {
  // ... no return statement here
}
let area = calculateArea();
console.log(area); // Output: undefined
```

#### Important Note:

- It's generally better to avoid relying on undefined. Try to explicitly initialize your variables whenever possible.

**[⬆ Back to Top](#table-of-contents)**

## 3. What is Garbage Collection? (गार्बेज कलेक्शन क्या है?)

`Simplified Analogy`: Imagine your computer's memory as a room. When you create variables and objects, it's like putting items and furniture in that room. Over time, when you're done with items and remove them from the room, some space becomes empty. Garbage collection is like an automatic cleaning service that periodically comes in, sees the empty spaces left by those gone items, and makes that space available for reuse.

`Technical Explanation`: Garbage collection is a process within programming languages that automatically reclaims memory occupied by objects that are no longer in use by the program. This makes space for creating new objects.

`सरल सादृश्य (Analogy)`: अपने कंप्यूटर की मेमोरी को एक कमरे के रूप में देखें। जब आप वेरिएबल्स और ऑब्जेक्ट बनाते हैं, तो यह उस कमरे में सामान और फर्नीचर रखने जैसा है। जब आप आइटम के साथ काम पूरा कर लेते हैं और उन्हें कमरे से हटा देते हैं, तो कुछ जगह खाली हो जाती है। गार्बेज कलेक्शन एक स्वचालित सफाई सेवा की तरह है जो समय-समय पर आती है, उन वस्तुओं द्वारा छोड़े गए खाली स्थानों को देखती है, और उस स्थान को पुन: उपयोग के लिए उपलब्ध कराती है।

`तकनीकी व्याख्या`: गार्बेज कलेक्शन प्रोग्रामिंग भाषाओं के भीतर एक प्रक्रिया है जो प्रोग्राम द्वारा उपयोग नहीं की जा रही वस्तुओं (objects) द्वारा घेरी गई मेमोरी को स्वचालित रूप से पुनः प्राप्त कर लेती है। इससे नई वस्तुओं को बनाने के लिए जगह बनती है।

#### Why is Garbage Collection Important?

`Prevents Memory Leaks`: Without garbage collection, programs that run for a long time would keep consuming memory from unused objects, eventually leading to crashes.

`Programmer Convenience`: Garbage collection relieves the programmer from the responsibility of manually freeing up memory.

##### Code Example (Illustrative – Not Exact Mechanism)

```js
function createPerson(name) {
  let person = { name: name };
  return person;
}

let alice = createPerson("Alice");

// ... some time later, we're done with alice
alice = null; // Remove the reference

// ... some more code runs

gc(); // Hypothetical garbage collection cleans up
```

**[⬆ Back to Top](#table-of-contents)**

## 4. What is Hoisting?

`Hoisting` is a behavior in JavaScript where declarations of variables and functions are conceptually "moved" to the top of their scope before the code is executed. It's important to understand that it's not literal movement of the code; it's an explanation of how JavaScript's interpreter seems to work regarding declarations.

`Hoisting`, JavaScript में एक व्यवहार है जिसमें वेरिएबल्स और फंक्शन्स की घोषणाओं (declarations) को उनके दायरे (scope) के शीर्ष पर ले जाया जाता है, इससे पहले कि वास्तव में कोड चलाया जाए। ध्यान रखें, कोड का शाब्दिक हलचल नहीं होता है। बल्कि यह इस बात की व्याख्या है कि जावास्क्रिप्ट के दुभाषिया (interpreter) घोषणाओं के मामले में कैसे काम करता है।

#### Key Points to Remember

- `Variables with var`: Declarations of variables using var are hoisted, but their initialization (assigning values) is not. This can lead to some surprising results if you aren't careful.

- `Variables with let and const`: Variables declared with let and const are also hoisted, but they are not accessible until their line of declaration is reached in the code execution.

- `Functions`: Both function declarations and traditional function expressions are hoisted to the top, allowing you to call them before the line where they appear in the code.

### Code Examples

##### var and Hoisting

````js
console.log(myVar); // Output: undefined  (Hoisted, but not yet initialized)
var myVar = "Hello";
```
````

##### let and const (No access before initialization)

```js
console.log(myLet); // Output: ReferenceError (not accessible before declaration)
let myLet = "Hello";

console.log(myConst); // Output: ReferenceError (same as above)
const myConst = "Hello";
```

##### Function Hoisting

```js
sayHello(); // Output: "Hello from Hoisting!"

function sayHello() {
  console.log("Hello from Hoisting!");
}
```

#### Best Practices to Avoid Confusion

- Declare variables at the top: Declare your variables (especially with var) at the beginning of their scope to reduce confusion.

- Preference for let and const: These have more predictable behavior due to not being accessible before initialization.

**[⬆ Back to Top](#table-of-contents)**

## 5. Event Propagation

When an event occurs on an element in the HTML DOM, it doesn't just trigger on that single element. The event "travels" through the DOM tree in two phases:

1. `Event Capturing Phase`: The event starts at the very top of the DOM (usually the document or window) and travels downwards towards the target element that triggered it.

2. `Event Bubbling Phase`: After reaching the target element, the event then "bubbles" back up the DOM tree towards the top, hitting the ancestors of the target element.

3. `कैप्चरिंग चरण`: घटना DOM के सबसे ऊपर से शुरू होती है (आमतौर पर document या window) और उस एलिमेंट की ओर नीचे जाती है जिसने इसे शुरू किया।

4. `बबलिंग चरण`: लक्ष्य एलिमेंट (target element) पर पहुंचने के बाद, यह घटना फिर "बबल" होकर लक्ष्य एलिमेंट के पूर्वजों को हिट करते हुए, वापस DOM ट्री के ऊपर चली जाती है।

```html
<div id="grandparent">
  <div id="parent">
    <button id="child">Click Me</button>
  </div>
</div>
```

```js
document.getElementById("grandparent").addEventListener(
  "click",
  (event) => {
    console.log("Grandparent (Capturing Phase)");
  },
  true
); // Use 'true' for capturing

document.getElementById("parent").addEventListener(
  "click",
  (event) => {
    console.log("Parent (Capturing Phase)");
  },
  true
);

document.getElementById("child").addEventListener(
  "click",
  (event) => {
    console.log("Child (Capturing Phase)");
    console.log("Child (Bubbling Phase)"); // Will fire twice
  },
  true
);

document.getElementById("parent").addEventListener("click", (event) => {
  console.log("Parent (Bubbling Phase)");
});

document.getElementById("grandparent").addEventListener("click", (event) => {
  console.log("Grandparent (Bubbling Phase)");
});
```

#### Key Points

1. `Default`: Bubbling is the default behavior
   event.target: Use the event.target property to identify the element on which the event initially occurred.

2. `stopPropagation()`: This method called within an event handler stops the event from propagating further.

**[⬆ Back to Top](#table-of-contents)**

## 6. The Importance of true in addEventListener

When you use the addEventListener function to attach an event listener to an element, the third argument (which is optional) controls whether you want to use the "capturing" or "bubbling" phase of event propagation.

```js
element.addEventListener(eventName, eventHandlerFunction, useCapture);
```

`eventName`: The type of event to listen for (e.g., 'click', 'mouseover').
`eventHandlerFunction`: The function to be executed when the event occurs.
`useCapture`: A boolean value (true or false) specifying the event propagation phase.

#### Cases to Use true

`Event Capturing`: Setting useCapture to true indicates that you want the event listener to be triggered during the capturing phase (when the event is traveling downwards in the DOM tree). This is a less common scenario, but can be useful when you want to intercept events before they reach their intended target.

#### Key Points

`Default is false`: If you omit the useCapture argument, it defaults to false, which means the event listener will use the bubbling phase.
`Most Cases Use Bubbling`: Typically, you'll handle events at the target element itself or during the bubbling phase, so the useCapture argument is often omitted.

**[⬆ Back to Top](#table-of-contents)**

## 7. What is Scoping? (स्कोपिंग क्या है?)

`Scoping` defines the visibility and accessibility of variables and functions within different parts of your code. Put simply, it's a set of rules determining where your variables and functions can be "seen" and used.

`स्कोपिंग` इस बात को निर्धारित करता है कि आपके प्रोग्राम के विभिन्न हिस्सों में वेरिएबल्स और फंक्शन्स कितने दृश्यमान (visible) हैं और उन्हें कितनी आसानी से उपयोग किया जा सकता है। सीधे शब्दों में कहें, तो यह नियमों का एक समूह है जो यह बताता है कि आपके वेरिएबल्स और फंक्शन्स को कहां "देखा" और उपयोग किया जा सकता है।

#### Types of Scopes in JavaScript

1. `Global Scope (ग्लोबल स्कोप)`: Any variable declared outside of any function has global scope and can be accessed from anywhere in your code.

2. `Function Scope (फंक्शन स्कोप)`: Variables declared inside a function (using var, let or const) are only accessible within that function.

3. `Block Scope (ब्लॉक स्कोप)`: With the introduction of let and const in modern JavaScript, we have block scope. Variables declared inside a block of code (usually enclosed in curly braces {}) are only accessible within that block.

```js
let globalVariable = "I'm global!";

function myFunction() {
  let functionVariable = "I'm inside the function";

  if (true) {
    let blockVariable = "I'm inside the block";
    console.log(globalVariable, functionVariable, blockVariable);
  }

  console.log(globalVariable, functionVariable);
  // console.log(blockVariable); // Error: blockVariable is out of scope
}

myFunction();
console.log(globalVariable);
// console.log(functionVariable); // Error: functionVariable is out of scope
```

#### Key Points

`Inner Scopes`: Code blocks within inner scopes can access variables from their outer scopes.
`Avoid global scope pollution`: Use function and block scopes to limit global variables and prevent potential naming conflicts.

**[⬆ Back to Top](#table-of-contents)**

## 8. What are Closures? (क्लोजर्स क्या हैं?)

A `closure` in JavaScript is the ability of an inner function to retain access to the variables in its outer (enclosing) function's scope even after the outer function has finished executing. In essence, a `closure` 'remembers' its birthplace with all its variables available at the time.

JavaScript में एक `क्लोजर` एक आंतरिक कार्य (inner function) की क्षमता है जो उसके बाहरी कार्य (outer/enclosing function) के परिवर्तनीय का उपयोग बंद के भीतर करने की अनुमति देता है, तब भी जब बाहरी कार्य अपना काम पूरा कर चुका हो। संक्षेप में, एक `क्लोजर` अपने जन्मस्थान को उस समय उपलब्ध सभी परिवर्तनीय के साथ 'याद' रखता है।

```js
## example 1

function outerFunction(name) {
  let message = "Hello, " + name + "!";

  function innerFunction() {
    console.log(message);
  }

  return innerFunction;
}

let greetAlice = outerFunction("Alice");
greetAlice(); // Output: "Hello, Alice!"
```

```js
## example 2

function createUserManager(name, initialScore) {
  let privateData = {
    name: name,
    score: initialScore,
  };

  return {
    incrementScore: function () {
      privateData.score++;
    },
    getScore: function () {
      return privateData.score;
    },
  };
}

const user1 = createUserManager("Bob", 5);
const user2 = createUserManager("Alice", 0);

user1.incrementScore();
user1.incrementScore();
console.log(user1.getScore()); // Output: 7

console.log(user2.getScore()); // Output: 0
```

#### Common Use Cases for Closures

- `Private Variables`: Closures help implement the concept of 'private' variables that aren't directly modifiable from outside (emulating encapsulation).
- `State Preservation`: For creating functions that maintain 'state' between calls (like counters).
- `Module Pattern`: Closures were commonly used to create module-like structures in JavaScript before official modules existed in the language.

**[⬆ Back to Top](#table-of-contents)**

## 9. NaN

`NaN` in JavaScript stands for "Not a Number". It's a special value that represents the result of an invalid or undefined mathematical operation.

Javascript में `NaN` का मतलब है "नॉट अ नंबर" (Not a Number)। यह एक विशेष मान (value) है जो किसी गलत या अपरिभाषित गणितीय क्रिया (operation) के परिणाम को दर्शाता है।

```js
let result = 42 / 0;
console.log(result); // Output: NaN

let result = "Hello" * 5;
console.log(result); // Output: NaN

let result = parseInt("xyz");
console.log(result); // Output: NaN

let result = 0 / 0;
console.log(isNaN(result)); // Output: true
```

#### Important Things to Remember

1. `NaN is 'sticky'`: Any operation with NaN usually results in NaN.
2. `NaN !== NaN`: Surprisingly, NaN is not equal to itself. For comparison, always use the isNaN() function.

**[⬆ Back to Top](#table-of-contents)**

## 10. Value vs. Reference

The way JavaScript assigns data to variables has important implications for how that data gets modified.

##### Value Types:

`Simple Data`: Things like numbers, strings, booleans, null, and undefined are assigned by value.
`Copy Mechanism`: When you assign a value-type variable to another, a copy of the actual value is made. Changes to one do not affect the other.

`सरल डेटा`: संख्याएं (numbers), स्ट्रिंग्स (strings), बूलियन्स (booleans), null, और undefined जैसी चीज़ें मूल्य (value) के अनुसार सौंपी (assign) जाती हैं।
`कॉपी की प्रक्रिया`: जब आप किसी वैल्यू-टाइप वेरिएबल को दूसरे को देते हैं, तो वास्तविक मूल्य की एक कॉपी बन जाती है। एक मूल्य में बदलाव का दूसरे पर कोई असर नहीं पड़ता।

```js
let num1 = 10;
let num2 = num1;

num2 = 20;

console.log(num1); // Output: 10 (remains unchanged)
console.log(num2); // Output: 20
```

##### Reference Types

`Complex Data`: Objects, arrays, and functions are assigned by reference.
`Pointer Mechanism`: A variable assigned an object doesn't store the object itself; it stores a reference (think of it like an address) pointing to the object's location in memory.
`Shared Memory Location`: If you modify the object through one variable, other variables referencing the same object see the change.

`जटिल डेटा`: ऑब्जेक्ट्स, एरेज़ (arrays), और फ़ंक्शंस रेफरेंस द्वारा दिए जाते हैं।
`पॉइंटर मैकेनिज्म`: जब एक वेरिएबल को किसी ऑब्जेक्ट को सौंपा जाता है, तो वह ऑब्जेक्ट को संग्रहीत नहीं करता है; बल्कि वह एक रेफरेंस संग्रहीत (store) करता है, जो मेमोरी में ऑब्जेक्ट के स्थान की ओर इशारा करता है।
`शेयर्ड मेमोरी लोकेशन`: यदि आप एक वेरिएबल का उपयोग करते हुए ऑब्जेक्ट में बदलाव करते हैं, तो उसी ऑब्जेक्ट के रेफ़रेंस करने वाले अन्य वेरिएबल्स भी बदलाव को देख पाएंगे।

```js
let person1 = { name: "Alice" };
let person2 = person1;

person2.name = "Bob";

console.log(person1.name); // Output: "Bob" (both changed)
console.log(person2.name); // Output: "Bob"
```

**[⬆ Back to Top](#table-of-contents)**

## 11. new and this keyword

#### The 'new' Keyword

- `Purpose (उद्देश्य)`: The new keyword is used in JavaScript to create new objects based on a constructor function.

- `Hindi Explanation (हिंदी व्याख्या)`: JavaScript में new कीवर्ड का उपयोग एक कंस्ट्रक्टर फ़ंक्शन के आधार पर नई वस्तुओं (objects) को बनाने के लिए किया जाता है।

```js
function Car(brand, model) {
  this.brand = brand;
  this.model = model;
}
let myCar = new Car("Honda", "Civic");
console.log(myCar.brand); // Output: "Honda"
```

#### The 'this' Keyword

- `Meaning (अर्थ)`: The this keyword in JavaScript refers to the object that the current function is a part of. The value of this changes depending on the context in which the function is called.

- `Hindi Explanation (हिंदी व्याख्या)`: JavaScript में this कीवर्ड उस वस्तु (object) को इंगित करता है जिसका वर्तमान फ़ंक्शन एक हिस्सा है। जिस संदर्भ में फ़ंक्शन को बुलाया जाता है, उसके आधार पर this का मान बदल जाता है।

```js
const myObject = {
  name: "My Object",
  greet: function () {
    console.log("Hello from " + this.name);
  },
};
myObject.greet(); // Output: "Hello from My Object"

function myFunction() {
  console.log(this); // In a browser, this will likely log the 'window' object
}

myFunction();

document.getElementById("myButton").addEventListener("click", function () {
  console.log(this); // This will refer to the button element
});
```

`Other contexts`: The behavior of this can sometimes be less obvious in cases like standalone functions, event handlers, etc.

#### Arrow Functions (=>) and this

`Arrow functions` have a special behavior regarding this. They do not create their own this binding; instead, they inherit this from the enclosing (surrounding) scope.

```js
const myObject = {
  name: "iphone 15",
  traditionalMethod: function () {
    console.log(this.name); // Output: "iphone 15"
  },
  arrowMethod: () => {
    console.log(this.name); // Output: Likely undefined or value from outside scope
  },
};

myObject.traditionalMethod();
myObject.arrowMethod();
```

#### Nested Functions and this

Within `nested functions`, this in the inner function usually refers to the global object (or undefined in strict mode), not the object of the outer function.

```js
function outerFunction() {
  this.name = "Outer";

  function innerFunction() {
    console.log(this.name); // Likely undefined or global object
  }

  innerFunction();
}

outerFunction();
```

**[⬆ Back to Top](#table-of-contents)**

## 12. call, apply and bind

1. **call()**

   - `Purpose`: Invokes a function immediately, allowing you to explicitly set what this refers to inside the function.

   - `Syntax`: functionName.call(thisArg, arg1, arg2, ...)
     <br/>
     `उद्देश्य`: यह किसी भी फंक्शन को तुरंत चलाने की सुविधा देता है। इसमें ये भी निर्धारित किया जा सकता है कि फंक्शन के भीतर this क्या दर्शाएगा।
     `सिंटैक्स`: functionName.call(thisArg, arg1, arg2, ...)

```js
const person = {
  fullName: function () {
    return this.firstName + " " + this.lastName;
  },
};

const person1 = { firstName: "Alice", lastName: "Sharma" };
const person2 = { firstName: "Bob", lastName: "Verma" };

person.fullName.call(person1); // Output: "Alice Sharma"
person.fullName.call(person2); // Output: "Bob Verma"
```

2. **apply()**

   - `Purpose`: Very similar to call(), but the way you pass arguments differs. It takes an array of arguments.

   - `Syntax`: functionName.apply(thisArg, [arg1, arg2, ...])
     <br/>
     `उद्देश्य`: काफी हद तक call() के समान लेकिन इसमें arguments (तर्क) अलग रूप में पास (pass) की जाती हैं। यह arguments की एक array को ही ले लेती है।
     `सिंटैक्स`: functionName.apply(thisArg, [arg1, arg2, ...])

```js
const person = {
  fullName: function (city, country) {
    return this.firstName + " " + this.lastName + ", " + city + ", " + country;
  },
};

const person1 = { firstName: "Alice", lastName: "Sharma" };

person.fullName.apply(person1, ["Delhi", "India"]);
// Output: "Alice Sharma, Delhi, India"
```

`Key difference between call() and apply()`: In call(), arguments are passed individually, whereas in apply(), they are passed as an array.

2. **bind()**

   - `Purpose`: Unlike call() and apply(), which execute the function immediately, bind() creates a new function with a specified this value and arguments, which can be invoked later.
   - `Syntax`: functionName.bind(thisArg, arg1, arg2, ...)
     <br/>
     `उद्देश्य`: यह call() और apply() से भिन्न है क्योंकि यह फंक्शन को तुरंत नहीं चलाता बल्कि एक नया फंक्शन बनाता है, जिसमें this और अन्य तर्कों को बाँध (bind) दिया जाता है, जिसे बाद में कभी भी चलाया जा सकता है।
     `सिंटैक्स`: functionName.bind(thisArg, arg1, arg2, ...)

```js
const person = {
  fullName: function () {
    return this.firstName + " " + this.lastName;
  },
};

const person1 = { firstName: "Alice", lastName: "Sharma" };

const getPerson1Name = person.fullName.bind(person1);

console.log(getPerson1Name()); // Output: "Alice Sharma"
```

`Usage of bind()`: It’s helpful when you need to pass a function as a callback with a specific this value, without immediately invoking it.

**[⬆ Back to Top](#table-of-contents)**

## 13. var, let, and const

1. **var**

   - `Scope`: var is function-scoped, meaning it is accessible within the function where it's declared, or globally if declared outside any function.
   - `Hoisting`: Variables declared with var are hoisted to the top of their scope, but their initialization is not. This means you can access the variable before its declaration, but it will be undefined until the line where it’s initialized.
   - `Re-declaration`: You can redeclare the same variable multiple times in the same scope.
     <br/>
     `Scope`: var का स्कोप फंक्शन-स्कोप्ड होता है, यानी यह उसी फंक्शन में उपलब्ध होता है जहाँ इसे घोषित किया गया है, या अगर फंक्शन के बाहर है तो ग्लोबल स्कोप में भी उपलब्ध होता है।
     `Hoisting`: var डिक्लेरेशन होस्ट होती है, यानी इसे डिक्लेयर करने से पहले एक्सेस किया जा सकता है, लेकिन इनिशलाइज़ नहीं किया जाएगा। इसे इनिशलाइज़ होने तक undefined माना जाएगा।
     `Re-declaration`: एक ही स्कोप में आप एक ही नाम से वैरिएबल को कई बार डिक्लेयर कर सकते हैं।

```js
## Syntax
var x = 10;
```

`Example:`

```js
function example() {
  var x = 5;
  if (true) {
    var x = 10; // Same variable as above
  }
  console.log(x); // Output: 10 (no block-level scope) }
}
```

2. **let**

   - `Scope`: let is block-scoped, meaning it is only accessible within the block (i.e., {}) where it's declared.
   - `Hoisting`: let is hoisted but not initialized. Accessing it before declaration results in a ReferenceError.
   - `Re-declaration`: let cannot be redeclared within the same scope, but can be reassigned.
     <br/>
     `Scope`: let का स्कोप ब्लॉक-स्कोप्ड होता है, यानी यह केवल उसी ब्लॉक के भीतर उपलब्ध होता है जहाँ इसे घोषित किया गया है।
     `Hoisting`: let डिक्लेरेशन होस्ट होती है लेकिन इनिशलाइज़ नहीं होती। इसे डिक्लेयर करने से पहले एक्सेस करने पर ReferenceError आएगा।
     `Re-declaration`: let को एक ही स्कोप में दोबारा डिक्लेयर नहीं किया जा सकता, लेकिन इसे रीअसाइन किया जा सकता है।

```js
## Syntax
let x = 10;
```

`Example:`

```js
function example() {
  let x = 5;
  if (true) {
    let x = 10; // अलग वैरिएबल (ब्लॉक स्कोप्ड)
  }
  console.log(x); // आउटपुट: 5 (ब्लॉक-लेवल स्कोप का सम्मान किया जाता है)
}
```

2. **const**

- `Scope`: Like let, const is also block-scoped.
- `Hoisting`: const is hoisted but not initialized. Accessing it before declaration results in a ReferenceError.
- `Re-declaration`: You cannot reassign or redeclare a const variable once it’s initialized. However, if the const holds an object or array, its properties or elements can still be modified.
  <br/>
  `Scope`: const भी let की तरह ब्लॉक-स्कोप्ड होता है।
  `Hoisting`: const डिक्लेरेशन होस्ट होती है लेकिन इनिशलाइज़ नहीं होती। इसे डिक्लेयर करने से पहले एक्सेस करने पर ReferenceError मिलेगा।
  `Re-declaration`: एक बार const वैरिएबल को इनिशलाइज़ करने के बाद इसे दोबारा असाइन या डिक्लेयर नहीं किया जा सकता। लेकिन अगर यह किसी ऑब्जेक्ट या array को होल्ड कर रहा है, तो उसकी प्रॉपर्टीज या एलिमेंट्स बदले जा सकते हैं।

```js
## Syntax
const x = 10;
```

`Example:`

```js
const arr = [1, 2, 3];
arr.push(4); // सही (array को मोडिफाई कर रहे हैं, असाइन नहीं कर रहे)
arr = [5, 6]; // Error: असाइनमेंट किया गया const वैरिएबल
```

**[⬆ Back to Top](#table-of-contents)**

## 14. Temporal Dead Zone (TDZ)

- `Concept`: The Temporal Dead Zone refers to the time between when a variable is hoisted and when it is initialized. In the case of let and const, accessing the variable before its declaration in this zone causes a ReferenceError.
- `Scope`: This applies to variables declared with let and const because they are block-scoped and hoisted but not initialized until their declaration is encountered in the code.
  <br/>
  `Concept`: Temporal Dead Zone (TDZ) वह समय होता है जब एक वैरिएबल होस्ट किया जाता है लेकिन इसे इनिशलाइज़ नहीं किया गया होता। let और const के मामले में, डिक्लेरेशन से पहले वैरिएबल को एक्सेस करने पर ReferenceError आता है।
  `Scope`: यह नियम let और const पर लागू होता है क्योंकि यह दोनों ब्लॉक-स्कोप्ड होते हैं और इन्हें होस्ट तो किया जाता है लेकिन तब तक इनिशलाइज़ नहीं किया जाता जब तक कि कोड में इनकी डिक्लेरेशन लाइन तक नहीं पहुंचती।

  <br/>
  `Example:`

```js
console.log(x); // ReferenceError: Cannot access 'x' before initialization
let x = 5;
```

_उपरोक्त उदाहरण में, let वैरिएबल x होस्ट तो किया गया है, लेकिन उसे डिक्लेयर करने से पहले एक्सेस करने पर एक त्रुटि (ReferenceError) आती है।_

`Summary:`

- TDZ let और const वैरिएबल्स के लिए होता है, जिसमें होस्टिंग और इनिशलाइज़ेशन के बीच उन्हें एक्सेस नहीं किया जा सकता।

**[⬆ Back to Top](#table-of-contents)**

## 15. Callback Function

- `Definition`: A callback function is a function that is passed as an argument to another function and is executed after some operation has been completed by the receiving function. It allows asynchronous or delayed execution of code, which is a key concept in JavaScript, especially when dealing with operations like fetching data, file handling, or timers.
- `Usage`: In JavaScript, callback functions are commonly used in event handling, asynchronous tasks (like HTTP requests), and array methods (like .map(), .filter()).

`परिभाषा`: एक callback function वह फंक्शन होता है जिसे किसी अन्य फंक्शन के तर्क (argument) के रूप में पास किया जाता है और तब चलाया जाता है जब वह मुख्य फंक्शन अपना कार्य पूरा कर लेता है। यह खासकर तब उपयोगी होता है जब हम ऐसे कार्य कर रहे होते हैं जिनमें समय लगता है, जैसे कि डेटा फ़ेच करना, फाइल हैंडलिंग, या टाइमर।
`उपयोग`: JavaScript में callback functions का उपयोग इवेंट हैंडलिंग, asynchronous tasks (जैसे HTTP requests), और array methods (जैसे .map(), .filter()) में किया जाता है।

```js
function mainFunction(callback) {
  // कुछ कोड
  callback(); // Callback function को चलाना
}

function myCallback() {
  console.log("Callback function चल गया!");
}

mainFunction(myCallback);
```

`उदाहरण` (Asynchronous Callback का प्रयोग setTimeout के साथ):

```js
function greet(name) {
  console.log("नमस्ते, " + name);
}

setTimeout(function () {
  greet("Alice");
}, 2000); // 2 सेकंड के बाद चलेगा
```

**[⬆ Back to Top](#table-of-contents)**

## 16. Promises

- `Definition`: A Promise is an object in JavaScript that represents the eventual completion (or failure) of an asynchronous operation and its resulting value. It allows you to write asynchronous code in a more readable and manageable way compared to traditional callback-based approaches.
- `States of a Promise`:

  - `Pending`: The initial state, meaning the operation has not completed yet.
  - `Fulfilled`: The operation was successful, and the promise has a resolved value.
  - `Rejected`: The operation failed, and the promise is rejected with a reason (error).

<br/>

- `परिभाषा`: JavaScript में Promise एक ऐसा ऑब्जेक्ट है जो किसी asynchronous ऑपरेशन के सफल या असफल होने को दर्शाता है और उसके अंतिम परिणाम (value) को प्राप्त करता है। यह callback-based तरीके की तुलना में asynchronous कोड को पढ़ने और प्रबंधित करने का एक बेहतर तरीका प्रदान करता है।
- `Promise की अवस्थाएँ`:
  - `Pending`: प्रारंभिक अवस्था, जहाँ ऑपरेशन अभी पूरा नहीं हुआ है।
  - `Fulfilled`: ऑपरेशन सफल हुआ और प्रॉमिस ने एक वैल्यू प्रदान की।
  - `Rejected`: ऑपरेशन असफल रहा और प्रॉमिस ने एक कारण (त्रुटि) के साथ इसे अस्वीकार कर दिया।

```js
## Syntax

let promise = new Promise(function(resolve, reject) {
  // async operation
  if (/* success */) {
    resolve("Operation Successful");
  } else {
    reject("Operation Failed");
  }
});
```

```js
## Example

let fetchData = new Promise(function(resolve, reject) {
  let dataAvailable = true;

  if (dataAvailable) {
    resolve("डेटा सफलतापूर्वक प्राप्त हुआ!");
  } else {
    reject("डेटा प्राप्त करने में त्रुटि.");
  }
});

fetchData
  .then(function(result) {
    console.log(result); // Output: Data fetched successfully!
  })
  .catch(function(error) {
    console.log(error); // If rejected: Error fetching data.
  });
```

- `How Promises Work:`

  - `then()`: This method is used to handle the result when the promise is resolved (fulfilled).
  - `catch()`: This method is used to handle any errors if the promise is rejected.
  - `finally()` (optional): This method runs after the promise is settled, either resolved or rejected, allowing cleanup operations.
  - `Chaining Promises`:
    You can chain multiple .then() calls to handle asynchronous operations in a sequence.

<br/>

- `कैसे काम करता है Promise`:
  - `then()`: यह method तब चलती है जब प्रॉमिस सफलतापूर्वक (fulfilled) हो जाता है।
  - `catch()`: यह method तब चलती है जब प्रॉमिस असफल (rejected) हो जाता है।
  - `finally()` (optional): यह method प्रॉमिस के settle होने के बाद चलती है, चाहे वह resolve हो या reject, जिससे cleanup कार्यों को प्रबंधित किया जा सके।
  - `Promise Chaining`:
    आप कई .then() calls को एक साथ जोड़ सकते हैं ताकि asynchronous कार्यों को क्रम से संभाला जा सके।

```js
## Example

let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve(10), 1000);  // 1 सेकंड बाद resolve होगा
});

promise
  .then(function(result) {
    console.log(result); // आउटपुट: 10
    return result * 2;   // अगले `then` में 20 पास करेगा
  })
  .then(function(result) {
    console.log(result); // आउटपुट: 20
  });
```

- `Key Points:`
  - Promises asynchronous ऑपरेशंस को अधिक प्रभावी ढंग से प्रबंधित करने में सहायक होते हैं।
    यह "callback hell" से बचने में मदद करते हैं और सफलता और असफलता के मामलों को संभालने के लिए एक साफ और chainable सिंटैक्स प्रदान करते हैं।

**[⬆ Back to Top](#table-of-contents)**
