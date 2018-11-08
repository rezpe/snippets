# Overview

This page covers how to create cloud functions with Firebase Cloud Functions

## Basic Cloud Function

```javascript
const functions = require('firebase-functions');

// // Create and Deploy Your First Cloud Functions
// // https://firebase.google.com/docs/functions/write-firebase-functions
//
// exports.helloWorld = functions.https.onRequest((request, response) => {
//  response.send("Hello from Firebase!");
// });

exports.getInfo = functions.https.onRequest((req, res) => {
    res.set('Access-Control-Allow-Origin', '*');
    res.status(200).send(`Hello ${req.body.name || 'World'}!`);
  });
```

## Sqlite Database Access

The file has to be deployed with the index.js file

```javascript
const functions = require('firebase-functions');
const sqlite3 = require('sqlite3');

//https://cloud.google.com/functions/docs/concepts/exec#functions-concepts-after-timeout-nodejs
exports.getInfo = functions.https.onRequest((req, res) => {
    res.set('Access-Control-Allow-Origin', '*');
    const db = new sqlite3.Database(__dirname+"/data.db");
    let query = req.body.query || "SELECT COUNT(*) FROM mush group by cap-shape "
    db.all(query, function(err, rows) {
        res.status(200).send(JSON.stringify(rows));
    });
    db.close()
  });
```
