## React代码片段精选

## Table of Contents

### Array

* [DataList](#datalist)
* [DataTable](#datatable)
* [MappedTable](#mappedtable)

### Input

* [Input](#input)
* [LimitedTextarea](#limitedtextarea)
* [PasswordRevealer](#passwordrevealer)
* [Select](#select)
* [TextArea](#textarea)

### Object

* [TreeView](#treeview)

### String

* [AutoLink](#autolink)

### Visual

* [Carousel](#carousel)
* [Collapse](#collapse)
* [FileDrop](#filedrop)
* [Mailto](#mailto)
* [StarRating](#starrating)
* [Tab](#tab)
* [Ticker](#ticker)
* [Toggle](#toggle)
* [Tooltip](#tooltip)
---

## Array

### DataList

Renders a list of elements from an array of primitives.
Use the value of the `isOrdered` prop to conditionally render a `<ol>` or `<ul>` list.
Use `Array.prototype.map` to render every item in `data` as a `<li>` element, give it a `key` produced from the concatenation of the its index and value.
Omit the `isOrdered` prop to render a `<ul>` list by default.

```jsx
function DataList({ isOrdered, data }) {
  const list = data.map((val, i) => (
    <li key={`${i}_${val}`}>{val}</li>
  ));
  return isOrdered ? <ol>{list}</ol> : <ul>{list}</ul>;
}
```


```jsx
const names = ['John', 'Paul', 'Mary'];
ReactDOM.render(<DataList data={names}/>, document.getElementById('root'));
ReactDOM.render(<DataList data={names} isOrdered/>, document.getElementById('root'));
```
<br>[⬆ Back to top](#table-of-contents)

### DataTable

Renders a table with rows dynamically created from an array of primitives.
Render a `<table>` element with two columns (`ID` and `Value`).
Use `Array.prototype.map` to render every item in `data` as a `<tr>` element, consisting of its index and value, give it a `key` produced from the concatenation of the two.

```jsx
function DataTable({ data }) {
  return (
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Value</th>
        </tr>
      </thead>
      <tbody>
        {data.map((val, i) =>
          <tr key={`${i}_${val}`}>
            <td>{i}</td>
            <td>{val}</td>
          </tr>
        )}
      </tbody>
    </table>
  );
}
```


```jsx
const people = ['John', 'Jesse'];
ReactDOM.render(
  <DataTable data={people} />,
  document.getElementById('root')
);
```
<br>[⬆ Back to top](#table-of-contents)

### MappedTable

Renders a table with rows dynamically created from an array of objects and a list of property names.
Use `Object.keys()`, `Array.prototype.filter()`, `Array.prototype.includes()` and `Array.prototype.reduce()` to produce a `filteredData` array, containing all objects with the keys specified in `propertyNames`.
Render a `<table>` element with a set of columns equal to the amount of values in `propertyNames`.
Use `Array.prototype.map` to render each value in the `propertyNames` array as a `<th>` element.
Use `Array.prototype.map` to render each object in the `filteredData` array as a `<tr>` element, containing a `<td>` for each key in the object.

```jsx
function MappedTable({ data, propertyNames }) {
  let filteredData = data.map(v =>
    Object.keys(v)
      .filter(k => propertyNames.includes(k))
      .reduce((acc, key) => ((acc[key] = v[key]), acc), {})
  );
  return (
    <table>
      <thead>
        <tr>{propertyNames.map(val => <th key={`h_${val}`}>{val}</th>)}</tr>
      </thead>
      <tbody>
        {filteredData.map((val, i) => (
          <tr key={`i_${i}`}>
            {propertyNames.map(p => <td key={`i_${i}_${p}`}>{val[p]}</td>)}
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```
#
### Notes

This component does not work with nested objects and will break if there are nested objects inside any of the properties specified in `propertyNames`.,<!-tags: array,object -->,<!-expertise: 1 -->

```jsx
const people = [
  { name: 'John', surname: 'Smith', age: 42 },
  { name: 'Adam', surname: 'Smith', gender: 'male' }
];
const propertyNames = ['name', 'surname', 'age'];
ReactDOM.render(
  <MappedTable data={people} propertyNames={propertyNames} />,
  document.getElementById('root')
);
```
<br>[⬆ Back to top](#table-of-contents)

## Input

### Input

Renders an `<input>` element that uses a callback function to pass its value to the parent component.
Use object destructuring to set defaults for certain attributes of the `<input>` element.
Render an `<input>` element with the appropriate attributes and use the `callback` function in the `onChange` event to pass the value of the input to the parent.

```jsx
function Input ({ callback, type = 'text', disabled = false, readOnly = false, placeholder = '' }) {
  return (
    <input
      type={type}
      disabled={disabled}
      readOnly={readOnly}
      placeholder={placeholder}
      onChange={({ target: { value } }) => callback(value)}
    />
  );
}
```


```jsx
ReactDOM.render(
  <Input type='text' placeholder='Insert some text here...' callback={(val) => console.log(val)}/>,
  document.getElementById('root')
);
```
<br>[⬆ Back to top](#table-of-contents)

### LimitedTextarea

Renders a textarea component with a character limit.
Use the `React.useState()` hook to create the `content` state variable and set its value to `value`.
Create a method `setFormattedContent`, which trims the content of the input if it's longer than `limit`.
Use the `React.useEffect()` hook to call the `setFormattedContent` method on the value of the `content` state variable.
Use a`<div>` to wrap both the`<textarea>` and the `<p>` element that displays the character count and bind the `onChange` event of the `<textarea>` to call `setFormattedContent` with the value of `event.target.value`.

```jsx
function LimitedTextarea({ rows, cols, value, limit }) {
  const [content, setContent] = React.useState(value);
  const setFormattedContent = text => {
    text.length > limit ? setContent(text.slice(0, limit)) : setContent(text);
  };
  React.useEffect(() => {
    setFormattedContent(content);
  }, []);
  return (
    <div>
      <textarea
        rows={rows}
        cols={cols}
        onChange={event => setFormattedContent(event.target.value)}
        value={content}
      />
      <p>
        {content.length}/{limit}
      </p>
    </div>
  );
}
```


```jsx
ReactDOM.render(
  <LimitedTextarea limit={32} value='Hello!' />,
  document.getElementById('root')
);
```
<br>[⬆ Back to top](#table-of-contents)

### PasswordRevealer

Renders a password input field with a reveal button.
Use the `React.useState()` hook to create the `shown` state vairable and set its value to `false`.
Use a`<div>` to wrap both the`<input>` and the `<button>` element that toggles the type of the input field between `"text"` and `"password"`.

```jsx
function PasswordRevealer({ value }) {
  const [shown, setShown] = React.useState(false);
  return (
    <div>
      <input
        type={shown ? "text" : "password"}
        value={value}
        onChange={() => {}}
      />
      <button onClick={() => setShown(!shown)}>Show/Hide</button>
    </div>
  );
}
```


```jsx
ReactDOM.render(<PasswordRevealer />, document.getElementById('root'));
```
<br>[⬆ Back to top](#table-of-contents)

### Select

Renders a `<select>` element that uses a callback function to pass its value to the parent component.
Use object destructuring to set defaults for certain attributes of the `<select>` element.
Render a `<select>` element with the appropriate attributes and use the `callback` function in the `onChange` event to pass the value of the textarea to the parent.
Use destructuring on the `values` array to pass an array of `value` and  `text` elements and the `selected` attribute to define the initial `value` of the `<select>` element.

```jsx
function Select ({ values, callback, disabled = false, readonly = false, selected }) {
  return (
    <select
      disabled={disabled}
      readOnly={readonly}
      onChange={({ target : { value } }) => callback(value)}
    >
      {values.map(([value, text]) => <option selected={selected === value}value={value}>{text}</option>)}
    </select>
  );
}
```


```jsx
let choices = [
  ['grapefruit', 'Grapefruit'],
  ['lime', 'Lime'],
  ['coconut', 'Coconut'],
  ['mango', 'Mango']
];
ReactDOM.render(
  <Select values={choices} selected='lime' callback={(val) => console.log(val)}/>,
  document.getElementById('root')
);
```
<br>[⬆ Back to top](#table-of-contents)

### TextArea

Renders a `<textarea>` element that uses a callback function to pass its value to the parent component.
Use object destructuring to set defaults for certain attributes of the `<textarea>` element.
Render a `<textarea>` element with the appropriate attributes and use the `callback` function in the `onChange` event to pass the value of the textarea to the parent.

```jsx
function TextArea ({ callback, cols = 20, rows = 2, disabled = false, readOnly = false, placeholder='' }) {
  return (
    <textarea
      cols={cols}
      rows={rows}
      disabled={disabled}
      readOnly={readOnly}
      placeholder={placeholder}
      onChange={({ target : { value } }) => callback(value)}
    />
  );
}
```


```jsx
ReactDOM.render(
  <TextArea placeholder='Insert some text here...' callback={(val) => console.log(val)}/>,
  document.getElementById('root')
);
```
<br>[⬆ Back to top](#table-of-contents)

## Object

### TreeView

Renders a tree view of a JSON object or array with collapsible content.
Use object destructuring to set defaults for certain props. 
Use the value of the `toggled` prop to determine the initial state of the content (collapsed/expanded).
Use the `React.setState()` hook to create the `isToggled` state variable and give it the value of the `toggled` prop initially.
Return a `<div>` to wrap the contents of the component and the `<span>` element, used to alter the component's `isToggled` state.
Determine the appearance of the component, based on `isParentToggled`, `isToggled`, `name` and `Array.isArray()` on `data`. 
For each child in `data`, determine if it is an object or array and recursively render a sub-tree.
Otherwise, render a `<p>` element with the appropriate style.

```css
.tree-element {
  margin: 0;
  position: relative;
}
div.tree-element:before {
  content: '';
  position: absolute;
  top: 24px;
  left: 1px;
  height: calc(100% - 48px);
  border-left: 1px solid gray;
}
.toggler {
  position: absolute;
  top: 10px;
  left: 0px;
  width: 0; 
  height: 0; 
  border-top: 4px solid transparent;
  border-bottom: 4px solid transparent;
  border-left: 5px solid gray;
  cursor: pointer;
}
.toggler.closed {
  transform: rotate(90deg);
}
.collapsed {
  display: none;
}
```

```jsx
function TreeView({
  data,
  toggled = true,
  name = null,
  isLast = true,
  isChildElement = false,
  isParentToggled = true
}) {
  const [isToggled, setIsToggled] = React.useState(toggled);
  return (
    <div
      style={ { marginLeft: isChildElement ? 16 : 4 + "px" }}
      className={isParentToggled ? "tree-element" : "tree-element collapsed"}
    >
      <span
        className={isToggled ? "toggler" : "toggler closed"}
        onClick={() => setIsToggled(!isToggled)}
      />
      {name ? <strong>&nbsp;&nbsp;{name}: </strong> : <span>&nbsp;&nbsp;</span>}
      {Array.isArray(data) ? "[" : "{"}
      {!isToggled && "..."}
      {Object.keys(data).map(
        (v, i, a) =>
          typeof data[v] == "object" ? (
            <TreeView
              data={data[v]}
              isLast={i === a.length - 1}
              name={Array.isArray(data) ? null : v}
              isChildElement
              isParentToggled={isParentToggled && isToggled}
            />
          ) : (
            <p
              style={ { marginLeft: 16 + "px" }}
              className={isToggled ? "tree-element" : "tree-element collapsed"}
            >
              {Array.isArray(data) ? "" : <strong>{v}: </strong>}
              {data[v]}
              {i === a.length - 1 ? "" : ","}
            </p>
          )
      )}
      {Array.isArray(data) ? "]" : "}"}
      {!isLast ? "," : ""}
    </div>
  );
}
```


```jsx
let data = {
  lorem: {
    ipsum: "dolor sit",
    amet: {
      consectetur: "adipiscing",
      elit: [
        "duis",
        "vitae",
        {
          semper: "orci"
        },
        {
          est: "sed ornare"
        },
        "etiam",
        ["laoreet", "tincidunt"],
        ["vestibulum", "ante"]
      ]
    },
    ipsum: "primis"
  }
};
ReactDOM.render(<TreeView data={data} name='data'/>, document.getElementById("root"));
```
<br>[⬆ Back to top](#table-of-contents)

## String

### AutoLink

Renders a string as plaintext, with URLs converted to appropriate `<a>` elements.
Use `String.prototype.split()` and `String.prototype.match()` with a regular expression to find URLs in a string.
Return a  `<React.Fragment>` with matched URLs rendered as `<a>` elements, dealing with missing protocol prefixes if necessary, and the rest of the string rendered as plaintext.

```jsx
function AutoLink({ text }) {
  const delimiter = /((?:https?:\/\/)?(?:(?:[a-z0-9]?(?:[a-z0-9\-]{1,61}[a-z0-9])?\.[^\.|\s])+[a-z\.]*[a-z]+|(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)(?:\.(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3})(?::\d{1,5})*[a-z0-9.,_\/~#&=;%+?\-\\(\\)]*)/gi;
  return (
    <React.Fragment>
      {text.split(delimiter).map(word => {
        let match = word.match(delimiter);
        if (match) {
          let url = match[0];
          return (
            <a href={url.startsWith("http") ? url : `http://${url}`}>{url}</a>
          );
        }
        return word;
      })}
    </React.Fragment>
  );
}
```


```jsx
ReactDOM.render(
  <AutoLink text='foo bar baz http://example.org bar' />,
  document.getElementById('root')
);
```
<br>[⬆ Back to top](#table-of-contents)

## Visual

### Carousel

Renders a carousel component.
Use the `React.setState()` hook to create the `active` state variable and give it a value of `0` (index of the first item).
Use an object, `style`, to hold the styles for the individual components.
Use the `React.setEffect()` hook to update the value of `active` to the index of the next item, using `setTimeout`.
Destructure `props`, compute if visibility style should be set to `visible` or not for each carousel item while mapping over and applying the combined style to the carousel item component accordingly.
Render the carousel items using `React.cloneElement()` and pass down rest `props` along with the computed styles.

```jsx
function Carousel(props) {
  const [active, setActive] = React.useState(0);
  let scrollInterval = null;
  const style = {
    carousel: {
      position: "relative"
    },
    carouselItem: {
      position: "absolute",
      visibility: "hidden"
    },
    visible: {
      visibility: "visible"
    }
  };
  React.useEffect(() => {
    scrollInterval = setTimeout(() => {
      const { carouselItems } = props;
      setActive((active + 1) % carouselItems.length);
    }, 2000);
  });
  const { carouselItems, ...rest } = props;
  return (
    <div style={style.carousel}>
      {carouselItems.map((item, index) => {
        const activeStyle = active === index ? style.visible : {};
        return React.cloneElement(item, {
          ...rest,
          style: {
            ...style.carouselItem,
            ...activeStyle
          }
        });
      })}
    </div>
  );
}
```


```jsx
ReactDOM.render(
  <Carousel
    carouselItems={[
      <div>carousel item 1</div>,
      <div>carousel item 2</div>,
      <div>carousel item 3</div>
    ]}
  />,
  document.getElementById("root")
);
```
<br>[⬆ Back to top](#table-of-contents)

### Collapse

Renders a component with collapsible content.
Use the `React.setState()` hook to create the `isCollapsed` state variable with an initial value of `props.collapsed`.
Use an object, `style`, to hold the styles for individual components and their states.
Use a `<div>` to wrap both the `<button>` that alters the component's `isCollapsed` state and the content of the component, passed down via `props.children`.
Determine the appearance of the content, based on `isCollapsed` and apply the appropriate CSS rules from the `style` object.
Finally, update the value of the `aria-expanded` attribute based on `isCollapsed` to make the component accessible.

```jsx
function Collapse(props) {
  const [isCollapsed, setIsCollapsed] = React.useState(props.collapsed);
  const style = {
    collapsed: {
      display: "none"
    },
    expanded: {
      display: "block"
    },
    buttonStyle: {
      display: "block",
      width: "100%"
    }
  };
  return (
    <div>
      <button
        style={style.buttonStyle}
        onClick={() => setIsCollapsed(!isCollapsed)}
      >
        {isCollapsed ? "Show" : "Hide"} content
      </button>
      <div
        className="collapse-content"
        style={isCollapsed ? style.collapsed : style.expanded}
        aria-expanded={isCollapsed}
      >
        {props.children}
      </div>
    </div>
  );
}
```


```jsx
ReactDOM.render(
  <Collapse>
    <h1>This is a collapse</h1>
    <p>Hello world!</p>
  </Collapse>,
  document.getElementById('root')
);
```
<br>[⬆ Back to top](#table-of-contents)

### FileDrop

Renders a file drag and drop component for a single file.
Create a ref called `dropRef` for this component.
Use the `React.useState()` hook to create the `drag` and `filename` variables, initialized to `false` and `''` respectively.
The variables `dragCounter` and `drag` are used to determine if a file is being dragged, while `filename` is used to store the dropped file's name.
Create the `handleDrag`, `handleDragIn`, `handleDragOut` and `handleDrop` methods to handle drag and drop functionality,  bind them to the component's context.
Each of the methods will handle a specific event, the listeners for which are created and removed in the `React.useEffect()` hook and its attached `cleanup()` method.
`handleDrag` prevents the browser from opening the dragged file, `handleDragIn` and `handleDragOut` handle the dragged file entering and exiting the component, while `handleDrop` handles the file being dropped and passes it to `props.handleDrop`.
Return an appropriately styled `<div>` and use `drag` and `filename` to determine its contents and style. 
Finally, bind the `ref` of the created `<div>` to `dropRef`.

```css
.filedrop {
  min-height: 120px;
  border: 3px solid #D3D3D3;
  text-align: center;
  font-size: 24px;
  padding: 32px;
  border-radius: 4px;
}
.filedrop.drag {
  border: 3px dashed #1E90FF;
}
.filedrop.ready {
  border: 3px solid #32CD32;
}
```


```jsx
function FileDrop(props) {
  const [drag, setDrag] = React.useState(false);
  const [filename, setFilename] = React.useState('');
  let dropRef = React.createRef();
  let dragCounter = 0;
  const handleDrag = e => {
    e.preventDefault();
    e.stopPropagation();
  };
  const handleDragIn = e => {
    e.preventDefault();
    e.stopPropagation();
    dragCounter++;
    if (e.dataTransfer.items && e.dataTransfer.items.length > 0) setDrag(true);
  };
  const handleDragOut = e => {
    e.preventDefault();
    e.stopPropagation();
    dragCounter--;
    if (dragCounter === 0) setDrag(false);
  };
  const handleDrop = e => {
    e.preventDefault();
    e.stopPropagation();
    setDrag(false);
    if (e.dataTransfer.files && e.dataTransfer.files.length > 0) {
      props.handleDrop(e.dataTransfer.files[0]);
      setFilename(e.dataTransfer.files[0].name);
      e.dataTransfer.clearData();
      dragCounter = 0;
    }
  };
  React.useEffect(() => {
    let div = dropRef.current;
    div.addEventListener("dragenter", handleDragIn);
    div.addEventListener("dragleave", handleDragOut);
    div.addEventListener("dragover", handleDrag);
    div.addEventListener("drop", handleDrop);
    return function cleanup() {
      div.removeEventListener("dragenter", handleDragIn);
      div.removeEventListener("dragleave", handleDragOut);
      div.removeEventListener("dragover", handleDrag);
      div.removeEventListener("drop", handleDrop);
    };
  });
  return (
    <div
      ref={dropRef}
      className={
        drag ? "filedrop drag" : filename ? "filedrop ready" : "filedrop"
      }
    >
      {filename && !drag ? <div>{filename}</div> : <div>Drop files here!</div>}
    </div>
  );
}
```


```jsx
ReactDOM.render(<FileDrop handleDrop={console.log}/>, document.getElementById('root'));
```
<br>[⬆ Back to top](#table-of-contents)

### Mailto

Renders a link formatted to send an email.
Destructure the component's props, use `email`, `subject` and `body` to create a `<a>` element with an appropriate `href` attribute.
Render the link with `props.children` as its content.

```jsx
function Mailto({ email, subject, body, ...props }) {
  return (
    <a href={`mailto:${email}?subject=${subject || ""}&body=${body || ""}`}>
      {props.children}
    </a>
  );
}
```


```jsx
ReactDOM.render(
  <Mailto email="foo@bar.baz" subject="Hello" body="Hello world!">
    Mail me!
  </Mailto>,
  document.getElementById("root")
);
```
<br>[⬆ Back to top](#table-of-contents)

### StarRating

Renders a star rating component.
Define a component, called `Star` that will render each individual star with the appropriate appearance, based on the parent component's state.
In the `StarRating` component, use the `React.setState()` hook to define the `rating` and `selection` state variables with the initial values of `props.rating` (or `0` if invalid or not supplied) and `0`.
Create a method, `hoverOver`, that updates `selected` and `rating` according to the provided `event`.
Create a `<div>` to wrap the `<Star>` components, which are created using `Array.prototype.map` on an array of 5 elements, created using `Array.from`, and handle the `onMouseLeave` event to set `selection` to `0`, the `onClick` event to set the `rating` and the `onMouseOver` event to set `selection` to the `star-id` attribute of the `event.target` respectively. 
Finally, pass the appropriate values to each `<Star>` component (`starId` and `marked`).

```jsx
function Star({ marked, starId }) {
  return (
    <span star-id={starId} style={ { color: "#ff9933" }} role="button">
      {marked ? "\u2605" : "\u2606"}
    </span>
  );
}
function StarRating(props) {
  const [rating, setRating] = React.useState(
    typeof props.rating == "number" ? props.rating : 0
  );
  const [selection, setSelection] = React.useState(0);
  const hoverOver = event => {
    let val = 0;
    if (event && event.target && event.target.getAttribute("star-id"))
      val = event.target.getAttribute("star-id");
    setSelection(val);
  };
  return (
    <div
      onMouseOut={() => hoverOver(null)}
      onClick={() =>
        setRating(event.target.getAttribute("star-id") || this.state.rating)
      }
      onMouseOver={hoverOver}
    >
      {Array.from({ length: 5 }, (v, i) => (
        <Star
          starId={i + 1}
          key={`star_${i + 1} `}
          marked={selection ? selection >= i + 1 : rating >= i + 1}
        />
      ))}
    </div>
  );
}
```


```jsx
ReactDOM.render(<StarRating/>, document.getElementById('root'));
ReactDOM.render(<StarRating rating={2} />, document.getElementById('root'));
```
<br>[⬆ Back to top](#table-of-contents)

### Tab

Renders a tabbed menu and view component.
Define a `TabItem` component, pass it to the `Tab` and remove unnecessary nodes expect for `TabItem` by identifying the function's name in `props.children`.
Use the `React.useState()` hook to initialize the value of the `bindIndex` state variable to `props.defaultIndex`. 
Use `Array.prototype.map` on the collected nodes to render the `tab-menu` and `tab-view`. 
Define `changeTab`, which will be executed when clicking a `<button>` from the `tab-menu`.
`changeTab` executes the passed callback, `onTabClick` and updates `bindIndex`, which in turn causes a re-render, evaluating the `style` and `className` of the `tab-view` items and `tab-menu` buttons according to their `index`.

```css
.tab-menu > button {
  cursor: pointer;
  padding: 8px 16px;
  border: 0;
  border-bottom: 2px solid transparent;
  background: none;
}
.tab-menu > button.focus {
  border-bottom: 2px solid #007BEF;
}
.tab-menu > button:hover {
  border-bottom: 2px solid #007BEF;
}
```


```jsx
function TabItem(props) {
  return <div {...props} />;
}
function Tabs(props) {
  const [bindIndex, setBindIndex] = React.useState(props.defaultIndex);
  const changeTab = newIndex => {
    if (typeof props.onTabClick === "function") props.onTabClick(newIndex);
    setBindIndex(newIndex);
  };
  const items = props.children.filter(item => item.type.name === "TabItem");
  return (
    <div className="wrapper">
      <div className="tab-menu">
        {items.map(({ props: { index, label } }) => (
          <button
            onClick={() => changeTab(index)}
            className={bindIndex === index ? "focus" : ""}
          >
            {label}
          </button>
        ))}
      </div>
      <div className="tab-view">
        {items.map(({ props }) => (
          <div
            {...props}
            className="tab-view_item"
            key={props.index}
            style={ { display: bindIndex === props.index ? "block" : "none" }}
          />
        ))}
      </div>
    </div>
  );
}
```


```jsx
ReactDOM.render(
  <Tabs defaultIndex="1" onTabClick={console.log}>
    <TabItem label="A" index="1">
      Lorem ipsum
    </TabItem>
    <TabItem label="B" index="2">
      Dolor sit amet
    </TabItem>
  </Tabs>,
  document.getElementById("root")
);
```
<br>[⬆ Back to top](#table-of-contents)

### Ticker

Renders a ticker component.
Use the `React.useState()` hook to initialize the `ticker` state variable to `0`.
Define two methods, `tick` and `reset`, that will periodically increment `timer` based on `interval` and reset `interval` respectively.
Return a `<div>` with two `<button>` elements, each of which calls `tick` and `reset` respectively.

```jsx
function Ticker(props) {
  const [ticker, setTicker] = React.useState(0);
  let interval = null;
  const tick = () => {
    reset();
    interval = setInterval(() => {
      if (ticker < props.times) 
        setTicker(ticker + 1);
      else 
        clearInterval(interval);
    }, props.interval);
  }
  const reset = () => {
    setTicker(0);
    clearInterval(interval);
  }
  return (
    <div>
      <span style={ { fontSize: 100 }}>{this.state.ticker}</span>
      <button onClick={this.tick}>Tick!</button>
      <button onClick={this.reset}>Reset</button>
    </div>
  );
}
```


```jsx
ReactDOM.render(<Ticker times={5} interval={1000} />, document.getElementById('root'));
```
<br>[⬆ Back to top](#table-of-contents)

### Toggle

Renders a toggle component.
Use the `React.useState()` to initialize the `isToggleOn` state variable to `false`.
Use an object, `style`, to hold the styles for individual components and their states.
Return a `<button>` that alters the component's `isToggledOn` when its `onClick` event is fired and determine the appearance of the content based on `isToggleOn`, applying the appropriate CSS rules from the `style` object.

```jsx
function Toggle(props) {
  const [isToggleOn, setIsToggleOn] = React.useState(false);
  style = {
    on: {
      backgroundColor: "green"
    },
    off: {
      backgroundColor: "grey"
    }
  };
  return (
    <button
      onClick={() => setIsToggleOn(!isToggleOn)}
      style={isToggleOn ? style.on : style.off}
    >
      {isToggleOn ? "ON" : "OFF"}
    </button>
  );
}
```


```jsx
ReactDOM.render(<Toggle />, document.getElementById('root'));
```
<br>[⬆ Back to top](#table-of-contents)

### Tooltip

Renders a tooltip component.
Use the `React.useState()` hook to create the `show` variable and initialize it to `false`.
Return a `<div>` element that contains the `<div>` that will be the tooltip and the `children` passed to the component.
Handle the `onMouseEnter` and `onMouseLeave` methods, by altering the value of the `show` variable.

```css
.tooltip {
  position: relative;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  visibility: hidden;
  padding: 5px;
  border-radius: 5px;
}
.tooltip-arrow {
  position: absolute;
  top: 100%;
  left: 50%;
  border-width: 5px;
  border-style: solid;
  border-color: rgba(0, 0, 0, 0.7) transparent transparent;
}
```


```jsx
function Tooltip({ children, text, ...rest }) {
  const [show, setShow] = React.useState(false);
  return (
    <div>
      <div className="tooltip" style={show ? { visibility: "visible" } : {}}>
        {text}
        <span className="tooltip-arrow" />
      </div>
      <div
        {...rest}
        onMouseEnter={() => setShow(true)}
        onMouseLeave={() => setShow(false)}
      >
        {children}
      </div>
    </div>
  );
}
```


```jsx
 ReactDOM.render(
     <Tooltip text='Simple tooltip'>
       <button>Hover me!</button>
     </Tooltip>,
     document.getElementById('root')
 );
```
<br>[⬆ Back to top](#table-of-contents)
