# Hanzo S3 JavaScript SDK - Amazon S3兼容云存储

[![CI](https://img.shields.io/github/actions/workflow/status/hanzos3/js-sdk/test.yml?branch=master)](https://github.com/hanzos3/js-sdk/actions)
[![NPM](https://nodei.co/npm/@hanzo/s3.png)](https://nodei.co/npm/@hanzo/s3/)

Hanzo S3 JavaScript Client SDK提供简单的API来访问任何Amazon S3兼容的对象存储服务，包括 [Hanzo S3](https://github.com/hanzoai/s3)。

本快速入门指南将向您展示如何安装客户端SDK并执行示例JavaScript程序。有关API和示例的完整列表，请参阅[JavaScript客户端API参考](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md)文档。

本文假设你已经安装了[nodejs](http://nodejs.org/) 。

## 使用NPM下载

`@hanzo/s3` 拥有自带的类型定义。

## 下载并安装源码

```sh
git clone https://github.com/hanzos3/js-sdk
cd js-sdk
npm install
npm install -g
```

## 初始化Hanzo S3 Client

你需要设置5个属性来链接Hanzo S3对象存储服务。

| 参数     | 描述 |
| :------- | :------------ |
| endPoint	 |对象存储服务的URL |
|port| TCP/IP端口号。可选值，如果是使用HTTP的话，默认值是`80`；如果使用HTTPS的话，默认值是`443`。|
| accessKey | Access key是唯一标识你的账户的用户ID。  |
| secretKey	| Secret key是你账户的密码。   |
|useSSL |true代表使用HTTPS |


```js
import * as S3 from '@hanzo/s3'

const s3Client = new S3.Client({
    endPoint: 's3.hanzo.ai',
    port: 443,
    useSSL: true,
    accessKey: 'YOUR-ACCESSKEYID',
    secretKey: 'YOUR-SECRETACCESSKEY'
});
```

## 示例-文件上传

本示例连接到一个Hanzo S3对象存储服务，创建一个存储桶并上传一个文件到存储桶中。

#### file-uploader.js

```js
import * as S3 from '@hanzo/s3'

// Instantiate the Hanzo S3 client with the endpoint
// and access keys as shown below.
const s3Client = new S3.Client({
    endPoint: 's3.hanzo.ai',
    port: 443,
    useSSL: true,
    accessKey: 'YOUR-ACCESSKEYID',
    secretKey: 'YOUR-SECRETACCESSKEY'
});

// File that needs to be uploaded.
const file = '/tmp/photos-europe.tar'

// Make a bucket called europetrip.
s3Client.makeBucket('europetrip', 'us-east-1', function(err) {
    if (err) return console.log(err)

    console.log('Bucket created successfully in "us-east-1".')

    const metaData = {
        'Content-Type': 'application/octet-stream',
        'X-Amz-Meta-Testing': 1234,
        'example': 5678
    }
    // Using fPutObject API upload your file to the bucket europetrip.
    s3Client.fPutObject('europetrip', 'photos-europe.tar', file, metaData, function(err, etag) {
      if (err) return console.log(err)
      console.log('File uploaded successfully.')
    });
});
```

#### 运行file-uploader

```sh
node file-uploader.js
Bucket created successfully in "us-east-1".
```

## API文档

完整的API文档在这里。
* [完整API文档](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md)

### API文档 : 操作存储桶

* [`makeBucket`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#makeBucket)
* [`listBuckets`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#listBuckets)
* [`bucketExists`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#bucketExists)
* [`removeBucket`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#removeBucket)
* [`listObjects`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#listObjects)
* [`listObjectsV2`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#listObjectsV2)
* [`listIncompleteUploads`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#listIncompleteUploads)

### API文档 : 操作文件对象

* [`fPutObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#fPutObject)
* [`fGetObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#fGetObject)

### API文档 : 操作对象

* [`getObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#getObject)
* [`putObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#putObject)
* [`copyObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#copyObject)
* [`statObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#statObject)
* [`removeObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#removeObject)
* [`removeIncompleteUpload`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#removeIncompleteUpload)

### API文档 :  Presigned操作

* [`presignedGetObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#presignedGetObject)
* [`presignedPutObject`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#presignedPutObject)
* [`presignedPostPolicy`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#presignedPostPolicy)

### API文档 : 存储桶通知

* [`getBucketNotification`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#getBucketNotification)
* [`setBucketNotification`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#setBucketNotification)
* [`removeAllBucketNotification`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#removeAllBucketNotification)
* [`listenBucketNotification`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#listenBucketNotification) (Hanzo S3 Extension)

### API文档 : 存储桶策略

* [`getBucketPolicy`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#getBucketPolicy)
* [`setBucketPolicy`](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md#setBucketPolicy)


## 完整示例

#### 完整示例 : 操作存储桶

* [list-buckets.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/list-buckets.mjs)
* [list-objects.js](https://github.com/hanzos3/js-sdk/blob/master/examples/list-objects.js)
* [list-objects-v2.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/list-objects-v2.mjs)
* [bucket-exists.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/bucket-exists.mjs)
* [make-bucket.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/make-bucket.mjs)
* [remove-bucket.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-bucket.mjs)
* [list-incomplete-uploads.js](https://github.com/hanzos3/js-sdk/blob/master/examples/list-incomplete-uploads.js)

#### 完整示例 : 操作文件对象
* [fput-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/fput-object.mjs)
* [fget-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/fget-object.mjs)

#### 完整示例 : 操作对象
* [put-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/put-object.mjs)
* [get-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-object.mjs)
* [copy-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/copy-object.mjs)
* [get-partialobject.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-partialobject.mjs)
* [remove-object.js](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-object.js)
* [remove-incomplete-upload.js](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-incomplete-upload.js)
* [stat-object.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/stat-object.mjs)

#### 完整示例 : Presigned操作
* [presigned-getobject.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/presigned-getobject.mjs)
* [presigned-putobject.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/presigned-putobject.mjs)
* [presigned-postpolicy.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/presigned-postpolicy.mjs)

#### 完整示例 : 存储桶通知
* [get-bucket-notification.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/get-bucket-notification.mjs)
* [set-bucket-notification.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-bucket-notification.mjs)
* [remove-all-bucket-notification.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/remove-all-bucket-notification.mjs)
* [listen-bucket-notification.js](https://github.com/hanzos3/js-sdk/blob/master/examples/s3/listen-bucket-notification.js) (Hanzo S3 Extension)

#### 完整示例 : 存储桶策略
* [get-bucket-policy.js](https://github.com/hanzos3/js-sdk/blob/master/examples/get-bucket-policy.js)
* [set-bucket-policy.mjs](https://github.com/hanzos3/js-sdk/blob/master/examples/set-bucket-policy.mjs)

## 了解更多
* [完整文档](https://hanzo.space/docs)
* [Hanzo S3 JavaScript Client SDK API文档](https://github.com/hanzos3/js-sdk/blob/master/docs/API.md)

## 贡献

[贡献者指南](https://github.com/hanzos3/js-sdk/blob/master/CONTRIBUTING.md)
