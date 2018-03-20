# s3-stream-download

Multipart streaming download from S3 with progress watch

## Features

## Installation

``` bash
  $ npm i git+https://github.com/medroomdev/s3-stream-download.git --save
```

## Usage

```js
var AWS = require('aws-sdk');
var S3StreamDownload = require('./index');
var fs = require('fs');

var s3 = new AWS.S3({
    apiVersion: '2006-03-01',
    region: '<REGION>',
    signatureVersion: 'v4',
    s3DisableBodySigning: false
});

let download = new s3StreamDownload(s3, { Bucket: '<BUCKET>', Key: '<KEY>' }, { downloadChunkSize:  1024 * 1024, concurrentChunks: 15, 
    onLoad: (totalSize, chunkSize) => {
        // called after totalSize and chunkSize of file is calculated
    }, 
    onPart: () => {
        // called after each part is downloaded
    }
});

download.pipe(window.require('fs').createWriteStream(path.join(app.getAppPath(), '<FILEPATH>')));

download.on('end', () => {
    console.log("finished");
});
```

## Documentation

### new S3StreamDownload(s3, s3Params, options)

Creates a new instance of a multipart download stream.  This is a readable stream that can be
piped into other streams.

__Arguments__

* `s3` - Configured aws-sdk s3 instance.  Should be preconfigured with any credentials.
* `s3Params` - Params object that would normally be passed to s3.getObject()
* `options` - Options
    - `downloadChunkSize` - Size of each chunk in bytes.  Defaults to 5MB.
    - `concurrentChunks` - Number of chunks to download concurrently. Defaults to 5.
    - `retries` - How many times a failed chunk should be retried before failing the entire download. Retries will exponentially backoff to allow for recovery.  Defaults to 5.
    - `onLoad` - Callback for when the information of the object is retrieved from s3.
    - `onPart` - Callback for when a part is downloaded.



## People

[Raphael Rosa](https://github.com/RaphaelRosa) from [MedRoom](http://www.medroom.com.br)

based on the package created by [Chris Kinsman](https://github.com/chriskinsman) from [PushSpring](http://www.pushspring.com)

## License

  [MIT](LICENSE)

[shippable-image]: https://api.shippable.com/projects/57bfbd4c016a370e00eb8907/badge?branch=master
[shippable-url]: https://app.shippable.com/projects/57bfbd4c016a370e00eb8907
