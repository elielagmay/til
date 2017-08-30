### Find Redux wrapped children

```
const component = shallow(<MyComponent />)
expect(component.find('Connect(ChildComponent)')).to.have.length(1)
```

### Find Apollo wrapped children

```
const component = shallow(<MyComponent />)
expect(component.find('Apollo(ChildComponent)')).to.have.length(1)
```
