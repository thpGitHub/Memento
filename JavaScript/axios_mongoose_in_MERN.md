# Axios Mongoose in MERN Stack

## Depuis le back

création d'un fichier `database_connect.js` à la base du serveur car la connection sera disponible dès le début et partout !

````javascript
const mongoose = require('mongoose');
require('dotenv').config();

const connection = process.env.DB_HOST_ATLAS;
mongoose.connect(connection,{ useNewUrlParser: true,
                              useUnifiedTopology: true,
                              useFindAndModify: false})
    .then(() => console.log("Database Connected Successfully"))
    .catch(err => console.log(err));
````

Création du schema mongoose `user.js` dans le dossier `models`

````javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const userSchema = new Schema({
    name: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true
    },
});
// userSchema.set('validateBeforeSave', false);
module.exports = mongoose.model("User", userSchema, "userTest")
````

Depuis `server.js`

````javascript
const express = require('express');
const app = express();
const http = require('http').createServer(app);
const path = require('path');
require('./database_connect');

// controllers
const users = require('./controllers/qpudControllerUser');
app.use('/controllers/qpudControllerUser', users);

// Say to Heroku where are the build folder (/client/build)
app.use(express.static(path.join(__dirname, 'client', 'build')));

// app.get("*", (req, res) => {
//     res.sendFile(path.join(__dirname, "client", "build", "index.html"));
// });

const PORT = process.env.PORT || 8001;
http.listen(PORT, () => {
    const date = new Date();
    console.log(`${ date.getHours() }H${ date.getMinutes() } on port : ${ PORT }`);
});
````

Depuis le controlleur `qpudControllerUser.js`

````javascript
const express = require('express');
const router = express.Router()

const User = require('../models/user');

router.get('/', (req, res) => {
    User.find()
        .then(users => res.json(users))
        .catch(err => console.log(err))
})

// router.post('/', (req, res) => {
//     console.log('jsuis dans le router post');
//     console.log('jsuis dans le router post et req body = ', req.body);
//     const { username, email } = req.body; // Attention le state username ne correspond pas au name de la bdd
//     const newUser = new User({
//         name: username, email: email
//     })
//     newUser.save()
//         .then(() => res.json({
//             message: "Created account successfully"
//         }))
//         .catch(err => res.status(400).json({
//             "error": err,
//             "message": "Error creating account"
//         }))
// })
module.exports = router 
````

## Depuis le front

depuis un component `login.js`

````javascript
import React from 'react';
import axios from 'axios';

const Login = () => {
        
    const handleSubmit = (e) => {
        e.preventDefault();     
        axios
          .get("/controllers/qpudControllerUser")
          .then((users) => console.log(users))
          .catch((err) => console.log(err)); 
    }
       return (
        <div>
            {/* <form id="login" action="/verify_pseudo_pwd" method="post"> */}
            <form id="login" onSubmit={ e=> handleSubmit(e) } onKeyUp={ handleKeyUp }>
                <h1>LOGIN</h1>
                <div id="message_login"></div>
                <input type="text"     name="pseudo" id="pseudo" placeholder="Pseudo" autoComplete="off" spellCheck="false" />
                <input type="password" name="pwd"    id="pwd"    placeholder="Password"/>
                <input type="submit"   name="submit" value="Login"/>
                <a href="/registration">Create an Account</a>
            </form>
        </div>
    );
}
export default Login;
````
