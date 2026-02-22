# Hanzo S3 JavaScript SDK

[![CI](https://img.shields.io/github/actions/workflow/status/hanzos3/js-sdk/test.yml?branch=master)](https://github.com/hanzos3/js-sdk/actions)
[![NPM](https://nodei.co/npm/@hanzo/s3.png)](https://nodei.co/npm/@hanzo/s3/)

The Hanzo S3 JavaScript Client SDK provides high-level APIs to access any Amazon S3 compatible object storage server, including [Hanzo S3](https://github.com/hanzoai/s3).

This guide will show you how to install the client SDK and execute an example JavaScript program.
For a complete list of APIs and examples, please take a look at the [JavaScript Client API Reference](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md) documentation.

This document presumes you have a working [Node.js](http://nodejs.org/) development environment, LTS versions v16, v18 or v20.

## Download from NPM

```sh
npm install --save @hanzo/s3
```

## Download from Source

```sh
git clone https://github.com/hanzos3/js-sdk
cd js-sdk
npm install
npm run build
npm install -g
```

## Using with TypeScript

`@hanzo/s3` is shipped with builtin type definitions.

## Initialize Hanzo S3 Client

The following parameters are needed to connect to a Hanzo S3 object storage server:

| Parameter   | Description                                                                  |
| :---------- | :--------------------------------------------------------------------------- |
| `endPoint`  | Hostname of the object storage service.                                      |
| `port`      | TCP/IP port number. Optional, defaults to `80` for HTTP and `443` for HTTPs. |
| `accessKey` | Access key (user ID) of an account in the S3 service.                        |
| `secretKey` | Secret key (password) of an account in the S3 service.                       |
| `useSSL`    | Optional, set to 'true' to enable secure (HTTPS) access.                     |

```js
import * as S3 from '@hanzo/s3'

const s3Client = new S3.Client({
  endPoint: 's3.hanzo.ai',
  port: 443,
  useSSL: true,
  accessKey: 'YOUR-ACCESSKEYID',
  secretKey: 'YOUR-SECRETACCESSKEY',
})
```

## Quick Start Example - File Uploader

This example connects to a Hanzo S3 object storage server, creates a bucket, and uploads a file to the bucket.

#### file-uploader.mjs

```js
import * as S3 from '@hanzo/s3'

// Instantiate the Hanzo S3 client with the object store service
// endpoint and an authorized user's credentials
const s3Client = new S3.Client({
  endPoint: 's3.hanzo.ai',
  port: 443,
  useSSL: true,
  accessKey: 'YOUR-ACCESSKEYID',
  secretKey: 'YOUR-SECRETACCESSKEY',
})

// File to upload
const sourceFile = '/tmp/test-file.txt'

// Destination bucket
const bucket = 'js-test-bucket'

// Destination object name
const destinationObject = 'my-test-file.txt'

// Check if the bucket exists
// If it doesn't, create it
const exists = await s3Client.bucketExists(bucket)
if (exists) {
  console.log('Bucket ' + bucket + ' exists.')
} else {
  await s3Client.makeBucket(bucket, 'us-east-1')
  console.log('Bucket ' + bucket + ' created in "us-east-1".')
}

// Set the object metadata
var metaData = {
  'Content-Type': 'text/plain',
  'X-Amz-Meta-Testing': 1234,
  example: 5678,
}

// Upload the file with fPutObject
// If an object with the same name exists,
// it is updated with new data
await s3Client.fPutObject(bucket, destinationObject, sourceFile, metaData)
console.log('File ' + sourceFile + ' uploaded as object ' + destinationObject + ' in bucket ' + bucket)
```

#### Run the File Uploader

```sh
node file-uploader.mjs
Bucket js-test-bucket created successfully in "us-east-1".
File /tmp/test-file.txt uploaded successfully as my-test-file.txt to bucket js-test-bucket
```

## API Reference

The complete API Reference is available here:

- [Hanzo S3 JavaScript API Reference](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md)

### Bucket Operations

- [`makeBucket`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#makeBucket)
- [`listBuckets`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#listBuckets)
- [`bucketExists`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#bucketExists)
- [`removeBucket`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#removeBucket)
- [`listObjects`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#listObjects)
- [`listObjectsV2`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#listObjectsV2)
- [`listObjectsV2WithMetadata`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#listObjectsV2WithMetadata) (Extension)
- [`listIncompleteUploads`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#listIncompleteUploads)
- [`getBucketVersioning`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#getBucketVersioning)
- [`setBucketVersioning`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#setBucketVersioning)
- [`setBucketLifecycle`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#setBucketLifecycle)
- [`getBucketLifecycle`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#getBucketLifecycle)
- [`removeBucketLifecycle`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#removeBucketLifecycle)
- [`getObjectLockConfig`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#getObjectLockConfig)
- [`setObjectLockConfig`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#setObjectLockConfig)

### File Object Operations

- [`fPutObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#fPutObject)
- [`fGetObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#fGetObject)

### Object Operations

- [`getObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#getObject)
- [`putObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#putObject)
- [`copyObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#copyObject)
- [`statObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#statObject)
- [`removeObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#removeObject)
- [`removeObjects`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#removeObjects)
- [`removeIncompleteUpload`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#removeIncompleteUpload)
- [`selectObjectContent`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#selectObjectContent)

### Presigned Operations

- [`presignedUrl`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#presignedUrl)
- [`presignedGetObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#presignedGetObject)
- [`presignedPutObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#presignedPutObject)
- [`presignedPostPolicy`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#presignedPostPolicy)

### Bucket Notification Operations

- [`getBucketNotification`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#getBucketNotification)
- [`setBucketNotification`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#setBucketNotification)
- [`removeAllBucketNotification`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#removeAllBucketNotification)
- [`listenBucketNotification`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#listenBucketNotification) (Hanzo S3 Extension)

### Bucket Policy Operations

- [`getBucketPolicy`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#getBucketPolicy)
- [`setBucketPolicy`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#setBucketPolicy)

## Examples

#### Bucket Operations

- [list-buckets.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/list-buckets.mjs)
- [list-objects.js](https://github.com/hanzos3/js-sdk/blob/master/examples/list-objects.js)
- [list-objects-v2.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/list-objects-v2.mjs)
- [list-objects-v2-with-metadata.js](https://github.com/hanzos3/js-sdk/blob/master/examples/list-objects-v2-with-metadata.js) (Extension)
- [bucket-exists.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/bucket-exists.mjs)
- [make-bucket.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/make-bucket.mjs)
- [remove-bucket.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-bucket.mjs)
- [list-incomplete-uploads.js](https://github.com/hanzos3/js-sdk/blob/master/examples/list-incomplete-uploads.js)
- [get-bucket-versioning.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-bucket-versioning.mjs)
- [set-bucket-versioning.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-bucket-versioning.mjs)
- [set-bucket-tagging.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-bucket-tagging.mjs)
- [get-bucket-tagging.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-bucket-tagging.mjs)
- [remove-bucket-tagging.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-bucket-tagging.mjs)
- [set-bucket-lifecycle.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-bucket-lifecycle.mjs)
- [get-bucket-lifecycle.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-bucket-lifecycle.mjs)
- [remove-bucket-lifecycle.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-bucket-lifecycle.mjs)
- [get-object-lock-config.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-object-lock-config.mjs)
- [set-object-lock-config.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-object-lock-config.mjs)
- [set-bucket-replication.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-bucket-replication.mjs)
- [get-bucket-replication.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-bucket-replication.mjs)
- [remove-bucket-replication.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-bucket-replication.mjs)
- [set-bucket-encryption.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-bucket-encryption.mjs)
- [get-bucket-encryption.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-bucket-encryption.mjs)
- [remove-bucket-encryption.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-bucket-encryption.mjs)

#### File Object Operations

- [fput-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/fput-object.mjs)
- [fget-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/fget-object.mjs)

#### Object Operations

- [put-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/put-object.mjs)
- [get-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-object.mjs)
- [copy-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/copy-object.mjs)
- [get-partialobject.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-partialobject.mjs)
- [remove-object.js](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-object.js)
- [remove-incomplete-upload.js](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-incomplete-upload.js)
- [stat-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/stat-object.mjs)
- [get-object-retention.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-object-retention.mjs)
- [put-object-retention.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/put-object-retention.mjs)
- [set-object-tagging.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-object-tagging.mjs)
- [get-object-tagging.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-object-tagging.mjs)
- [remove-object-tagging.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-object-tagging.mjs)
- [set-object-legal-hold.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-object-legal-hold.mjs)
- [get-object-legal-hold.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-object-legal-hold.mjs)
- [compose-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/compose-object.mjs)
- [select-object-content.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/select-object-content.mjs)

#### Presigned Operations

- [presigned-getobject.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/presigned-getobject.mjs)
- [presigned-putobject.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/presigned-putobject.mjs)
- [presigned-postpolicy.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/presigned-postpolicy.mjs)

#### Bucket Notification Operations

- [get-bucket-notification.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-bucket-notification.mjs)
- [set-bucket-notification.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-bucket-notification.mjs)
- [remove-all-bucket-notification.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-all-bucket-notification.mjs)
- [listen-bucket-notification.js](https://github.com/hanzos3/js-sdk/blob/master/examples/s3/listen-bucket-notification.js) (Hanzo S3 Extension)

#### Bucket Policy Operations

- [get-bucket-policy.js](https://github.com/hanzos3/js-sdk/blob/master/examples/get-bucket-policy.js)
- [set-bucket-policy.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-bucket-policy.mjs)

## Custom Settings

- [setAccelerateEndPoint](https://github.com/hanzos3/js-sdk/blob/master/examples/set-accelerate-end-point.js)

## Explore Further

- [Hanzo S3 Documentation](https://hanzo.space/docs)
- [Hanzo S3 JavaScript API Reference](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md)

## Contribute

- [Contributors Guide](https://github.com/hanzos3/js-sdk/blob/master/CONTRIBUTING.md)

## License

[Apache 2.0](LICENSE)
