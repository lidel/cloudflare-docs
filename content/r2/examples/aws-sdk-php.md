---
title: Configure `aws-sdk-php` for R2
summary: Example of how to configure `aws-sdk-php` to use R2.
pcx-content-type: configuration
weight: 1001
layout: example
---

{{<render file="_keys.md">}}

This example uses version 3 of the [aws-sdk-php](https://packagist.org/packages/aws/aws-sdk-php) package. You must pass in the R2 configuration credentials when instantiating your `S3` service client:

```php
<?php
require 'vendor/aws/aws-autoloader.php';

$bucket_name        = "sdk-example";
$account_id         = "<accountid>";
$access_key_id      = "<access_key_id>";
$access_key_secret  = "<access_key_secret>";

$credentials = new Aws\Credentials\Credentials($access_key_id, $access_key_secret);

$options = [
    'region' => 'auto',
    'endpoint' => "https://$account_id.r2.cloudflarestorage.com",
    'version' => 'latest',
    'credentials' => $credentials
];

$s3_client = new Aws\S3\S3Client($options);

$contents = $s3_client->listObjectsV2([
    'Bucket' => $bucket_name
]);

var_dump($contents['Contents']);

// array(1) {
//   [0]=>
//   array(5) {
//     ["Key"]=>
//     string(14) "ferriswasm.png"
//     ["LastModified"]=>
//     object(Aws\Api\DateTimeResult)#187 (3) {
//       ["date"]=>
//       string(26) "2022-05-18 17:20:21.670000"
//       ["timezone_type"]=>
//       int(2)
//       ["timezone"]=>
//       string(1) "Z"
//     }
//     ["ETag"]=>
//     string(34) ""eb2b891dc67b81755d2b726d9110af16""
//     ["Size"]=>
//     string(5) "87671"
//     ["StorageClass"]=>
//     string(8) "STANDARD"
//   }
// }

$buckets = $s3_client->listBuckets();

var_dump($buckets['Buckets']);

// array(1) {
//   [0]=>
//   array(2) {
//     ["Name"]=>
//     string(11) "sdk-example"
//     ["CreationDate"]=>
//     object(Aws\Api\DateTimeResult)#212 (3) {
//       ["date"]=>
//       string(26) "2022-05-18 17:19:59.645000"
//       ["timezone_type"]=>
//       int(2)
//       ["timezone"]=>
//       string(1) "Z"
//     }
//   }
// }

?>
```