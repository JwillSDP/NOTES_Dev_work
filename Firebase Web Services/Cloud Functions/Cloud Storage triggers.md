# Cloud Storage triggers

> You can trigger a function in response to the uploading, updating, or deleting of files and folders in Cloud Storage.

<br />

## Import the required modules

```jsx
 const {onObjectFinalized} = require("firebase-functions/v2/storage")
```

<br />

## Scope a Cloud Storage function

```jsx
// scope handler to a specific bucket, using storage options parameter
export archivedopts = onObjectArchived({ bucket: "myBucket" }, (event) => {
  //…
});
```

## Handle Cloud Storage events

* **onObjectArchived** <br /> Only sent when a bucket has enabled object versioning. This event indicates that the live version of an object has become an archived version, either because it was archived or because it was overwritten by the upload of an object of the same name.
  
* **onObjectDeleted** <br /> Sent when an object has been permanently deleted. This includes objects that are overwritten or are deleted as part of the bucket's lifecycle configuration. For buckets with object versioning enabled, this is not sent when an object is archived (see onArchive), even if archival occurs via the storage.objects.delete method.
  
* **onObjectFinalized** <br />  Sent when a new object (or a new generation of an existing object) is successfully created in the bucket. This includes copying or rewriting an existing object. A failed upload does not trigger this event.
  
* **onMetadataUpdated** <br />  Sent when the metadata of an existing object changes.
