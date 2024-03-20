# Cloud Firestore triggers

```jsx

import {
  onDocumentWritten,
  onDocumentCreated,
  onDocumentUpdated,
  onDocumentDeleted,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

// To define a Cloud Firestore trigger, specify a document path and an event type:
exports.myfunction = onDocumentWritten("my-collection/{docId}", (event) => {
   /* ... */ 
});

// Specify a single document
// trigger an event for any change to a specific document then you can use the following function
import {
  onDocumentWritten,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.myfunction = onDocumentWritten("users/marie", (event) => {
  // Your code here
});

```
</br>

## Event Triggers

```jsx

// Trigger a function when a new document is created:

import {
  onDocumentCreated,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.createuser = onDocumentCreated("users/{userId}", (event) => {
    // Get an object representing the document
    // e.g. {'name': 'Marie', 'age': 66}
    const snapshot = event.data;
    if (!snapshot) {
        console.log("No data associated with the event");
        return;
    }
    const data = snapshot.data();

    // access a particular field as you would any JS property
    const name = data.name;

    // perform more operations ...
});

// Trigger a function when a document is updated:

import {
  onDocumentUpdated,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.updateuser = onDocumentUpdated("users/{userId}", (event) => {
    // Get an object representing the document
    // e.g. {'name': 'Marie', 'age': 66}
    const newValue = event.data.after.data();

    // access a particular field as you would any JS property
    const name = newValue.name;

    // perform more operations ...
});

// Trigger a function when a document is deleted

import {
  onDocumentDeleted,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.deleteuser = onDocumentDeleted("users/{userId}", (event) => {
    // Get an object representing the document
    // e.g. {'name': 'Marie', 'age': 66}
    const snap =  event.data;
    const data =  snap.data();

    // perform more operations ...
});

// Trigger a function for all changes to a document

import {
  onDocumentWritten,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.modifyuser = onDocumentWritten("users/{userId}", (event) => {
    // Get an object with the current document values.
    // If the document does not exist, it was deleted
    const document =  event.data.after.data();

    // Get an object with the previous document values
    const previousValues =  event.data.before.data();

    // perform more operations ...
});

// Reading Event Data

exports.updateuser2 = onDocumentUpdated("users/{userId}", (event) => {
    // Get an object with the current document values.
    // If the document does not exist, it was deleted
    const newValues =  event.data.after.data();

    // Get an object with the previous document values
    const previousValues =  event.data.before.data();
});

// Writting Event Data

import { onDocumentUpdated } from "firebase-functions/v2/firestore";

exports.countnamechanges = onDocumentUpdated('users/{userId}', (event) => {
  // Retrieve the current and previous value
  const data = event.data.after.data();
  const previousData = event.data.before.data();

  // We'll only update if the name has changed.
  // This is crucial to prevent infinite loops.
  if (data.name == previousData.name) {
    return null;
  }

  // Retrieve the current count of name changes
  let count = data.name_change_count;
  if (!count) {
    count = 0;
  }

  // Then return a promise of a set operation to update the count
  return data.after.ref.set({
    name_change_count: count + 1
  }, {merge: true});

});

// Data outside the trigger event

const { initializeApp } = require('firebase-admin/app');
const { getFirestore, Timestamp, FieldValue } = require('firebase-admin/firestore');

initializeApp();
const db = getFirestore();

exports.writetofirestore = onDocumentWritten("some/doc", (event) => {
    db.doc('some/otherdoc').set({ ... });
  });

  exports.writetofirestore = onDocumentWritten('users/{userId}', (event) => {
    db.doc('some/otherdoc').set({
      // Update otherdoc
    });
  });

```

- **onDocumentCreated**	Triggered when a document is written to for the first time.
- **onDocumentUpdated**	Triggered when a document already exists and has any value changed.
- **onDocumentDeleted**	Triggered when a document is deleted.
- **onDocumentWritten**	Triggered when onDocumentCreated, onDocumentUpdated or onDocumentDeleted is triggered.

</br>

# Cloud Storage triggers 

```jsx

// To get started, import the module required for handling Cloud Storage events:
 const {
    onObjectFinalized,
    onObjectDeleted,
    onObjectArchived,
    onMetadataUpdated
  } = require("firebase-functions/v2/storage");

// also add the dependencies for the Firebase Admin SDK and image processing tools:
const {initializeApp} = require("firebase-admin/app");
const {getStorage} = require("firebase-admin/storage");
const logger = require("firebase-functions/logger");
const path = require("path");

// library for image resizing
const sharp = require("sharp");

initializeApp();

// Access Cloud Storage object attributes
const fileBucket = event.data.bucket; // Storage bucket containing the file.
const filePath = event.data.name; // File path in the bucket.
const contentType = event.data.contentType; // File content type.

```
## Download, transform, and upload a file

```jsx

/**
 * When an image is uploaded in the Storage bucket,
 * generate a thumbnail automatically using sharp.
 */
exports.generateThumbnail = onObjectFinalized({cpu: 2}, async (event) => {

  const fileBucket = event.data.bucket; // Storage bucket containing the file.
  const filePath = event.data.name; // File path in the bucket.
  const contentType = event.data.contentType; // File content type.

  // Exit if this is triggered on a file that is not an image.
  if (!contentType.startsWith("image/")) {
    return logger.log("This is not an image.");
  }
  // Exit if the image is already a thumbnail.
  const fileName = path.basename(filePath);
  if (fileName.startsWith("thumb_")) {
    return logger.log("Already a Thumbnail.");
  }

  // Download file into memory from bucket.
  const bucket = getStorage().bucket(fileBucket);
  const downloadResponse = await bucket.file(filePath).download();
  const imageBuffer = downloadResponse[0];
  logger.log("Image downloaded!");

  // Generate a thumbnail using sharp.
  const thumbnailBuffer = await sharp(imageBuffer).resize({
    width: 200,
    height: 200,
    withoutEnlargement: true,
  }).toBuffer();
  logger.log("Thumbnail created");

  // Prefix 'thumb_' to file name.
  const thumbFileName = `thumb_${fileName}`;
  const thumbFilePath = path.join(path.dirname(filePath), thumbFileName);

  // Upload the thumbnail.
  const metadata = {contentType: contentType};
  await bucket.file(thumbFilePath).save(thumbnailBuffer, {
    metadata: metadata,
  });
  return logger.log("Thumbnail uploaded!");
});

```

- **onObjectArchived** Only sent when a bucket has enabled object versioning. This event indicates that the live version of an object has become an archived version, either because it was archived or because it was overwritten by the upload of an object of the same name.
- **onObjectDeleted** Sent when an object has been permanently deleted. This includes objects that are overwritten or are deleted as part of the bucket's lifecycle configuration. For buckets with object versioning enabled, this is not sent when an object is archived (see onArchive), even if archival occurs via the storage.objects.delete method.
- **onObjectFinalized** Sent when a new object (or a new generation of an existing object) is successfully created in the bucket. This includes copying or rewriting an existing object. A failed upload does not trigger this event.
- **onMetadataUpdated** Sent when the metadata of an existing object changes.





