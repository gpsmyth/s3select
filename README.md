# Understanding S3 Select

## Establish Env

- We're using python3
- In a created directory
  - Create your data.csv file
  - `python3 -m venv s3select_example/env`
  - `source s3select_example/env/bin/activate`
  - `pip3 install boto3`

## S3 side

- Bucket created with aserver-side ebcryption enabled by default
```bash
  aws s3 mb s3://gpsdata20211107 --region us-east-1 --profile <myprofile>
  
  (output)
  make_bucket: gpsdata20211107

  aws s3api  put-bucket-encryption --bucket gpsdata20211107 --region us-east-1 --server-side-encryption-configuration '{"Rules": [{"ApplyServerSideEncryptionByDefault": {"SSEAlgorithm": "AES256"}}]}'
  (No output produced)
  ```
- Now adding objet to bucket
  ```bash
  aws s3api get-bucket-encryption --bucket gpsdata20211107 --region us-east-1
  {
    "ServerSideEncryptionConfiguration": {
      "Rules": [
        {
            "ApplyServerSideEncryptionByDefault": {
                "SSEAlgorithm": "AES256"
            },
            "BucketKeyEnabled": false
        }
      ]
    }
  }

  aws s3api put-object --profile <myprofile> --bucket gpsdata20211107 --key data.csv --body data.csv
  {
      "ETag": "\"9235db6e2e427644d2cacbb97ae6e7ed\"",
      "ServerSideEncryption": "AES256"
  }
  (Shows that objects added into bucket use the bucket's default encryption policy.
  In this case, AES256 or SSE-S3)
  ```

## Running the program

``` bash
python3 gerry.py 
Gerry,(949) 123-45567,Chicago,Developer
Gerry,(021) 123-4567,Auckland,Engineer

Stats details bytesScanned: 
373
Stats details bytesProcessed: 
373
Stats details bytesReturned: 
79

wc -c data.csv 
     373 data.csv
So scanned file is 373 bytes long, and s3 returned 79 bytes.
So data at s3 is filtered at service side and the data returned is pre-filtered
```

## References 

- [S3 aws select blog](https://aws.amazon.com/blogs/storage/querying-data-without-servers-or-databases-using-amazon-s3-select/)

