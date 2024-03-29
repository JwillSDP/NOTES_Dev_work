# Import the required modules and initialize an app

## Deploy functions to a production environment
```bash
firebase emulators:start
```
```bash
firebase emulators:start --only functions
```
```bash
firebase init functions
```
```bash
npm install -g firebase-tools
```

<br />

## Initialize your project


```jsx
// The Cloud Functions for Firebase SDK to create Cloud Functions and triggers.
const {logger} = require("firebase-functions");
const {onRequest} = require("firebase-functions/v2/https");
const {onDocumentCreated} = require("firebase-functions/v2/firestore");

// The Firebase Admin SDK to access Firestore.
const {initializeApp} = require("firebase-admin/app");
const {getFirestore} = require("firebase-admin/firestore");

initializeApp();
```

<br />

# Add the "add message" function

```jsx
// Take the text parameter passed to this HTTP endpoint and insert it into
// Firestore under the path /messages/:documentId/original
exports.addmessage = onRequest(async (req, res) => {
  // Grab the text parameter.
  const original = req.query.text;
  // Push the new message into Firestore using the Firebase Admin SDK.
  const writeResult = await getFirestore()
      .collection("messages")
      .add({original: original});
  // Send back a message that we've successfully written the message
  res.json({result: `Message with ID: ${writeResult.id} added.`});
});
```

<br />

# Call functions from your app

```jsx
// Dependencies for callable functions.
const {onCall, HttpsError} = require("firebase-functions/v2/https");
const {logger} = require("firebase-functions/v2");


exports.addmessage = onCall((request) => {
// Message text passed from the client.
const text = request.data;
// Authentication / user information is automatically added to the request.
const uid = request.auth.uid;
const name = request.auth.token.name || null;
const picture = request.auth.token.picture || null;
const email = request.auth.token.email || null;
});
```




