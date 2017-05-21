# React.js

React is, in my opinion, a really good framework, although it feels a bit heave and clunky, sometimes.

It's best used in medium-sized projects, since it's way too heavy for small projects, and it can feel weird in big projects if you use it without something like flux or redux.

The core of React is the idea of a `component`: a unit, or module, with its own methods and properties.

It's common for React applications to use JSX, which is basically JavaScript on steroids: it allows HTML-like syntax, so instead of passing strings to your methods, you can actually pass HTML.

## create-react-app

A really good tool for generating and managing REACT apps is the `npm` module `create-react-app`, which basically jump-starts your project. It includes scripts for running and building your app, and it also has webpack with babel and style loaders and a test environment (Jest + enzyme) ready.

When you generate the app it takes care of building and serving the app automatically, but you can run the command `npm eject` to take full control over it.

## bind

One of the most common errors I ever encountered while writing React code was caused by a missed `bind(this)` call.

`bind` is important when you, for example, pass functions as parameters in an element: it basically sets the context for the `this` keyword. If a function altered a property in an element's state, for example, you'd want to bind it to that same element, since otherwise, when you called `this.setState()` in another element, `this` would be referred to that element, not to the one where the function originally came from.

## Passing methods through properties

One of the things I originally had some difficoulty in grasping, was how to pass methods which needed to be tied to events through components: for example, if I had a parent element, which needed to have its state altered by a function, which needed to be triggered by a child element.

The solution is this: write the function to alter the parent's state in the parent and then, in the parent's render method, pass the function to the child as a property:

```javascript
render() {
  return(
    <div>
      <Child the-method={this.theMethod.bind(this)}/>
    </div>
  )
}
```

## Constructors!!

Ok, so one of the things I used to get wrong when I started using React is that I forgot to call `super()` in an element's constructor before trying to access `this`. I know it's technically not a React-exclusive thing, but if you don't write a lot of ES6, then it's pretty difficult to come across this problem:

```javascript
class Element extends React.Component {
  constructor(props) {
    super(props)

    this.setState({})
  }
}
```

If I didn't call `super(props)` before setting the state, I'd get an error.
