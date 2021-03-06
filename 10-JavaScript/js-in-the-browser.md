# JavaScript in the Browser

## Learning Goals
By the end of this lesson, students will be able to...

- Practice evaluating JavaScript in the Chrome Dev Tools -> Console
- Connect a JavaScript script to a website
- Define the DOM
- Do basic manipulation of the DOM

## Running JavaScript in the Developer Console

Our first step is to prove that our browser Google Chrome can read and execute JavaScript.

In your browser, open a new empty tab, and pull up your developer tools. Click on the Console tab. This is where calls to `console.log()` will end up. It's also where you'll see any errors that occur.

Notice also that there is something that looks like a command prompt. Let's see what it does.

```javascript
console.log('live, from the browser!');
```

Nice, there it is. OK, let's do something a little more interesting - a popup box.

```javascript
alert('four score and seven years ago...');
```

Pops right up! Can't do that in the terminal!

[Where did the `alert` method come from? Who defined it?](https://developer.mozilla.org/en-US/docs/Web/API/Window/alert) `Window` is something that the browser makes available to us. When we write JavaScript that runs in the browser, we can predict that the `alert` method will be available to us.

What happens if we make a mistake? Let's reference an undefined variable.

```javascript
console.log(doesNotExist);
```

OK, so that's what an error looks like. If you double click on the message or click on the `(...)`, it'll show you more details.

The Chrome console is kind of like the rails console. It gives you access to all the variables you've defined in your scripts, allowing you to write JavaScript live and see the effects immediately. Use it well.

## Running JavaScript on Websites

Obviously we don't expect users to type out all their own JavaScript by hand. How do we get a website to connect to a JavaScript file? We'll add a link to our JavaScript file in our HTML, similar to the way we included CSS before.

When our user goes to a website using their browser, the browser will make a request for an HTML page. This HTML page contains references to our CSS and JavaScript files. Then, these files will be loaded into the browser.
<!-- Diagram located here: https://drive.google.com/a/adadevelopersacademy.org/file/d/0B6Pq6XZ1hzv1WHcyUnZZREtadDg/view?usp=sharing -->
![JS and CSS in the browser](images/js-css-browser.png)


### Try It

1. Create a new directory called `browser-js`, with two files: `index.html` and `index.js`:

    ```bash
    $ mkdir browser-js
    $ cd browser-js
    $ touch index.html
    $ touch index.js
    ```
1. Add this basic HTML to `index.html`:

    ```html
    <!-- index.html -->
    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <title>JavaScript Test Page</title>
      </head>
      <body>
        <h1>Test page for JavaScript in the Browser</h1>

        <div id="js-lecture-target">This is text that lives inside a div element with an id "js-lecture-target"!</div>
      </body>
    </html>
    ```

1. And finally, add this JavaScript to `index.js`:

    ```javascript
    // index.js
    console.log('This is a test');
    ```

1. Modify your HTML page `index.html` by adding this `<script>` tag at the end of `<body>`, RIGHT before the closing `</body>` tag:

    ```html
    <!-- index.html -->
    ...
    <body>
      ...

      <script src="index.js" type="text/javascript"></script>
    </body>
    ```

1. Ensure all files are saved
1. Open up the Chrome Dev Tools and switch to the Console tab
1. Reload the page (refresh the page)
1. Observe that the test text `This is a test` from the JavaScript prints out!

#### Extend This Example

1. Check that you and your neighbor are on the right track
1. Modify your `index.js` file so it prints to the console some other text
1. Reload your HTML page and make sure you see that new text!
1. Modify your `index.js` file so it prints out this text five times

### Side Note: About `<script>`

Just like CSS links, you can include multiple `<script>`s in your page, and they'll be loaded in order.

#### Where Should `<script>` Go?

Believe it or not, people have a lot of opinions on this topic.

<details>

  <summary>
    In this class we'll always load our scripts at the end of the body. If you'd like to learn more, click here to expand and see our thoughts.
  </summary>

The reason why JS loading is so contentious is, when the browser encounters a `<script>` tag, it stops loading the HTML document. It goes and downloads your _entire_ script, which might be quite large and hosted halfway across the internet. Only once the script has finished downloading does it continue rendering the page. That means if you put your `<script>` tags before your content, the user gets to look at an empty white screen for a while while your scripts load. Not a great user experience.

The easiest way to deal with this is to always place your scripts at the bottom of your `<body>` section. That way, the browser renders the whole page first, then goes and gets your scripts.

Out in the wild you'll see other techniques, like downloading the scripts asynchronously and not running them until the page has finished loading. This is cool, and you should definitely know that it's a thing, but it takes some work to set up.

</details>

## Manipulating a Website is Through the DOM

Logging to the console is alright, but the true power of JavaScript is that it can dynamically change the contents of the webpage.

Browsers have already defined a way (an interface) to represent, access, and modify the contents of a website. This interface is called **the DOM**. When we use this interface, we can dynamically change the page using JS.

Until now our web applications have rendered _static_ HTML which defines a webpage. They have then sent it to the browser which in turn "builds the DOM" from that HTML.

When we use JavaScript to manipulate the DOM, we do not change the original HTML that was sent to the browser, instead we change the browser's internal representation of that same webpage. An important consequence of this distinction is that all of our changes disappear as soon as the browser forgets about that webpage (e.g. if we close the tab or the whole browser).

### What is the DOM?

**The DOM** is the interface the browser provides for dynamically changing the page's content, behavior, and appearance. We can think of it as a representation of the site's content that the browser gives us.

_DOM_ stands for Document Object Model. The document in question is our web page! The "Object Model" part is for distinguishing the DOM approach from other ways of accessing a document (which are not available in the browser). You can see the DOM for any webpage your browser has loaded by opening the developer tools and selecting the Elements tab (in Chrome).

The DOM is effectively a tree, like the ones we've learned about in CS fundamentals. In general each node in the tree matches up to an individual HTML tag such as `div` or `img`.

![dom as represented as a tree](images/dom.gif)

Nodes in the DOM have properties that match the attributes set on them in the HTML (e.g. an `img` node might have `src` and `id` properties). DOM nodes also have children, which are all of the other nodes nested within them (e.g. a `div` node might have a `table` child node).

## Using JavaScript and the DOM

In JavaScript, the DOM is exposed through a set of objects and methods that provide access to the HTML structure of the webpage.

We will find and practice methods for DOM manipulation that will allow us to do things like:
- add a node (element) to the DOM
- remove a node (element) from the DOM
- change attributes of a node
- enable interactions from a user, such as clicking a node (element)

### Exercise

The DOM is accessed through an object called `document`. Therefore, we will start DOM manipulation through this `document` object, since it is the broadest object that holds the DOM.

1. In the Dev Tools Console, type into the console `document`. What do you get? When you expand it, what details do you see? What does it represent?
1. Draw a representation of the DOM as a tree with a partner

#### Finding an Existing Node and Modifying It

We can find an existing node on the DOM in a sleuth of ways. We will explore one way:

1. In the Dev Tools Console, enter into the console `document.getElementById('');` What do you get?
1. In the Dev Tools Console, enter into the console `document.getElementById('js-lecture-target');` What do you get? What details do you see? What does it represent?
1. Where is an element with the id `js-lecture-target` defined?

Now let's try looking at these details and manipulating them.

1. In the Dev Tools Console, enter in the following line:
    ```javascript
    document.getElementById('js-lecture-target').innerHTML;
    ```

    What do you get back? What does it represent? Does it have the tags included?
1. In the Dev Tools Console, enter in the following line:
    ```javascript
    document.getElementById('js-lecture-target').innerHTML = 'I can change website text with JavaScript!';
    ```

    What do you get back? What do you see on the website (HTML)?
1. Refresh the page. Do you continue to see the changes that you made?

Now that we've tried it in the console, let's update our `index.js` to use this new knowledge.

Replace the contents of your `index.js` file with the following:

```javascript
// index.js
console.log('This is a test');

const target = document.getElementById('js-lecture-target');  // Find the HTML element where the ID is js-lecture-target
target.innerHTML = '<p>I give you... content!</p>'; // Put this HTML inside the div we retrieved above
```

Now that all of the files are saved, reload your HTML page. When your HTML page loads, as the browser reads through the HTML, it comes across the `<script>` tag, which loads and runs the appropriate JavaScript code in `index.js`. Therefore, when everything is all finished loading and executing, we already see that the JS has run!

## Conclusion

We have modified the DOM using JavaScript code! We should be able to extend this knowledge and imagine the possibilities of what we can do with DOM manipulation:

- If we select the correct elements, we can change how something looks dynamically
- If we select the correct elements, we can define new behaviors for our website

## Out-of-the-Box DOM Manipulation is Clunky

It's worth noting that the raw interface for DOM manipulation in JavaScript isn't great. It's clunky, and lots of pieces are slightly different in different browsers. There are plenty of popular JavaScript libraries out there that can make this code less clunky.

## Additional Resources
* [MDN on the DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
* [StackOverflow on where to put your `<script>` tags](http://stackoverflow.com/questions/436411/where-is-the-best-place-to-put-script-tags-in-html-markup)
*  [Slides on JS In the browser](https://docs.google.com/presentation/d/1GPTn6W0QeEyquCxBJFj-E9W-i-MgXsBytA4xtCCW6Q4/edit#slide=id.g195ed98213_0_86)
