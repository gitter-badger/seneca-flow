# seneca-flow
======================================================

> Why write code when you can write messages

The plugin enables actions to contain actions with can be used to format properties for the action.   

* Control flow
* Output transforms using lodash
* Templating


## Features



All messages can have the following properties:

* if$: string with condition to be evaluated in order to run the actions
* until$: string with condition to be evaluated in order to continue
* wait$: milliseconds to wait before retry action to check until$
* pause$: milliseconds to pause after action has completed
* $$'anyproperty': object each property is a action which is preloaded to use in templates
* $'anyproperty': object will execute message and set action property without $.  Nested properties must prefix $ for each parent.
* out$: object/array of actions which run using Waterfall. Fixed value which can use template functions.
* key$: string for the key to store the message data. Used in sequences
* exit$: string condition for exit. Used in sequences

### Templates

Each of the actions property values can contain the following:

* $: using only the $ will assign the input data of the same property
  * input { x:10 } action { x:'$' } set action { x:10 }
* $\*: assigns all input data
  * input { x:10 } action { x:'$\*' } set action { x : { x:10 } }
* <%=  %>: lodash templating
  * input { x:10, y:20, msg:'hello' } action { alert:'<%= $.msg %> x = <%= $.x %> y = <%= $.y %>' } set action { alert : 'hello x = 10 y = 20' }
* If none of the above but contains $ the expression will be evaluated
  * input { x:10 } action { y:'$.x', z:'$.x * $.x' } set action { y : 10, z : 100 }

Input data is created and assigned to $ from $$'anyproperty' and data from the previous action in sequences.

### Sequence

Properties:
* sequence: (required) array of actions
* merge: (optional) boolean Merge result data when set via key$ otherwise overwrite
* data: (optional) object to be passed to each action for templating
* extend: (optional) object to extend each action
* in: (optional) set in data for start of sequence
* exit: (optional) string condition for exit. Run after each action and can use :
  * '$' which will be populated with results set via key$ and 'in' will be set to the previous action result
  * 'out' which will contain the result of the action


### Iterate

Properties:
* iterate: (required) object action to use to iterate
* with: (with or times required) array of objects to to iterator as "in" property
* times: (with or times required) integer number of times to iterate
* exit: (optional) string condition for exit. Run after each action and can use :
  * '$' - 'in' will be set to the current 'with' data. 'index' current index
  * 'out' which will contain the result of the action
* series: (optional) boolean force run in series


### Parallel

Properties:
* parallel: (required) array actions to execute
* merge: (optional) boolean merge results of actions


### Waterfall

Properties:
* waterfall: (required) array actions to execute. Actions must accept 'in' property
* in: (optional) data to pass to first item


### Each

Properties:
* each: (required) object action to run for each
* in: (required) array


### _ : Lodash

This is a waterfall compatible function to transform action results via the out$ property

Properties:
* \_: (required) string lodash function to call
* args: (required) arguments for function
* in: (required) data to pass to first item
* select: (optional) property to operate on




Current Version: 0.0.1

Tested on: Node 6.2.1

[![Build Status](https://travis-ci.org/georgigriffiths/seneca-flow.png?branch=master)](https://travis-ci.org/georgigriffiths/seneca-flow)


# Install

```sh
npm install seneca-flow
```

## Testing

```sh
npm test
```


## Releases

   * 0.0.1: initial
