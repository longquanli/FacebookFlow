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

