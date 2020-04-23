## Function and Class Components
* a function component
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
* a component
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
