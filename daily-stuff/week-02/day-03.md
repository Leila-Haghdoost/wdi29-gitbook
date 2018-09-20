# Day 03

What we covered today:

* JavaScript - The Document Object Model \(DOM\)
  * Accessing Elements \(Selection\)
  * Manipulating Elements
  * Creating Elements
  * Events
* JavaScript - Animation

  * Timers
  * Callbacks

#### **Slides**

* [​The DOM, events, selectors, timers and callbacks​](https://github.com/textchimp/wdi-29/blob/master/week2/javascript-and-html.pdf)

### Power User Zone {#power-user-zone}

* ​[Notes/shortcut list](https://gist.github.com/textchimp/faa5366b949067699f17734f42e5b5ea)​



#### JavaScript - The Document Object Model \(DOM\) {#javascript---the-document-object-model-dom}

**What is the DOM?**

The **Document Object Model**, or 'DOM':

1. Is a structural representation of HTML documents - the document is represented as a tree with nodes; and
2. Defines methods for accessing, manipulating and creating those nodes.

JavaScript can't access or manipulate what's in the browser directly - that's just a bunch of pixels. JavaScript can, however, interact with the DOM.

The DOM is HTML after it has been parsed by the browser. It can be different to the HTML you wrote, if the browser fixes issues in your code, if JavaScript changes it etc.

**How do we use it?**

The document object gives us ways of accessing and changing the DOM of the current webpage.

General strategy:

* Find the DOM node using an access method
* Store that DOM node in a variable
* Manipulate the DOM node by changing its attributes, styles, inner HTML, or appending new nodes to it.

**Accessing elements \(Selection\)**

**Get element by ID**

```javascript
// The method signature:
// document.getElementById(id);

// If the HTML had:
// <img id="mainpicture" src="http://placekitten.com/200/300">
// We'd access it this way:
let img = document.getElementById('mainpicture');

// DON'T USE THE HASH!
```

**Get elements by tag name**

```javascript
// The method signature:
// document.getElementsByTagName(tagName);

// If the HTML had:
<li class="catname">Lizzie</li>
<li class="catname">Daemon</li>

// We'd access it this way:
let listItems = document.getElementsByTagName('li');
for (let i = 0; i < listItems.length; i++) {
  let listItem = listItems[i];
}
```

**Query selector and query selector all**

```javascript
// The HTML5 spec includes a few even more convenient methods.
// Available in IE9+, FF3.6+, Chrome 17+, Safari 5+:

document.getElementsByClassName(className);
let catNames = document.getElementsByClassName('catname');
for (let i = 0; i < catNames.length; i++) {
  let catName = catNames[i];
}

// Available in IE8+, FF3.6+, Chrome 17+, Safari 5+:
document.querySelector(cssQuery);
document.querySelectorAll(cssQuery);
let catNames = document.querySelectorAll('ul li.catname');
```

Remember, some of these methods return arrays and some return single things!

**Methods that return single elements**

```javascript
getElementById();
querySelector(); // returns only the first of the matching elements
  let firstCatName = document.querySelector('ul li.catname');
```

**Methods that will return an array of elements**

Others return a collection of elements in an array: \(NOTE: not exactly\)

```javascript
getElementByClassName();
getElementByTagName();
querySelectorAll();

let catNames = document.querySelectorAll('ul li.catname');
let firstCatName = catNames[0];
```

**Manipulating elements**

**Changing an element's attributes**

You can access and change attributes of DOM nodes using JavaScript object dot notation.

If we had this HTML:

![](http://placekitten.com/200/300)

We can change the src attribute this way:

```javascript
let oldSrc = img.src;
img.src = 'http://placekitten.com/100/500';

// To set class, use the property className:
img.className = "picture";
```

**Changing an element's style**

You can change styles on DOM nodes via the style property.

If we had this CSS:

```css
body {
  color: red;
}
```

We'd run this JS:

```javascript
let pageNode = document.getElementsByTagName('body')[0];
pageNode.style.color = 'red';
```

**IMPORTANT:** CSS property names with a "-" must be camelCased and number properties must have a unit:

For example, to replicate this CSS:

```css
body {
  background-color: pink;
  padding-top: 10px;
  font-style: oblique;
}
```

We would need to do this:

```javascript
let pageNode = document.querySelector('body');
pageNode.style.backgroundColor = 'pink';
pageNode.style.paddingTop = '10px';
padgeNode.style.fontStyle = 'oblique';
```

**Changing an element's HTML**

Each DOM node has an innerHTML property with the HTML of all its children:

`let pageNode = document.getElementsByTagName('body')[0];`

You can read out the HTML like this:

`console.log(pageNode.innerHTML);`

```javascript
// You can set innerHTML yourself to change the contents of the node:
pageNode.innerHTML = "<h1>Oh Noes!</h1> <p>I just changed the whole page!</p>"

// You can also just add to the innerHTML instead of replace:
pageNode.innerHTML += "...just adding this bit at the end of the page.";
```

**Creating elements**

The document object also provides ways to create nodes from scratch:

`document.createElement(tagName);`

`document.createTextNode(text);`

`document.appendChild();`

```javascript
let pageNode = document.getElementsByTagName('body')[0];

let newImg = document.createElement('img');
newImg.src = 'http://placekitten.com/400/300';
newImg.style.border = '1px solid black';
pageNode.appendChild(newImg);

let newParagraph = document.createElement('p');
let paragraphText = document.createTextNode('Squee!');
newParagraph.appendChild(paragraphText);
pageNode.appendChild(newParagraph);
```

#### _Document Object Model - Manipulating elements - Exercises_ {#document-object-model-manipulating-elements-exercises}

* ​[Exercises: DOM Access](https://gist.github.com/textchimp/f395efd8f5a8c3c2750b4af455078ac1)​
* ​[Exercises: Logo Hijack](https://gist.github.com/textchimp/2609786b842837f5d6f276e0dee6665d)​
* ​[Exercises: AboutMe & Booklist](https://gist.github.com/wofockham/894b9a5e05a971e0208b)

### Events {#events}

#### _Adding event listeners_ {#adding-event-listeners}

In IE 9+ \(and all other browsers\):

`domNode.addEventListener(eventType, eventListener, useCapture);`

```javascript
// HTML = <button id="counter">0</button>
​const counterButton = document.getElementById('counter');
const button = document.querySelector('button');
button.addEventListener('click', makeMadLib);

​const onButtonClick = function() {    
  counterButton.innerHTML = parseInt(counterButton.innerHTML) + 1;
};

counterButton.addEventListener('click', onButtonClick, false);
```

#### _Some event types_ {#some-event-types}

The browser triggers many events. A short list:

* **mouse events** \(MouseEvent\): mousedown, mouseup , click, dblclick, mousemove, mouseover, mousewheel , mouseout, contextmenu
* **touch events** \(TouchEvent\): touchstart, touchmove, touchend, touchcancel
* **keyboard events** \(KeyboardEvent\): keydown, keypress, keyup
* **form events**: focus, blur, change, submit
* **window events**: scroll, resize, hashchange, load, unload

#### _Getting details from a form_ {#getting-details-from-a-form}

```javascript
// HTML// <input id="myname" type="text">
// <button id="button">Say My Name</button>

​const button = document.getElementById('button');
const onClick = function(event) {    
  let myName = document.getElementById("myname").value;    
  alert("Hi, " + myName);
};

button.addEventListener('click', onClick);
```

* ​[JSBin - Event Listener - button click](http://jsbin.com/goguxomiwo/edit?js,output)​

## JavaScript - The Window Object {#javascript-the-window-object}

When you run JS in the browser, it gives you the window object with many useful properties and methods:

```javascript
window.location.href; // Will return the URL
window.navigator.userAgent; // Tell you about the browser
window.scrollTo(10, 50);
```

Lots of things that we have been using are actually a part of the window object. As the window object is the assumed global object on a page.

```javascript
window.alert("Hello world!"); // Same things
alert("Hello World!");​

window.console.log("Hi"); // Same things
console.log("Hi");
```

## Animation in JavaScript {#animation-in-javascript}

### Starting an animation {#starting-an-animation}

The standard way to animate in JS is to use these 2 window methods.

To call a function once after a delay:

`window.setTimeout(callbackFunction, delayMilliseconds);`

To call a function repeatedly, with specified interval between each call:

`window.setInterval(callbackFunction, delayMilliseconds);`

Commonly used to animate DOM node attributes:

```javascript
const makeImageBigger = function() {  
  const img = document.getElementsByTagName('img')[0];  
  img.setAttribute('width', img.width+10);
};
window.setInterval(makeImageBigger, 1000);
```

### Animating styles {#animating-styles}

It's also common to animate CSS styles for size, transparency, position, and color:

```javascript
let img = document.getElementsByTagName('img')[0];
img.style.opacity = 1.0;
window.setInterval(fadeAway, 1000);
const fadeAway = function() {  
  img.style.opacity = img.style.opacity - .1;
};​

// Note: opacity is 1E9+ only (plus all other browsers).
let img = document.getElementsByTagName('img')[0];
img.style.position = 'absolute';
img.style.top = '0px';
const watchKittyFall = function() {  
  let oldTop = parseInt(img.style.top);  
  let newTop = oldTop + 10;  
  img.style.top = newTop + 'px';
};

​window.setInterval(watchKittyFall, 1000);
// Note: you must specify (and strip) units.
```

### Stopping an animation {#stopping-an-animation}

To stop at an animation at a certain point, store the timer into a variable and clear with one of these methods:

```javascript
window.clearTimeout(timer);
window.clearInterval(timer);
```

```javascript
let img = document.getElementsByTagName('img')[0];
img.style.opacity = 1.0;
  
​const fadeAway = function() {  
  img.style.opacity = img.style.opacity - .1;  
  if (img.style.opacity < .5) {    
    window.clearInterval(fadeTimer);  
    }
};
let fadeTimer = window.setInterval(fadeAway, 100);
```

## **Homework**

* [The Cat Walk](https://gist.github.com/wofockham/b4a62f016bfd241627dd)
* [http://dn.ht/picklecat/](http://dn.ht/picklecat/)
* [http://cat-bounce.com/](http://cat-bounce.com/)


