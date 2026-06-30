# Gamepad besturing

Je kan de [Excalibur Gamepad](https://excaliburjs.com/docs/gamepad) gebruiken om gebruikersinput te lezen. Dit werkt zowel voor PS4 / XBox controllers als voor de arcade kast / joysticks.

<br><br><br>

### Gamepad onthouden

De gamepad vuurt een `connect` event af zodra je op een button drukt. Op dat moment kan je de gamepad opslaan. 

```javascript
export class Game extends Engine {

    mygamepad

    constructor() {
        super()
        this.start(ResourceLoader).then(() => this.startGame())
    }

    startGame(){
        this.input.gamepads.enabled = true
    }
}
```
Nu kan je in de player de sticks en buttons uitlezen:

```javascript
export class Player extends Actor {

    onPreUpdate(engine) {
        if (engine.input.gamepads.at(0).wasButtonPressed(Buttons.Face1)) {
            console.log("Gamepad 0, button 1 pressed");
        }
        if (engine.input.gamepads.at(1).wasButtonPressed(Buttons.Face1)) {
            console.log("Gamepad 1, button 1 pressed");
        }

        // bewegen
        const xValue = engine.input.gamepads.at(0).getAxes(Axes.LeftStickX)
        const yValue = engine.input.gamepads.at(0).getAxes(Axes.LeftStickY)

        console.log(`X: ${xValue} Y: ${yValue}`)

        let speed = 100
        this.vel = new Vector(xValue * speed, yValue * speed)
        
    }
}
```


<br><br><br>


## 🎮 🎮 🎮 🎮 Local multiplayer

Je kan aan een player class meegeven welke controller er bij hoort! Let op dat je wel controleert of die speler uberhaupt bestaat. Voor "drop-in" multiplayer kan je het `connect` event gebruiken of elk frame checken of player 2 de controller gebruikt.

```javascript
export class Game extends Engine {
    startGame(){
        this.input.gamepads.enabled = true

        let gamepadOne = engine.input.gamepads.at(0)
        this.add(new Player(gamepadOne)

        let gamepadTwo = engine.input.gamepads.at(1)
        this.add(new Player(gamepadTwo)
    }
}
```
```javascript
export class Player extends Actor {

    mygamepad

    constructor(gamepad){
        this.mygamepad = gamepad
    }

    onPreUpdate(engine) {
        const x = this.mygamepad.getAxes(Axes.LeftStickX)
        const y = this.mygamepad.getAxes(Axes.LeftStickY)
        this.vel = new Vector(x * 10, y * 10)

        if (this.mygamepad.isButtonPressed(Buttons.Face1)) {
            console.log('Jump!')
        }
    }
}
```

<br><br><br>

