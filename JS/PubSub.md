# Design patterns
In programming, a design pattern is a way of structuring your code that enables you to write code for different purposes, similarly to programming paradigms.

The Pub Sub model is based on the idea of having a dispatcher PUBlish some event, and other parts of the code SUBscribe to that event, in such a way that you can very well separate your logic from either your routes or view.

I personally really enjoy using this pattern, because it feels like you're keeping everything simpler, since your view isn't crowded with complex logic.

Let's talk about implementations: there are not many "pure" PubSub libraries, although there are similar things (for example the Observable Pattern is pretty popular). I made a library for it myself: [PubbySubby](https://www.npmjs.com/package/pubby-subby).

The implementation is really easy, so let's take a look:

```javascript
class PubSub {
  constructor(topics={}) {
    this.topics = topics
  }

  sub(topicName, action) {
    this.topics[topicName] = this.topics[topicName] || []
    this.topics[topicName].push(action)
    return this
  }

  unsub(topicName, action) {
    let found = false
    if(this.topics[topicName]) {
      this.topics[topicName].forEach((res, i) => {
        if(res == action) {
          this.topics[topicName].splice(i, 1)
          found = true
          return this
        }
      })
    }
  }

  pub(topicName) {
    if(this.topics[topicName]) {
      this.topics[topicName].forEach(res => res())
    }
  }
}
```

Looks good! In a few lines of code, we wrote a functioning implementation for one of the most used design patterns of today: the PubSub model!

Let's take a closer look at the code:

First of all, we're creating a PubSub class, which we will have to instantiate later. This class has a field for each one of its handlers. We also need functions to register and remove handlers: we'll call these `sub` and `unsub`. We also want a `pub` function to publish an event and call all the handlers on it.

## Sub

```javascript
sub(topicName, action) {
  this.topics[topicName] = this.topics[topicName] || []
  this.topics[topicName].push(action)
  return this
}
```
Our sub function takes a topic name and an action, and it sets and handler for the topicName.

How we do this is we basically set a field of our class variable called `topics` to be the `action` that gets passed in. This is what happens when we call the sub function:

```javascript
let pubsub = new PubSub()

pubsub.sub('doSomething', () => console.log('Did something'))
```

At this point `pubsub.topics` equals an object:

```javascript
{
  doSomething: function() {
    console.log('Did something')
  }
}
```

No we need a way to call registered handlers: a `pub` function.

## Pub
Let's take a look at the `sub` function in our class:

```javascript
pub(topicName) {
  if(this.topics[topicName]) {
    this.topics[topicName].forEach(res => res())
  }
}
```

It only takes one argument: the name of the topic we want to publish. We then want to check whether we have registered such a topic in out instance, and if we have we want to loop over each handler for that topic, and execute it.

Let's say that we wanted to remove a topic. How would we go about that?

## Unsub
```javascript
unsub(topicName, action) {
  if(this.topics[topicName]) {
    this.topics[topicName].forEach((res, i) => {
      if(res == action) {
        this.topics[topicName].splice(i, 1)
        return this
      }
    })
  }
}
```

Let's dive into the `unsub` function:

The function takes two arguments: the name of the topic we want to remove the handler from and the handler function.

And you may be wandering:

> Why would we need both of those? Isn't one enough?

And, actually, no, it isn't: imagine we have multiple  handlers on a single topic or, vice versa, the same handler on multiple topics. Removing them all wouldn't really make sense, right?

Ok, so we want to check if we have registered a topic with the name we are passing to the function. If there is such a topic, then we want to loop over each one of the handlers on the topic and, if any one of them matches the function we passed as an argument, we want to remove it from the element's handler list.

This is really about all you need to build a functioning PubSub implementation. Of course you could do some more stuff, build payloads into it, but that's more complicated stuff that isn't really necessary for a basic implementation.

## Conclusion
In conclusion I do recommend that you try the PubSub model out, and that you write your own, better, implementation, mostly because it's the best way to really get to understand something: when in doubt, just re-write it from scratch!
