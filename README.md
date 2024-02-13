# Javascript Questions for Interviews

## 1. `Event delegation / इवेंट डेलिगेशन क्या है?`

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

## 2. `Null and Undefined`

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

## 3. `What is Garbage Collection? (गार्बेज कलेक्शन क्या है?)`

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

## 4. `What is Hoisting?`

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

## 5. `Event Propagation`

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

## 6. `The Importance of true in addEventListener`

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

## 7. `What is Scoping? (स्कोपिंग क्या है?)`

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

## 8. `What are Closures? (क्लोजर्स क्या हैं?)`

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
