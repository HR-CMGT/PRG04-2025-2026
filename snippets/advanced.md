# Advanced setup

- build preview
- starting from scratch
    - startcode
- de game sneller laden en builden
- VS code settings

<br>
<br>
<br>

## Build preview

Om snel te testen of het publiceren naar de `docs` map werkt kan je een preview doen. Dit kan je toevoegen aan `package.json`:

```json
"scripts": {
    "preview": "vite preview --outDir=docs --base=./"
},
```

<br>
<br>
<br>



<br><br><br>

## Starting from scratch

Maak een [Vite](https://vitejs.dev) project met `npm create vite@latest`. In de Vite setup kies je voor `vanilla` en `javascript`. Vervolgens open je de projectmap en installeer je excalibur.

```bash
npm create vite@latest mijn-game-project # kies voor vanilla en javascript
cd mijn-game-project
npm install excalibur
npm run dev
```
Je krijgt nu een standaard Vite project. Voeg een `SRC` folder toe. Je kan de `PUBLIC` map en de voorbeeldcode van Vite verwijderen (`counter.js, main.js, `en de `.svg files`)

### Package.json aanpassen

Voeg het build en preview commando toe aan package.json. We voegen hier `docs` toe aan de `outDir` omdat github pages met een `docs` folder werkt. De `base` variabele bepaalt het startpunt van waaruit je project naar bestanden gaat zoeken. 

```json
 "scripts": {
    "dev": "vite",
    "build": "vite build --outDir=docs --base=./",
    "preview": "vite preview --outDir=docs --base=./"
  },
```
<br>
<br>
<br>

## De game sneller laden en builden

Als je `excalibur` apart compileert van je game code, dan gaat het `npm run build` proces veel sneller, en het laden van je game in de browser gaat ook sneller na een update. Dit komt doordat `excalibur` dan in de `cache` kan blijven en alleen je game update opnieuw wordt gecompileerd / geladen.

#### vite.config.js

```js
import { defineConfig } from 'vite';

export default defineConfig({
    build: {
        rollupOptions: {
            output: {
                manualChunks(id) {
                    // If the module is inside node_modules and includes 'excalibur',
                    // bundle it into a chunk named 'vendor-excalibur'
                    if (id.includes('node_modules') && id.includes('excalibur')) {
                        return 'vendor-excalibur';
                    }
                }
            }
        }
    }
});

```

<Br>
<br>
<br>

### Startcode

Je kan onderstaande twee classes toevoegen aan de SRC map. Let op dat je `game.js` inlaadt in `index.html`. `index.html` staat in de root. Verder staan alle werkbestanden in de SRC map.

GAME.JS

```javascript
import { Actor, Engine, Vector } from "excalibur"
import { Resources, ResourceLoader } from './resources.js'

export class Game extends Engine {
    constructor() {
        super({ width: 800, height: 600 })
        this.start(ResourceLoader).then(() => this.startGame())
    }
    startGame(){
        console.log("start de game!")
    }
}

new Game()
```
RESOURCES.JS

Plaats je resources in de `public` folder. Daarbinnen kan je subfolders aanmaken voor images, sounds, fonts, etc.

```javascript
import { ImageSource, Sound, Resource, Loader } from 'excalibur'
const Resources = {
    Fish: new ImageSource('images/fish.png')
}
const ResourceLoader = new Loader([Resources.Fish])
export { Resources, ResourceLoader }
```


<br>
<br>
<br>

## VS Code Tip

Tip: clickable npm commando's in VS Code

Het is super handig om de npm commando's aan te kunnen klikken:

```
Open "File > Preferences > Settings"
Search "npm script"
Toggle "Npm: Enable Script Explorer"
```

<br>
<br>
<br>
