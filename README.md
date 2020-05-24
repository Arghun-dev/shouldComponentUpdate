# shouldComponentUpdate

in `shouldComponentUpdate` lifecycle method you prevent a component to re-render until some condition is true

Example:

As you can see, the shouldComponentUpdate class method has access to the next props and state before running the re-rendering of a component. That's where you can decide to prevent the re-render by returning false from this method. If you return true, the component re-renders.

```
class Square extends Component {
  shouldComponentUpdate(nextProps, nextState) {
    if (this.props.number === nextProps.number) {
      return false;
    } else {
      return true;
    }
  }
 
  render() {
    return <Item>{this.props.number * this.props.number}</Item>;
  }
}
```

in this case, if the incoming number prop didn't change, the component should not update. 


# React's Pure Component

In the previous case, you have used `shouldComponentUpdate` to prevent a rerender of the child component. It can be used to prevent component renderings on a fine-grained level: You can apply equality checks for different props and state, but also use it for other kind of checks. However, imagine you are not interested in checking each incoming prop by itself, which can be error prone too, but only in preventing a rerendering when nothing relevant (props, state) has changed for the component. That's where you can use the more broad yet simpler solution for preventing the rerender: React's PureComponent.

Example:

```
import React, { Component, PureComponent } from 'react';
 
...
 
class Square extends PureComponent {
  render() {
    return <Item>{this.props.number * this.props.number}</Item>;
  }
}
```

As alternative, if you want to use a functional stateless component as PureComponent instead, use recompose's pure higher-order component. You can install recompose on the command line via npm with `npm install recompose`. Then apply its higher-order component on your initially implemented Square component:

```
import { pure } from 'recompose';
 
...
 
const Square = pure(({ number }) => <Item>{number * number}</Item>);
```

Under the hood, recompose applies React's PureComponent for you. Again I encourage you to add console logs to your components to experience the rerenders of each component.

This small yet powerfull React performance optimization tutorial has shown you examples for React's shouldComponentUpdate() and React's PureComponent. As you have seen, you can also use higher-order components that implement these performance optimizations for you. You can find the finished application in this.

After all, you can always use console log statements to track your component rerenders. If shouldComponentUpdate is not called, check whether the props or state have changed in the first place, because this is a major source of this lifecycle method not being called. On the other hand, you should use these performance optimizations in React carefully, because preventing accidentally a rerendering may lead to unexpected bugs. Because of its virtual DOM, React by itself is a performant library, and you can rely on this fact until something becomes a performance bottleneck in your component hierarchy. It's usually the case when rendering a large list of data. Then it's recommended to check the internal implementation for the component of an item in a list. Maybe before using shouldComponentUpdate or PureComponent, you should change the implementation of the component in the first place.
