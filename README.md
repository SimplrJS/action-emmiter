# action-emitter
Action emitter based on [fbemitter](facebook/emitter). Instead of string event types we use classes (functions).
The package is most useful when used with [TypeScript](http://typescriptlang.org).

[![Build Status](https://travis-ci.org/SimplrJS/action-emitter.svg?branch=master)](https://travis-ci.org/SimplrJS/action-emitter)
[![NPM version](http://img.shields.io/npm/v/action-emitter.svg)](https://www.npmjs.com/package/action-emitter) [![dependencies Status](https://david-dm.org/simplrjs/action-emitter/status.svg)](https://david-dm.org/simplrjs/action-emitter) [![devDependencies Status](https://david-dm.org/simplrjs/action-emitter/dev-status.svg)](https://david-dm.org/simplrjs/action-emitter?type=dev)


# Get started
```cmd
$ npm install action-emitter --save-dev
```

# Usage
First import the `action-emitter` package and then create a new emitter instance.
```ts
import { ActionEmitter } from "action-emitter";
let emitter = new ActionEmitter();
```


# API

## `constructor(): void`
Create a new emitter instance.

### Example
```ts
let emitter = new ActionEmitter();
```

## `addListener(actionClass, callback): EventSubscription`
Register a specific callback to be called on a particular action event. A token is returned that can be used to remove the listener.

### Arguments
| Argument      | Type                        | Description                 |
|---------------|-----------------------------|-----------------------------|
| `actionClass` | `Function`                  | Action class function.      |
| `callback`    | `(action: TAction) => void` | Listener callback function. |


### Example
```ts
class Action {
    constructor(private value: string) { }
    public get Value() {
        return this.value;
    }
}

let token = emitter.addListener<Action>(Action, (action) => {
    console.log(action.Value);
});
```


## `once(actionClass, callback): EventSubscription`
Similar to `addListener()` but the callback is removed after it is invoked once. Similar to `addListener()` but the callback is removed after it is invoked once.

### Arguments
| Argument      | Type                        | Description                 |
|---------------|-----------------------------|-----------------------------|
| `actionClass` | `Function`                  | Action class function.      |
| `callback`    | `(action: TAction) => void` | Listener callback function. |

### Example
```ts
class Action {
    constructor(private value: string) { }
    public get Value() {
        return this.value;
    }
}

let token = emitter.once<Action>(Action, (action) => {
    console.log(action.Value);
});
```


## `removeAllListeners(actionClass): void`
Removes all of the registered listeners. If provide `actionClass`, only listeners for that action class are removed.

### Arguments
| Argument                    | Type                        | Description                 |
|-----------------------------|-----------------------------|-----------------------------|
| `actionClass`<sup>[*]</sup> | `Function`                  | Action class function.      |
<sup>[*]</sup> - not required.

### Example
```ts
class Action {
    constructor(private value: string) { }
    public get Value() {
        return this.value;
    }
}

emitter.removeAllListeners(Action);
// Or
emitter.removeAllListeners();
```


## `listeners(actionClass): Function[]`
Return an array of listeners that are currently registered for the given action class.

### Arguments
| Argument      | Type                        | Description                 |
|---------------|-----------------------------|-----------------------------|
| `actionClass` | `Function`                  | Action class function.      |


### Example
```ts
class Action {
    constructor(private value: string) { }
    public get Value() {
        return this.value;
    }
}

emitter.removeAllListeners(Action);
// Or
emitter.removeAllListeners();
```


## `emit(action): void`
Emits an action event with the given data. All callbacks that are listening to the particular action event will be notified.

### Arguments
| Argument      | Type      | Description                 |
|---------------|-----------|-----------------------------|
| `action`      | `TAction` | Action class instance.      |


### Example
```ts
class Action {
    constructor(private value: string) { }
    public get Value() {
        return this.value;
    }
}

emitter.emit<Test>(new Test("value"));
//or 
emitter.emit(new Test("value"));
```

# License
Released under the [MIT license](LICENSE).
