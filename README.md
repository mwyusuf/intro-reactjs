# Intro-Reactjs
Complete Intro to react

## No Frills React
Let's start by writing pure React. No compile step. No JSX. No Babel. No Webpack or Parcel. Just some JavaScript on a page.

Let's start your project. Create your project directory. Create an index.html and put it into a src/ directory inside of your project folder. In index.html put:

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="stylesheet" href="./style.css">
  <title>Adopt Me</title>
</head>

<body>
  <div id="root">not rendered</div>
  <script src="https://unpkg.com/react@17.0.1/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@17.0.1/umd/react-dom.development.js"></script>
  <script src="./App.js" />
</body>

</html>
```
This is about the simplest React app you can build.

- The first thing we do is make our own component, App. React is all about making components. And then taking those components and making more components out of those.
- There are two types of components, function components and class components. This is a function component. We'll see class components shortly.
- A function component must return markup (which is what React.createElement generates.)
- These component render functions have to be fast. This function is going to be called a lot. It's a hot code path.
- Inside of the render function, you cannot modify any sort of state. Put in functional terms, this function must be pure. You don't know how or when the function will be called so it can't modify any ambient state.
- React.createElement creates one instance of some component. If you pass it a string, it will create a DOM tag with that as the string. We used h1 and div, those tags are output to the DOM. If we put x-some-custom-element, it'll output that (so web components are possible too.)
- The second empty object (you can put null too) is attributes we're passing to the tag or component. Whatever we put in this will be output to the element (like id or style.)
- ReactDOM.render is what takes our rendered App component and puts in the DOM (in our case we're putting it in the root element.)
- Notice we're using React.createElement with App as a parameter to ReactDOM.render. We need an instance of App to render out. App is a class of components and we need to render one instance of a class. That's what React.createElement does: it makes an instance of a class.

Now that we've done that, let's separate this out from a script tag on the DOM to its own script file (best practice.) Make a new file in your src directory called App.js and cut and paste your code into it.

Modify your code so it looks like:

```js
const Pet = (props) => {
  return React.createElement("div", {}, [
    React.createElement("h1", {}, props.name),
    React.createElement("h2", {}, props.animal),
    React.createElement("h2", {}, props.breed),
  ]);
};

const App = () => {
  return React.createElement("div", {}, [
    React.createElement("h1", {}, "Adopt Me!"),
    React.createElement(Pet, {
      name: "Luna",
      animal: "Dog",
      breed: "Havanese",
    }),
    React.createElement(Pet, {
      name: "Pepper",
      animal: "Bird",
      breed: "Cockatiel",
    }),
    React.createElement(Pet, { name: "Doink", animal: "Cat", breed: "Mix" }),
  ]);
};

