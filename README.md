# Simple library of user-input prompts

This module contains several convenience methods for interacting with the user via the command line.

## Examples

Each method returns a Promise that resolves to the user's input. The `prompt` and `whisper` methods are identical, except that the latter hides the user's input by converting each character to asterisks (for passwords and such). The `yn` method prompts the user for a binary choice and then resolves to a boolean.

```node
const { prompt, whisper, yn } = require('@dawaltconley/cue')

const getLogin = async () => {
    let info = {}
    info.username = await prompt('Username:')
    info.password = await whisper('Password:')
    if (await yn('Display login info?')) {
        console.log(info)
    }
}
```

The `prompt` and `whisper` methods can also take an object or array of prompts as their argument, which will run in the order provided and return an object or array of answers.

```node
const getInfo = () => prompt([
    'Name:',
    'Age:',
    'Height:'
]).then(([name, age, height]) => {
    console.log(`${name} is ${age} years old and ${height} tall.`)
})

const setPassword = () => whisper({
    password: 'Password:',
    confirm: 'Confirm password:'
}).then(({ password, confirm }) => {
    if (password === confirm) {
        console.log('Passwords match.')
    } else {
        console.log('Passwords don\'t match, please try again.')
        setPassword()
    }
})
```
