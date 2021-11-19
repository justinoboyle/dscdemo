## Step 1 -- Provision a Firebase product

Firebase is an interface on top of Google Cloud we can use to demo creating a basic app.

1. Visit https://firebase.google.com and sign into your account.

2. Click "Go to Console"

3. Click "Create a project"

4. Enter a name -- we're going to use "DSC Demo". Accept the terms and continue.

5. Step through the onboarding -- you can skip Analytics.

## Step 2 -- Set up hosting.

1. Select "Hosting" from the dropdown on the left under project overview.

2. Click "Get started"

3. Make sure you have npm and NodeJS installed (on macOS homebrew it's `brew install nodejs` -- check out npmjs.org for other platforms)

4. Follow the instructions -- run `npm install -g firebase-tools` (this first one you may need to prefix with `sudo`) and `firebase login`, you may need to click a link and log in.

5. Create and `cd` to a folder you're going to use (I did `mkdir -p ~/Desktop/dscdemo && cd ~/Desktop/dscdemo`)

6. Run `firebase init`

7. Don't just press enter! Press the down arrow until you're over "Hosting: Configure files for Firebase Hosting", press "space" to toggle it, and then press enter to submit

8. Go down to "Use an existing project" and press enter

9. Select your DSC Demo project, probably the top one and press enter

10. For the rest of the hosting setup questions, use the defaults and just press enter until you see "Firebase initialization complete!"

11. Type "firebase deploy" into the command line.

12. You should see a "Hosting URL" show up -- congrats, that's your deployed website! That's a public link anyone can access.

## Step 3 -- make a To-do list app!

1. Open the directory in your favorite editor, I like Visual Studio Code

2. Open up `public/index.html` and delete everything in the file.

3. Here's a very basic template we can use to render our page:

```html
<html>
  <body>
    <div class="container">
      <h1>To-Do List App</h1>
      <div class="items"></div>
    </div>
    <script>
    
     </script>
  </body>
</html>
```

4. Underneath our items div, let's add some UI to add an item to the todo list:

```html
<input
    type="text"
    id="text"
    class="item-input"
    placeholder="Add an item..."
/>
<button id="add" class="add-item">Add Item</button>
```

5. Next, let's do some logic inside of our script to store the state and render it:

```js
let state = [];

function renderItems() {
    let itemsHTML = "";
    state.forEach(function (item, i) {
        itemsHTML += `
        <div class="item">
        <button onClick="deleteItem(${i})"class="item-delete">&times;</button>
        <span class="item-text">${item}</span>
        </div>`;
    });

    document.querySelector(".items").innerHTML = itemsHTML;
}
```

6. Ok great, our page is rendering our items (remember we have to call renderItems each time we change it). Let's add support to delete our item:

```js
function deleteItem(index) {
    state.splice(index, 1); // take it out of the state
    renderItems(); // re-render the page
}
```

7. Add a handler to handle creation. OK - last part, let's add a handler for when we click the button

```js
document
    .querySelector(".add-item")
    .addEventListener("click", function () {
        let item = document.querySelector(".item-input");
        state.push(item.value); // Add to the array
        item.value = ""; // Reset the input
        renderItems(); // Re-render the page
    });
```

8. And... some styling for fun, just below `<html>` (have fun with this, make it your own):

```html
  <head>
    <style>
      body {
        font-family: sans-serif;
        font-size: 14px;
        background-color: black;
        color: white;
      }
      button {
        background-color: #212529;
        border: none;
        color: white;
        margin: 5px;
      }
      span,
      div {
        color: white;
      }
      .container {
        width: 100%;
        max-width: 800px;
        margin: 0 auto;
        margin-top: 40px;
      }
    </style>
  </head>
```

## Step 4 - Deploy again

1. Run `firebase deploy`

2. The Hosting URL is now a public link to your app hosted in the cloud! Huzzhah!

[Demo link](https://dsc-demo-b2680.web.app)