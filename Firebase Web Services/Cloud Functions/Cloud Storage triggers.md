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
const fileBucket = event.data.bucket; // Storage bucket containing the file.
const filePath = event.data.name; // File path in the bucket.
const contentType = event.data.contentType; // File content type.
});
```

<br />

## Handle Cloud Storage events

* **onObjectArchived** <br /> Only sent when a bucket has enabled object versioning. This event indicates that the live version of an object has become an archived version, either because it was archived or because it was overwritten by the upload of an object of the same name.
  
* **onObjectDeleted** <br /> Sent when an object has been permanently deleted. This includes objects that are overwritten or are deleted as part of the bucket's lifecycle configuration. For buckets with object versioning enabled, this is not sent when an object is archived (see onArchive), even if archival occurs via the storage.objects.delete method.
  
* **onObjectFinalized** <br />  Sent when a new object (or a new generation of an existing object) is successfully created in the bucket. This includes copying or rewriting an existing object. A failed upload does not trigger this event.
  
* **onMetadataUpdated** <br />  Sent when the metadata of an existing object changes.

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
