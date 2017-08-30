### Find child components wrapped in Higher Order Components (HOC)

I spent a full hour wondering why I can't query a child component when shallow rendering using enzyme. I was sure the component is there; the code being tested is fairly straightforward. So why can't enzyme find the components?

Turns out if it's wrapped in a higher order component like Redux's `connect` or Apollo's `graphql`, then it's not possible to query it with just the child component's name.

#### Redux

```jsx
const component = shallow(<MyComponent />)
expect(component.find('Connect(ChildComponent)')).to.have.length(1)
```

#### Apollo

```jsx
const component = shallow(<MyComponent />)
expect(component.find('Apollo(ChildComponent)')).to.have.length(1)
```
