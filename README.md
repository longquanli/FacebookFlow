# Flow
![Flow logo](./media/flow.png)

Hello there! This is a Flow note.

[toc]

___

## Installation

__1.Yarn and Babel__:
>First install babel-cli and babel-preset-flow:

~~~
yarn add --dev babel-cli babel-preset-flow
~~~
>Creat .babelrc file at the root of your project:

~~~json
{
	"presets": ["flow"]
}
~~~
>Add a devDependency on the flow-bin npm package:

~~~
yarn add --dev flow-bin
~~~
>Initialize Flow:

~~~
yarn run flow init
~~~
>Run Flow:

~~~
yarn run flow
~~~
__Reference link:__ [https://flow.org/en/docs/install/](https://flow.org/en/docs/install/)

## Usage

__1. Initialize Your Project:__

~~~
flow init
~~~
__2. Run the Flow Background Process:__

~~~
flow status
~~~
__3. Stop:__

~~~
flow stop
~~~

## Type Annotations

### Primitive Types

* Booleans: true, false, !!x, Boolean(x)
* Strings: "hello", "foo" + 42, "foo" + String({}), "foo" + [].toString(), "" + JSON.stringify({})
* Numbers: 43, NaN, Infinity
* null: null
* undefined: void

__The primitive types appear in two ways__

1 Literal values

~~~js
// @flow
function method(x: number, y: string, z: boolean) {
	//...
};

method(3.14, "hello", true);
~~~
2 Wrapper objects

~~~js
// @flow
function method(x: Number, y: String, z: Boolean) {
	//...
};

method(new Number(42), new String("world"), new Boolean(true));
~~~

__Maybe types: null, void, type__

~~~js
// @flow
function helper(value: ?string) {
	//...
}

helper("bar");               //Works
helper();                    //Works
helper(null);                //Works
helper(undefined)            //Works
~~~
__Optional object properties: type, void__

~~~js
// @flow
function helper(value: {foo?: string}) {
	//...
}

helper({foo: "bar"});        //Works
helper({foo: undefined});    //Works
helper({foo: null});         //Error
helper({});                  //Works
~~~
__Optional function parameters: type, void__

~~~js
// @flow
function helper(value?: string}) {
	//...
}

helper("bar");             //Works
helper(undefined);         //Works
helper(null);              //Error
helper();                  //Works
~~~

__Function parameters with defaults: type, void__

~~~js
// @flow
function helper(value: string="foo"}) {
	//...
}

helper("bar");              //Works
helper(undefined);          //Works
helper(null);               //Error
helper();                   //Works
~~~

### Literal Types
* Booleans
* Numbers
* Strings

~~~js
// @flow
function getColor(name: "success" | "warning" | "danger") {
	switch(name) {
		case "success": return "green";
		case "warning": return "yellow";
		case "danger" : return "red";
	}
}

getColor("success");     //Works
getColor("danger");      //Works
getColor("error");       //Error
~~~

### Mixed Types
<span style="color:red">__mixed__</span> will accept any type of value. String, numbers, objects, functions.

~~~js
// @flow
function helper(value: mixed) {
	return "" + value;   //Error
}

helper("foo")      //Error
~~~

### Any Types
<span style="color:red">__any__</span> is the way to avoid using the type checker.

~~~js
// @flow
function add(one: any, two: any): number {
	return one + two;
}

add(1, 2)       //Works
add("1", "2").  //Works
add({}, [])     //Works
~~~

### Maybe Types

~~~js
// @flow
function helper(value: ?number) {
	if (typeof value === 'number') {
		return value * 2
	}
}
~~~

### Variable Types

<span style="color:red">*</span> __If statements, functions and other conditionally run code can all prevent Flow from being able to figure out precisely what a type will be.__

~~~js
// @flow
let foo = 42

function mutate() {
	foo = true
	foo = "hello";
}

mutate();
let isString: string = foo;    //Error
~~~

### Function Types
__Function Declarations__

~~~js
function method(str: string, bool?: boolean, ...nums: Array<number>): void {
	//...
}
~~~
__Arrow Functions__

