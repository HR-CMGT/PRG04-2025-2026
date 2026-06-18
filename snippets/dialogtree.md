# Dialog Tree

Het maken van een Dialog Tree kan je doen met een JSON file waar al je gesprekken in staan. Met javascript kan je vervolgens de juiste dialog opzoeken, en de vraag met antwoorden tonen.

## JSON

```json
[
    {
        "id":"start-conversation",
        "question":"Hi, I'm Danny the Troll. How are you today?",
        "answers":[
            {
                "text":"I'm fine, how are you?",
                "link":"nice-conversation"
            },{
                "text":"Arrrhh, a troll!",
                "link":"nasty-conversation"
            }
        ]
    },{
        "id":"nice-conversation",
        "question":"Oh I'm fine, how can I help you?",
        "answers":[
            {
                "text":"Tell me the way to the troll cave",
                "link":"cave-conversation"
            },{
                "text":"Do you have some spare food?",
                "link":"food-conversation"
            }
        ]
    },{
        "id":"nasty-conversation",
        "question":"So what! Trolls are people too you know!",
        "answers":[
            {
                "text":"Sorry I didn't know that",
                "link":"nice-conversation"
            },{
                "text":"Prepare for a fight you nasty troll!",
                "link":"fight-conversation"
            }
        ]
    }
]
```

## Javascript

In `vite` kan je een JSON rechtstreeks importeren zonder `fetch`.

```js
import dialogData from "./dialogs.json"

function showQuestion(id) {
    // find the dialog for this id
    const dialog = dialogData.find(d => d.id === id)
    // show the question
    console.log(dialog.question)
    // show the answers
    for(let answer of dialog.answers) {
        console.log(` - ${answer.text}`)
        console.log(`    (if chosen, go to ${answer.link})`)
    }
}

showQuestion("start-conversation")
```