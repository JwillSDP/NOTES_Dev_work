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

exports.myfunction = onDocumentWritten("my-collection/{docId}", (event) => {
   /* ... */ 
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
 const {onObjectFinalized} = require("firebase-functions/v2/storage");

// also add the dependencies for the Firebase Admin SDK and image processing tools:
 const {initializeApp} = require("firebase-admin/app");
const {getStorage} = require("firebase-admin/storage");
const logger = require("firebase-functions/logger");
const path = require("path");

// library for image resizing
const sharp = require("sharp");

initializeApp();

```

- **onObjectArchived** Only sent when a bucket has enabled object versioning. This event indicates that the live version of an object has become an archived version, either because it was archived or because it was overwritten by the upload of an object of the same name.
- **onObjectDeleted** Sent when an object has been permanently deleted. This includes objects that are overwritten or are deleted as part of the bucket's lifecycle configuration. For buckets with object versioning enabled, this is not sent when an object is archived (see onArchive), even if archival occurs via the storage.objects.delete method.
- **onObjectFinalized** Sent when a new object (or a new generation of an existing object) is successfully created in the bucket. This includes copying or rewriting an existing object. A failed upload does not trigger this event.
- **onMetadataUpdated** Sent when the metadata of an existing object changes.