ReactDOM.render(React.createElement(App), document.getElementById("root"));
```

## Java Script Tool

### NPM
npm does not stand for Node Package Manager. It is, however, the package manager for Node. (They don't say what it stands for.) It also has all the packages in the front end scene. npm makes a command line tool, called npm as well. npm allows you to bring in code from the npm registry which is a bunch of open source modules that people have written so you can use them in your project. Whenever you run npm install react (don't do this yet), it will install the latest version of React from the registry.

In order to start an npm project, run npm init at the root of your project. If you don't have Node.js installed, please go install that too. When you run npm init it'll ask you a bunch of questions. If you don't know the answer or don't care, just hit enter. You can always modify package.json later. This will allow us to get started installing and saving packages.

### Prettier
It's important to keep quality high when writing code. Or at least that's how I sell ESLint and Prettier to my co-workers. In reality I'm super lazy and want the machine to do as much work as possible so I can focus more on architecture and problem-solving and less on syntax and style. While there are many tools that can help you keep code quality high, these two I consider core to my workflow.

Prettier is an amazing tool from the brain of James Long. James, like many of us, was sick of having to constantly worry about the style of his code: where to stick indents, how many, when to break lines, etc etc. Coming from languages like Go, Reason, or Elm where all that is just taken care of by the tooling for the language, this quickly wears. James did something about it and made a tool to take care of it: Prettier.

Prettier is a really fancy pretty printer. It takes the code you write, breaks it down in to an abstract syntax tree (AST) which is just a representation of your code. It then takes that AST, throws away all of your code style you made and prints it back out using a predefined style. While this sounds a little scary, it's actually really cool. Since you no longer have control of the style of your code, you no longer have to think about it at all. Your code is always consistent, as is the code from the rest of your team. No more bikeshedding!! As I like to put it: if your brain is a processor, you get to free up the thread of your brain that worries about code styles and readability: it just happens for you. Don't like semicolons? Don't write them! It puts them in for you. I love Prettier.

Need to tool around a bit with it before you trust it? Go here. You can see how it works.

Let's go integrate this into our project. It's pretty easy (lol.)

Either install Prettier globally npm install --global prettier or replace when I run prettier with (from the root of your project) npx prettier. From there, run prettier script.js. This will output the formatted version of your file. If you want to actually write the file, run prettier --write script.js. Go check script.js and see it has been reformatted a bit. I will say for non-JSX React, prettier makes your code less readable. Luckily Prettier supports JSX! We'll get to that shortly.

Prettier has a few configurations but it's mostly meant to be a tool everyone uses and doesn't argue/bikeshed about the various code style rules. Here they are. I just use it as is since I'm lazy. Prettier can also understand flow and TypeScript.

Prettier is great to use with Visual Studio Code. Just download this extension. Pro tip: set it to only run Prettier when it detects a Prettier config file. Makes it so you never have to turn it off. In order to do that, set prettier.requireConfig to true and editor.formatOnSave to true.

So that our tool can know this is a Prettier project, we're going to create a file called .prettierrc and put {} in it. This lets everyone know this is a Prettier project that uses the default configuration. You can put other configs here if you hold strong formatting opinions.

### ESLint
On top of Prettier which takes of all the formatting, you may want to enforce some code styles which pertain more to usage: for example you may want to force people to never use with which is valid JS but ill advised to use. ESLint comes into play here. It will lint for this problems.

First of all, run npm install -D eslint eslint-config-prettier to install eslint in your project development dependencies. Then you may configure its functionalities.

There are dozens of preset configs for ESLint and you're welcome to use any one of them. The Airbnb config is very popular, as is the standard config (both of which I taught in previous versions of this class). I'm going to use a looser one for this class: eslint:recommended. Let's create an .eslintrc.json file to start linting our project.

Create this file called .eslintrc.json.

```json
{
  "extends": ["eslint:recommended", "prettier"],
  "plugins": [],
  "parserOptions": {
    "ecmaVersion": 2021,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "env": {
    "es6": true,
    "browser": true,
    "node": true
  }
}
```

### Parcel

Parcel is a great bundler for JavaScript projects. The first three revisions of this workshop all teach Webpack and end up spending a decent amount of time covering how to set it up. Webpack is a fantastic piece of technology and you should definitely consider using it for your large applications; it's been around a long time and has a lot of support.

That being said, Parcel is an amazing tool with zero-config. It works with everything we want to do out of the box. Since this is a class on React and not build processes, this allows us to focus more on React. Let's go see what it can do for us.

Parcel is going to accept an entry point, crawl through all of its dependencies, and output a single, complete file with all of our code in it. This means we can have large applications with many files and many dependencies. It would be an unmanageable mess. Already our React app has two components in one file: App and Pet. It'd be better if these were in separate files so it'd be easier to keep track of what was where. This is where Parcel can help us.

Install Parcel by doing ```npm install -D parcel@1.12.3```
Now inside of your package.json put:
```sh
"scripts" {
  "dev": "parcel src/index.html"
}
```

### Babel
Babel is a core part of the JavaScript tooling ecosystem. At its core it is a transpilation tool: it takes that looks one way and transforms it it into a different looking set of code. One of its core uses to transform futuristic JavaScript (like ES2021) to an older version of JavaScript (like ES5 i.e. JavaScript before 2015) so that older browsers can use your newer JavaScript. Babel also handles things like JSX for us and it can handle TypeScript too.

Typically Parcel handles 100% of your Babel needs and you don't have to care at all what's going on underneath the hood; its Babel config is well crafted and fits nearly all needs. Its one issue is that it can be slow to update when new capabilities come online and that's why here we're going to modify it a bit.

One thing that Parcel 1 does (Parcel 2 whenever it comes out will work differently) is merge your config with their own. That means we only need to set up the one thing we need to change and leave the rest alone. Run the following command:

```npm install -D @babel/core@7.12.16 @babel/preset-react@7.12.13```

You also need to install the core library from Babel if you're going to provide your own config. Don't worry, if you forget, Parcel will install it for you and update your package.json.

Create a file called .babelrc in your home directory and put this in there:

```json
{
  "presets": [
    [
      "@babel/preset-react",
      {
        "runtime": "automatic"
      }
    ]
  ]
}
```

## Core React Concept

### JSX
So far we've been writing React without JSX, something that I don't know anyone that actually does with their apps. Everyone uses JSX. I show you this way so what JSX is actually doing is demystified to you. It doesn't do hardly anything. It just makes your code a bit more readable.

If I write React.createElement("h1", { id: "main-title" }, "My Website");, what am I actually trying to have rendered out? <h1 id="main-title">My Website</h1>, right? What JSX tries to do is to shortcut this translation layer in your brain so you can just write what you mean. Let's convert Pet.js to using JSX. It will look like this:

```js
// delete the import

const Pet = (props) => {
  return (
    <div>
      <h1>{props.name}</h1>
      <h2>{props.animal}</h2>
      <h2>{props.breed}</h2>
    </div>
  );
};

export default Pet;
```

So now JSX is demystified a bit, let's go convert App.js.

```js
import { render } from "react-dom";
import Pet from "./Pet";

const App = () => {
  return (
    <div>
      <h1>Adopt Me!</h1>
      <Pet name="Luna" animal="dog" breed="Havanese" />
      <Pet name="Pepper" animal="bird" breed="Cockatiel" />
      <Pet name="Doink" animal="cat" breed="Mix" />
    </div>
  );
};

render(<App />, document.getElementById("root"));
```

### Hooks
Now we want to make it so you can modify what your search parameters are. Let's make a new route called SearchParams.js and have it accept these search parameters.

```js
const SearchParams = () => {
  const location = "Seattle, WA";
  return (
    <div className="search-params">
      <form>
        <label htmlFor="location">
          Location
          <input id="location" value={location} placeholder="Location" />
        </label>
        <button>Submit</button>
      </form>
    </div>
  );
};

export default SearchParams;
```

Now add it to your routes:
```js
// delete Pet import, and add SearchParams
import SearchParams from "./SearchParams";

// in App.js, replace all the Pets
<SearchParams />;
```
```js
// in SearchParams.js
import { useState } from "react";

// replace location
const [location, updateLocation] = useState("Seattle, WA");

// replace input
<input
  id="location"
  value={location}
  placeholder="Location"
  onChange={(e) => updateLocation(e.target.value)}
/>;
```
Let's next make the animal drop down.
```js
// under the imports
const ANIMALS = ["bird", "cat", "dog", "rabbit", "reptile"];

// under location
const [animal, updateAnimal] = useState("");

// under the location label
<label htmlFor="animal">
  Animal
  <select
    id="animal"
    value={animal}
    onChange={(e) => updateAnimal(e.target.value)}
    onBlur={(e) => updateAnimal(e.target.value)}
  >
    <option />
    {ANIMALS.map((animal) => (
      <option key={animal} value={animal}>
        {animal}
      </option>
    ))}
  </select>
</label>;
```
Let's make a third dropdown so you can select a breed as well as an animal.
```js
// under your other state inside the component
const [breed, updateBreed] = useState("");
const breeds = [];

// under the animal label
<label htmlFor="breed">
  Breed
  <select
    disabled={!breeds.length}
    id="breed"
    value={breed}
    onChange={(e) => updateBreed(e.target.value)}
    onBlur={(e) => updateBreed(e.target.value)}
  >
    <option />
    {breeds.map((breed) => (
      <option key={breed} value={breed}>
        {breed}
      </option>
    ))}
  </select>
</label>;
```

### useEffect
We have enough to start making some requests now. We want the app to request an initial set of pets on initial load of the page. So let's make that happen using a special hook called useEffect. useEffect allows you to say "do a render of this component first so the user can see something then as soon as the render is done, then do something (the something here being an effect.) In our case, we want the user to see our UI first then we want to make a request to the API so we can that initial list of pets.

Add this to SearchParams.js
```js
// change import at top
import { useEffect, useState } from "react";
import Pet from "./Pet";

// add to the other useStates inside component at top
const [pets, setPets] = useState([]);

// add inside component, beneath all the `useState` setup
useEffect(() => {
  requestPets();
}, []); // eslint-disable-line react-hooks/exhaustive-deps

async function requestPets() {
  const res = await fetch(
    `http://pets-v2.dev-apis.com/pets?animal=${animal}&location=${location}&breed=${breed}`
  );
  const json = await res.json();

  setPets(json.pets);
}

// in jsx, under form, inside the larger div
{
  pets.map((pet) => (
    <Pet name={pet.name} animal={pet.animal} breed={pet.breed} key={pet.id} />
  ));
}
```

### Custom Hooks
For now, we're going to make a custom hook of our own. Just like useState is a hook, there are a few others like useEffect (which we'll use in this lesson), useReducer (for doing Redux-like reducers), useRefs (for when you need to have programmatic access to a DOM node), and useContext (for using React's context which we'll do shortly as well.) But like React hooks, we can use these hooks to make our re-usable hooks.

We need a list of breeds based on which animal is selected. In general this would be nice to request once and if a user returns later to the same animal, that we would have some cache of that. We could implement in the component (and in general I probably would, this is overengineering it for just one use) but let's make a custom hook for it.

Make a new file called useBreedList.js in src and put this in it.

```js
import { useState, useEffect } from "react";

const localCache = {};

export default function useBreedList(animal) {
  const [breedList, setBreedList] = useState([]);
  const [status, setStatus] = useState("unloaded");

  useEffect(() => {
    if (!animal) {
      setBreedList([]);
    } else if (localCache[animal]) {
      setBreedList(localCache[animal]);
    } else {
      requestBreedList();
    }

    async function requestBreedList() {
      setBreedList([]);
      setStatus("loading");
      const res = await fetch(
        `http://pets-v2.dev-apis.com/breeds?animal=${animal}`
      );
      const json = await res.json();
      localCache[animal] = json.breeds || [];
      setBreedList(localCache[animal]);
      setStatus("loaded");
    }
  }, [animal]);

  return [breedList, status];
}
```
Head over to SearchParam.js and put this in there.
```js
import useBreedList from "./useBreedList";

// replace `const breeds = [];`
const [breeds] = useBreedList(animal);
```

### Strict Mode
React has a new strict mode. If you wrap your app in <React.StrictMode></React.StrictMode> it will give you additional warnings about things you shouldn't be doing. I'm not teaching you anything that would trip warnings from React.StrictMode but it's good to keep your team in line and not using legacy features or things that will be soon be deprecated.

Go to App.js and wrap <App /> in the render call in <StrictMode>.
```js
// import at top
import { StrictMode } from "react";

// replace render
render(
  <StrictMode>
    <App />
  </StrictMode>,
  document.getElementById("root")
);
```

## Specials Case React Tools

### Error Boundaries
Frequently there's errors with APIs with malformatted or otherwise weird data. Let's be defensive about this because we still want to use this API but we can't control when we get errors. We're going to use a feature called componentDidCatch to handle this. This is something you can't do with hooks so if you needed this sort of functionality you'd have to use a class component.

This will also catch 404s on our API if someone give it an invalid ID!

A component can only catch errors in its children, so that's important to keep in mind. It cannot catch its own errors. Let's go make a wrapper to use on Details.js. Make a new file called ErrorBoundary.js

```There are no Hook equivalents to the uncommon componentDidCatch lifecycles yet, but we plan to add them soon.```

resource : https://reactjs.org/docs/hooks-faq.html#do-hooks-cover-all-use-cases-for-classes

### Context
Historically I have not taught context when teaching React. This was for a couple reasons. First of all, the API they were using was still unofficial, however they standardized it in version 16. Secondly, normally you don't need context; React's state is enough. Thirdly, the old API was bad, in my opinion. The new one is pretty good.

So here we go. What is context? Context is like state, but instead of being confined to a component, it's global to your application. It's application-level state. This is dangerous. Avoid using context until you have to use it. One of React's primary benefit is it makes the flow of data obvious by being explicit. This can make it cumbersome at times but it's worth it because your code stays legible and understandable. Things like context obscure it.

Context (mostly) replaces Redux. Well, typically. It fills the same need as Redux. I really can't see why you would need to use both. Use one or the other.

Again, this is a contrived example. What we're doing here is overkill and should be accomplished via React's normal patterns. A better example would be something like a user's logged-in information. But let's check out what this looks like with theme.

Imagine if we wanted to let the user choose a simple theme for the site. And we want to make that theme stick as the user navigates across different pages. This means the state has to live outside of the route where it's selected. We could use Redux for this, we could use React itself, or we're going to use context, to teach you what that looks like.

Make a new file called ThemeContext.js:
```js
import { createContext } from "react";

const ThemeContext = createContext(["green", () => {}]);

export default ThemeContext;
```
```js
// import useState and ThemeContext
import { StrictMode, useState } from "react";
import ThemeContext from "./ThemeContext";

// top of App function body
const theme = useState("darkblue");

// wrap the rest of the app
<ThemeContext.Provider value={theme}>[…]</ThemeContext.Provider>;
```

Resource:
- https://btholt.github.io/complete-intro-to-react-v6
- https://reactjs.org/docs/getting-started.html