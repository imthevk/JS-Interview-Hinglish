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

```js
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

###इवेंट डेलिगेशन का तरीका:

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

## 2. Null and Undefined / इवेंट डेलिगेशन क्या है?

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
