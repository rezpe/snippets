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