~~~js
let method = (str: string, bool?: boolean, ...nums: Array<number>): void => {
	//...
}
~~~
__Async functions__

~~~js
// @flow
async function method(): Promise<number> {
	return 123;
}
~~~

__Object Types__

~~~js
// @flow
function helper(value: {foo?: string}) {
	//...
}

helper({foo: "bar"});          //Works
helper({foo: undefined});      //Works
helper({foo: null});           //Error
helper({});                    //Works
~~~

__Sealed Objects__

~~~js
// @flow
var obj = {
	foo: 1,
	bar: true,
	baz: "three",
};

var foo: number = obj.foo;      //Works
var bar: boolean = obj.bar;     //Works
var baz: null = obj.baz;        //Error
obj.bat = 'four'                //Error
~~~

__Exact Object Types__

~~~js
// @flow
var foo: {| foo: string |} = {foo: "Hello", bar: "World" };    //Error
~~~

### Array Types

~~~js
// @flow
let arr1: Array<boolean> = [true, false, true]
let arr2: Array<mixed> = [1, [], "three"]
let arr3: number[] = [0, 1, 2, 3]
let arr4: ?number[] = [1, 2]
let arr5: ?number[] = null
let arr6: (?number)[] = [null]
~~~
__ReadOnlyArray__

~~~js
// @flow
const arr: $ReadOnlyArray<number> = [1, 2, 3]
const first = arr[0]       //Works
arr[1] = 20                //Error
arr.push(4)                //Error
~~~
### Class Types
__Class Methods__

~~~js
class MyClass {
	method(value: string): number {
		//...
	}
}
~~~
__Class Fields (Properties)__

1 Fields inside

~~~js
// @flow
class MyClass {
	prop: number;
	method() {
		this.prop = 42
	}
}
~~~

2 Fields outside

~~~js
// @flow
function MyClass {
	static constant: number;
	static helper: (number) => number
}

MyClass.helper = (x: number): number => {
	return x + 1
}
MyClass.constant = 52
~~~

__Class Generics__

~~~js
class MyClass<A, B, C> {
	property: A;
	method(val: B): c {
		//...
	}
}

var val: MyClass<number, boolean, string> = new MyClass(1, true, 'three')
~~~

### Type Aliases
<span style="color:red"}>Type alias</span> can used for complicated types reuse purpose.

~~~js
type MyObject = {
	foo: number,
	bar: boolean,
	baz: string
}
var val: MyObject = {}
function method(val: MyObject){
	//...
}
class Foo {
	constuctor(val: MyObject) {
		//...
	}
}
~~~

## Comonents
### Class Comonents
__Adding Props and State__

~~~js
// @flow
import React, {Component} from 'react';
type Props = {
	foo: number,
	bar?: string,
}
type State = {
	count: number,
}

class MyComponent extends Component<Props, State> {
	state = {
		count: 0,
	}
	componentDidMount() {
		setInterval(() => {
			this.setState(prevState => ({
			count: prevState.count + 1,
			}))
		}, 1000)
	}
	render() {
		this.props.doesNotExist;    //Error
		return <div>{this.props.bar} has {this.state.count} numbers.</div>
	}
}

<MyComponent foo={32} />
~~~
__Default Props__

~~~js
import React, {Component} from 'react';

type Props = {
	foo: number,
}

class MyComponent extends React.Component<Props> {
	static defaultProps = {
		foo: 52
	};
}

<MyComponent />
~~~

### Stateless Functional Components
__Normal__

~~~js
import React from 'react';
type Props = {
	foo: number,
	bar?: string,
}
function MyComponent(props: Props) {
	props.doesNoteExist;     //Error
	return <div>{this.props.bar}</div>;
}

<MyComponent foo={42} />
~~~
__Default Props__

~~~js
import React from 'react';

type Props = {
	foo: number,
}

function MyComponent(props: Props) {
	//...
}

MyComponent.defaultProps = {
	foo: 42,
}

<MyComponent />
~~~
<span style="color: red">*</span> You don't need to make foo nullable in your Props type. Flow will make sure that foo is optional if you have a default prop for foo.