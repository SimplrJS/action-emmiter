# action-emitter
Action emitter based on [fbemitter](https://github.com/facebook/emitter). Instead of string event types we use classes (functions).
The package is most useful when used with [TypeScript](http://typescriptlang.org).

[![Build Status](https://travis-ci.org/SimplrJS/action-emitter.svg?branch=master)](https://travis-ci.org/SimplrJS/action-emitter)
[![NPM version](http://img.shields.io/npm/v/action-emitter.svg)](https://www.npmjs.com/package/action-emitter) [![dependencies Status](https://david-dm.org/simplrjs/action-emitter/status.svg)](https://david-dm.org/simplrjs/action-emitter) [![devDependencies Status](https://david-dm.org/simplrjs/action-emitter/dev-status.svg)](https://david-dm.org/simplrjs/action-emitter?type=dev)


## Get started
```cmd
$ npm install action-emitter --save-dev
```

## Usage
First import the `action-emitter` package and then create a new emitter instance.
```ts
import { ActionEmitter } from "action-emitter";
const Emitter = new ActionEmitter();
```


## API

### `constructor(): void`
Create a new emitter instance.

#### Emitter construction example:
```ts
const Emitter = new ActionEmitter();
```

### `addListener(actionClass, callback): EventSubscription`
Register a specific callback to be called on a particular action event. A subscription is returned that can be called to remove the listener.

#### Arguments
| Argument      | Type                        | Description                 |
|---------------|-----------------------------|-----------------------------|
| `actionClass` | `Function`                  | Action class function.      |
| `callback`    | `(action: TAction) => void` | Listener callback function. |


#### Add listener example:
```ts
class MyAction {
    constructor(private value: string) { }
    public get Value() {
        return this.value;
    }
}

let subsciption = Emitter.addListener<MyAction>(MyAction, action => {
    console.log(action.Value);
});
```


### `once(actionClass, callback): EventSubscription`
Similar to `addListener()` but the callback is removed after it is invoked once. A subscription is returned that can be called to remove the listener.

#### Arguments
| Argument      | Type                        | Description                 |
|---------------|-----------------------------|-----------------------------|
| `actionClass` | `Function`                  | Action class function.      |
| `callback`    | `(action: TAction) => void` | Listener callback function. |

#### Add once listener example:
```ts
class MyAction {
    constructor(private value: string) { }
    public get Value() {
        return this.value;
    }
}

let subsciption = Emitter.once<MyAction>(MyAction, action => {
    console.log(action.Value);
});
```


### `removeAllListeners(actionClass): void`
Removes all of the registered listeners. If provide `actionClass`, only listeners for that action class are removed.

#### Arguments
| Argument                    | Type        | Description                 |
|-----------------------------|-------------|-----------------------------|
| `actionClass`<sup>[*]</sup> | `Function`  | Action class function.      |

<sup>[*]</sup> - optional.

#### Remove all listeners example:
```ts
class MyAction {
    constructor(private value: string) { }
    public get Value() {
        return this.value;
    }
}

Emitter.removeAllListeners(MyAction);
// Or
Emitter.removeAllListeners();
```


### `listeners(actionClass): Function[]`
Returns an array of listeners that are currently registered for the given action class.

#### Arguments
| Argument      | Type       | Description                 |
|---------------|------------|-----------------------------|
| `actionClass` | `Function` | Action class function.      |


#### Listeners list example:
```ts
let listenersList = Emitter.listeners();
```


### `listenersCount(actionClass): number`
Return listeners count that are currently registered for the given action class. 
If action class is not specified, method will return all registered action listeners count.

#### Arguments
| Argument                    | Type       | Description                 |
|-----------------------------|------------|-----------------------------|
| `actionClass`<sup>[*]</sup> | `Function` | Action class function.      |

<sup>[*]</sup> - optional.

#### Listeners list example:
```ts
class MyAction {
    constructor(private value: string) { }
    public get Value() {
        return this.value;
    }
}

let globalListenersCount = Emitter.listenersCount();
// or 
let listenersCount = Emitter.listenersCount(MyAction);
```


### `emit(action): void`
Emits an action event with the given data. All callbacks that are listening to the particular action event will be notified.

#### Arguments
| Argument      | Type      | Description                 |
|---------------|-----------|-----------------------------|
| `action`      | `TAction` | Action class instance.      |


#### Action emit example:
```ts
class MyAction {
    constructor(private value: string) { }
    public get Value() {
        return this.value;
    }
}

Emitter.emit<MyAction>(new MyAction("value"));
//or 
Emitter.emit(new MyAction("value"));
```

## Debuging
You can listen to all actions with `AnyAction` class.
```ts
import { AnyAction } from "action-emitter";
Emitter.addListener(AnyAction, anyAction => {
    let actionInstance = anyAction.Action;
    console.log(actionInstance);
});
```


## License
Released under the [MIT license](LICENSE).
