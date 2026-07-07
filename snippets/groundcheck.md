# Ground Check voor Platform Game

In een platform game wil je meestal alleen kunnen springen als je character op een ondergrond staat.
Je kan dit op meerdere manieren oplossen. Het is belangrijk dat je niet elk frame (60 keer per seconde) een groundcheck doet, dat is slecht voor de performance.

## Collision Events

Via Collision Start / End kan je bijhouden of je karakter tegen de grond aankomt of de grond heeft verlaten. Als je grond uit meerdere platformen bestaat heb je een array (of Set) nodig.

```js
export class Player extends Actor {
    constructor(scoreUI, livesUI) {
        super({ width: 100, height: 100, collisionType: CollisionType.Active })
        this.isGrounded = false
        this.groundContacts = new Set()
    }

    onInitialize(engine) {
        this.on("collisionstart", (event) => this.hitSomething(event))
        this.on("collisionend", (event) => this.leftSomething(event))
    }

    onPreUpdate(engine, delta) {
        this.vel.x = 0
        if (engine.input.keyboard.isHeld(Keys.A)) { this.vel.x = -250   }
        if (engine.input.keyboard.isHeld(Keys.D)) { this.vel.x = 250  }
        if (engine.input.keyboard.wasPressed(Keys.Space) && this.isGrounded) {
            this.vel.y = -1000
        }
    }

    onCollisionStart(event, other) {
        if (other.owner instanceof Platform) {
            this.groundContacts.add(other)
            this.isGrounded = this.groundContacts.size > 0
        }
    }

    onCollisionEnd(event, other) {
        if (other.owner instanceof Platform) {
            this.groundContacts.delete(other)
            this.isGrounded = this.groundContacts.size > 0
        }
    }
}
```


## Ground Sensor

Als je events nog steeds onvoorspelbare resultaten geven kan je een extra `groundsensor` actor *onder* je main character toevoegen met `addChild`. Deze actor heeft geen physics zodat deze kan overlappen met de grond.
Alleen op het moment dat je wil springen kijk je of de `groundsensor` een overlap heeft met de grond. 

```js
class GroundSensor extends Actor {
    constructor() {
        super({ width: 36, height: 8, collisionType: CollisionType.Passive })
        this.pos = new Vector(0, 185)
    }

    isTouchingGround(engine) {
        const bounds = this.collider.bounds
        for (let actor of engine.currentScene.actors) {
            if (actor instanceof Platform && bounds.overlaps(actor.collider.bounds)) {
                return true
            }
        }
        return false
    }
}

export class Player extends Actor {
    constructor() {
        super({ width: 100, height: 100, collisionType: CollisionType.Active })
        this.groundSensor = new GroundSensor()
    }

    onInitialize() {
        this.addChild(this.groundSensor)
    }

    onPreUpdate(engine) {
        if (engine.input.keyboard.wasPressed(Keys.Space) && this.groundSensor.isTouchingGround(engine)) {
            this.vel.y = -1000
        }
    }
}
```

<br><br><br>
