### GCS

#### Creating a bucket

- Globally unique name
- Region
- Default storage class:
  - Standard - frequently accessed data, no minimum storage duration
  - Nearline - <= once a month, 30 days
  - Coldline - <= once a quarter, 90 days
  - Archive - <= once a year, 365 days

#### Cloud storage

- Serverless - autoscaling and infinite scale
- Storage large objects using a key-value approach:
  - key is sort of the relative path of the file, and value the file content
  - Treats entire object as a unit (partial update not allowed)
  - Access at object level

#### Objects and Buckets

- Objects are stored in buckets with globally unique names
- Bucket names are used as part of object URLs
- Unlimited objects in a bucket
- Each object has a unique key
- Max object size is 5 TB
- Can configure storage class at the level of a bucket or an object. Default is the storage class of the bucket.

### Features across storage classes

- high durability (> 99.99% annual durability)
- low latency
- unlimited storage:
  - autoscaling
  - no minimum object size
- Same APIS across storage classes
- comitted SLA is 99.95% for multi-region and 99.90% for single region for standard, nearline, an coldline storage. No SLA for archive storage.

#### [Uploading and downloading objects](https://cloud.google.com/storage/docs/objects)

- Simple upload - small files + NO object metadata
- Multipart upload - for small files + object data
- Resumable upload - for larger files. Costs one additional HTTP request
- streaming transfers - upload an object of unknown size
- Parallel composite uploads - File is divided up to 32 chunks,which are uploaded in parallel. Faster if network and disk speed are not limiting factors.
- Simple download - Downloading objects to a destination
- Streaming download - downloading data to a process
- Sliced object download - slice and download large objects

#### Versioning

- Prevents accidental deletion and provides histor of what happened with a specific object
- You can restore an accidentally deleted object.
- Versioning is enabled at a bucket level.
- Live version is the latest version.
- Older versions are identified by object key + a generation number
- Reduce costs by deleting older (noncurrent) versions

#### Object lifecycle management

- File usage reduces with time
- You can save costs by moving files automatically between sotrage classes via Object Lifecycle Management.
- Identify objects using conditions based on age, CreatedBefore, isLive, MatchesStorageClass, NumberOfNewerVersions , etc.
- Two kinds of actions:
  - SetStorageClasscations (Change from one storage class to another)
  - Deletion (delete objects)
    Restrictions (Can only move to colder storage classes)
- Standard/Multi-regional/Regional to Nearline/ColdLine/Archive
- Nearline to Coldline/Archive
- Coldline to Archive

#### Encryption with KMS

- Cloud storage always encrypts data on the server side
- Configure server-side encryption:
  - Google-managed encryption key - default
  - Customer-Managed encryption Keys - created using Cloud Key Mangaement Service (KMS):
    - Cloud Storage Service Account should have access to keys in KMS for encrypting and ecrypting using the Customer-Managed encryption key
- Configure client-side encryption - encryption performed by customer before upload.
  - GCP does not know about the keys used.
  - ADvantage is that even while the data is being transmitted to GCP, the data is encrypted.

#### Cloud Storage - Scenarios

1. Speeding up large uploads to cloud storage?
   Use <strong> parallel coposite uploads </strong>
2. You don't want to permanently store application logs for regulatory reasons. You dont' expect to access them at all
   Assign Archive storage class
3. Logs stored in Cloud storage that should be accessed less than once in a quarter
   Coldline
4. How to change storage class of an existing bucket with thousands of objects to another class
   First, change the default storage class of the bucket. Go to bucket configuration tab to do this.
   Second, update the storage class of the objects individually in the bucket.

### [gsutil](https://cloud.google.com/storage/docs/gsutil)

```bash
gsutil mb gs://BUCKET_NAME # Make a bucket
gsutil ls -a gs://BUCKET_NAME # List all current and non-current objects inside the bucket
gsutil cp gs://SOURCE_BUCKET/SOURCE_OBJECT gs://DESTINATION_BUCKET/NAME_COPY # Copy objects
gsutil mv gs://BUCKET_NAME/OLD_OBJECT_NAME gs://BUCKET_NAME/NEW_OBJECT_NAME #Rename object
gsutil mv gs://OLD_BUCKET_NAME/OLD_OBJECT_NAME gs://NEW_BUCKET_NAME/NEW_OBJECT_NAME # Move object to another bucket
gsutil rewrite -s STORAGE_CLASS gs://BUCKET_NAME/OBJECT_PATH # Change storage class of an object
gsutil cp LOCAL_LOCATION gs://DESTINATION_BUCKET_NAME # Upload an object to a bucket
gsutil cp gs://BUCKET_NAME/OBJECT_PATH LOCAL_LOCATION # Download an object from a bucket

gsutil versioning set on/off gs://BUCKET_NAME
gsutil uniformbucketlevelaccess set on/off gs://BUCKET_NAME
gsutil acl ch # Set access permissions ofr specific objects
gsutil acl ch -u AllUsers:R gs://BUCKET_NAME/OBJECT_PATH # Makes specific object public
gsutil acl ch -u john.doe@example.com:WRITE gs://BUCKET_NAME/OBJECT_PATH
gsutil acl set JSON_FILE gs://BKT_NAME
## Permissions - READ (R), WRITE (R), OWNER (O)
## Scope - User, allAuthenticatedUsers, allUsers(-u), Group (-g), Project (-p), etc.

gsutil iam ch MEMBER_TYPE:MEMBER_NANE:IAM_ROLE gs://BUCKET_NAME #Setup IAM role
gsutil iam ch user:me@myemail.com:objectCreator gs://BUCKET_NAME
gsutil iam ch allUsers:objectViewer gs://BUCKET_NAME #Make the entire bucket readable
gsutil signurl -d 10m YOUR_SERIVCE_ACCOUNT_KEY gs://BUCKET_NAME/OBJECT_PATH # Signed URL for temporary access (for 10 minutes)






```
