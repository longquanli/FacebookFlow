# Flow
![Flow logo](./media/flow.png)

Hello there! This is a Flow note.

* [Flow](#flow)
	* [Installation](#installation)
	* [Usage](#Usage)
	* [Type Annotations](#type-annotations)
		* [Primitive Types](#primitive-types)
		* [Literal Types](#literal-types)
		* [Mixed Types](#mixed-types)
		* [Any Types](#any-types)
		* [Maybe Types](#maybe-types)
		* [Variable Types](#variable-types)
		* [Function Types](#function-types)
		* [Object Types](#object-types)
		* [Array Types](#array-types)
		* [Class Types](#class-types)
		* [Type Aliases](#type-aliases)
		* [Tuple Types](#tuple-types)
	* [Components](#components)
 		* [Class Components](#class-components)
 		* [Stateless Functional Components](#stateless-functional-components)

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

### Object Types

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

### Tuple Types
<span style="color:red">__Tuple__</span> are a sort of list but with a limited set of items. It is strictly enforced and don't match array types.

~~~js
// @flow
let tuple1: [number, boolean, string] = [1, true, "three"]
~~~

### Interface Types
Classes in Flow are nominally typed. You can not use one with another one's type even when they have the same exact properties and methods.

~~~js
// @flow
class Foo {
	serialize() {return "foo";}
}
class Bar {
	serialize() {return "Bar";}
}

const foo: Foo = new Bar();   //Error
~~~

Use <span style="color:red">__Interface__</span> in order to declear the structure of the class that you are expecting.

~~~js
// @flow
interface People()<A, B, C>{
	write(name: string): string;
	name: string;
	hobby?: string;
	lunch?: { [key: string]: number };
	jump(height: B, way: A): C;
	+readOnly: number;
	-writeOnly: number;
}

class Alice implements People<string, number, string> {
	write(name) {
		return "This is " + name
	}
	name = Alice
	jump(height, way) {
		return `I can jump ${height} in this ${way}`
	}
	People.writeOnly = 3
	value2 = People.readOnly
}
~~~

### Utility Types
#### `$Keys<T>`

~~~js
// @flow
const countries = {
	US: "United States",
	IT: "Italy",
	FR: "France" 
};
type Country = $Keys<typeof countries>

const italy: Country = 'IT';
const nope: Country = 'nope';    //'nope' is not a country.
~~~

#### `$Values<T>`

~~~js
// @flow
type = Props = {
	name: string,
	age: number	
}
//The following two types are equivalent.
type PropValues = string | number;
type Prop$Vales = $Values<Props>

~~~

#### `$ReadOnly<T>`

~~~js
// @flow
type Props = {
	name: string,
	age: number,
}
type ReadOnlyProps = $ReadOnly<Props>
function render(props: ReadOnlyProps) {
	const {name, age} = props;  //Ok
	props.age = 32;             //Error
}
~~~
#### `$Exact<T>`

~~~js
// Same
type ExactUser1 = $Exact<{name: string}>;
type ExactUser2 = {| name: string |};
~~~

#### `$Diff<A, B>`

#### `$PropertyType<T, K>`

~~~js
// @flow
import React, {Component} from 'react'
class Person extends Component {
	props: {
		name: string,
		jump: ({x: number, y: number}) => string
	};
}

const someProps: $PropertyType<Person, 'props'> = {
	name: 'foo',
	jump: (data: {x: 5, y: 23}) => return `foo can jump ${x} 			height and ${y} length. `
}
~~~
#### `$Rest<A, B>`

#### `$ElementType<T, K>`
T could be object, tuple or array

~~~js
// @flow
type Obj = {
	name: string,
	age: number,
}
('Jon': $ElementType<Obj, 'name'>);
(42: $ElementType<Obj, ''age>)
~~~

#### `NonMaybeType<T>`

~~~js
// @flow
type MaybeName = ?string;
type Name = $NonMaybeType<MaybeName>;

(null, MaybeName);    // ok
(null, Name)          // Error
~~~

#### `$ObjMap<T, F>`

#### `$Call<F, T...>`

#### `$Class<T>`

~~~js
// @flow
class Store {}
class SubStore extends Store {}

function makeStore(storeClass: Class<Store>) {
	return new storeClass();
}

(makeStore(Store): Store);
(makeStore(SubStore): Store);
~~~
#### `$Shape<T>`

~~~js
// @type
type Person = $Shape<{
	age: number,
	name: string,
}>
const person1: Person = {age: 28};
~~~


## Components

### Class Components
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