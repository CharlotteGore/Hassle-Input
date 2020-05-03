# Hassle::Input

This is a reference Input Management system. It is designed to make it easy to bind and rebind gamepad, keyboard, mouse and touch controls to named actions. Using actions instead of direct controls allows the developer to decouple from the actual keys used, making it easier to configure and change inputs both in code and at runtime.


## Example Setup

Hassle Input has enums for standard mouse buttons, keys and gamepad buttons. Your responsibility is to define your game's
actions. There are three types of actions. Digital actions are binary, they're either 1 or 0. These are used for keyboard buttons, mouse buttons etc. Analog actions are numbers and are useful for capturing thumbsticks, mouse movement etc. Touch actions are a little strange and give an array of Touch objects which have an 'active' status and the same normalized x/y 
coords as the mouse axes. The number of these is set by the amount the system supports, i.e, 0 for a none touch device, 1 for 
messing around with Mobile mode in Chrome Devtools and more for a real touch device.

In general actions are a list of all the interactions that a player can make with your game. After you have defined your
list of actions, the next step is to make an `InputBindings` object. A valid input bindings object has three main children, `gamepad`, `mouse`,`keys` and a special `touch` category

```ts
import { MouseButtons, MouseActions, Keys, InputBindings } from 'Input/types' 

enum DigitalActions {
    Use,
    Examine,
    WalkLeft,
    WalkRight,
}

enum AnalogActions {
    MouseX,
    MouseY,
    MoveLeft,
    MoveRight,
}

// we don't need any touch actions so we leave this empty
enum TouchActions {
    Touch
}

const bindings: InputBindings<DigitalActions, AnalogActions, TouchActions> = {
    gamepad: {
        axis: {
            [GamepadAxis.LX]: AnalogActions.MoveLeft,
            [GamepadAxis.LY]: AnalogActions.MoveRight,
        },
        buttons
    },
    mouse: {
        axis: {
            [MouseAxis.X]: AnalogActions.MouseX,
            [MouseAxis.Y]: AnalogActions.MouseY,
        },
        buttons: {
            [MouseButtons.Left]: DigitalActions.Interact,
            [MouseButtons.Right]: ButtonActions.Examine,
        }
    },
    keys: {
        axis: null,
        buttons: {
            [Keys.Right]: DigitalActions.WalkRight,
            [Keys.Left]: DigitalActions.WalkLeft,
            [Keys.Space]: DigitalActions.Interact,
            []
        }
    },
    touch: {

    }
}

const inputs = new InputManager<DigitalActions, AnalogActions, TouchActions>();
inputs.bindActions(bindings);
```

In addition if you want to use the mouse cursor or touch events (and want to know when the mouse is over your game) you 
need to pass in a DOM element for binding to.

```ts
inputs.setMouseAndTouchActiveElement(document.getElementById('canvas'));
```

## Integrating

Initalize:

```ts
// initialize the input system
inputs.initialize();
```

Each tick (via setInterval or requestAnimationFrame)

```ts
    // every tick of your game...
    inputs.update();
    // then...
    return inputs.getActionStatus();
```

When you're done..

```ts
inputs.destroy();
```


## Running the test page

This has been mostly set up for running in Electron.

```sh
yarn install
yarn testpage
electron ./TestPage/main.js
```

## NPM Package?

I'm using this as a submodule atm so not a priority, but eventually, if dev continues.