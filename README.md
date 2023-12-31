# aws-s3-glacier-restore
Restore objects

### Features at a glance
- Calculates exact costs before restore
- Multithreaded API requests significantly reduce restore time
- Can check if all files for given prefix are already restored or not.

### Installation and Upgrade
aws-s3-glacier-restore is developed in Python and uses pip.
### First time setup
- The easiest way to install/upgrade s3select is to use pip:
```
$ pip3 install -U aws-s3-glacier-restore
```
### Authentication
aws-s3-glacier-restore uses the same authentication and endpoint configuration as aws-cli. If aws command is working on your machine, there is no need for any additional configuration.

### Example usage
To get help and see available options:
```
$ aws-s3-glacier-restore -h
usage: aws-s3-glacier-restore [-h] [-p PREFIX] [-i INPUT_FILE]
                              [-d DAYS_TO_KEEP] [-D DESTINATION_BUCKET]
                              [-t THREADS] [-s] [--profile PROFILE]

Utility script to restore files on AWS S3 that have GLACIER storage class

optional arguments:
  -h, --help            show this help message and exit
  -p PREFIX, --prefix PREFIX
                        S3 prefix URL to restore
  -i INPUT_FILE, --input_file INPUT_FILE
                        Input file containing all s3 paths to restore
  -d DAYS_TO_KEEP, --days_to_keep DAYS_TO_KEEP
                        How many days to keep restored files
  -D DESTINATION_BUCKET, --destination_bucket DESTINATION_BUCKET
                        Restore to this bucket instead of to original bucket
                        while preserving same path structure as in original
                        bucket. This is useful if you don't know for how long
                        you'll need restored files. Once you don't need them
                        you can delete them from destination bucket.
  -t THREADS, --threads THREADS
                        Number of threads to use. Default=40
  -s, --status_print    Just print status of files and how many of them are in
                        glacier and how many of them are restored already
  --profile PROFILE     Use a specific AWS profile from your credential file.
  ```


The script 'aws-s3-glacier-restore' will read the object_restore.txt file and initiate the restoration process for the specified objects. The script will handle the restoration based on the configuration provided in the command-line arguments, such as the number of threads, restoration tier, days to keep restored files, etc.

To restore objects, you can simply replace their S3 URLs or prefixes to the object_restore.txt file (https://github.com/elsevier-research/databricks-rt-utils/blob/main/s3-dbfs-glacier-restore/object_restore.txt). one per line, as shown in the example below.
```
s3://bucket-name/object-key-1
s3://bucket-name/object-key-2
s3://bucket-name/prefix/folder/object-key-3
```
The script will automatically pick up the objects and initiate the restoration process for them.
  
Example usage to restore files:
```
$ aws-s3-glacier-restore -i object_restore.txt -d 5
$ aws-s3-glacier-restore -i <input_file> -d <days_to_keep>
```
-i: Input file containing S3 paths to restore.
-d: Number of days to keep restored files.

or
```
aws-s3-glacier-restore -p s3://bucket-name/prefix/folder/object-key -d <days_to_keep>
```

If you want to check restore status of files you can use -s switch:
```
aws-s3-glacier-restore -i object_restore.txt -s
```
