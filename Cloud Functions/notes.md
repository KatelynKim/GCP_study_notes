### Cloud Functions
* Run code in response to events
* Various runtimes supported
* No need to worry about servers, scaling, or availability
* Pay for # of invocations, compute time of invocations, and memory and CPU provisioned
* Time bound - default timeout of 1 min and 9 min for 1st gen and max 60 min for 2 gen for invocations triggered by HTTP requests.
* First and second gens available

### Events can be triggered from:
- Cloud storage
- Cloud Pub/sub
- HTTP requests
- Firebase
- Firestore
- Stackdriver logging

### Key points:
1. No server management - no need to think about scaling or availability
2. Cloud functions automatically spin up and back down in response to events (They scale horizontally)
3. Recommended for responding to events.
4. Not recommended for long-running processes such as batch processing.

### Second generation enhacnements
* Longer request timeout - up to 60 min for HTTP-trigerred functions
* Larger instance sizes - up to 16 GiB RAM with 4 vCPU (V1 was 8 GB RAM with 2 vCPU)
* Concurrency - can handle up to 1000 concurrent requests per function instance (vs 1 in 1st gen)
* Multiple function revisions and traffic splitting supported (Not in v1)
* Support 90+ event types - enabled by Eventarc (Only 7 in v1)\
* Uses Clound Run in the background to create a container for running our cloud functions.

#### Scaling and concurrency
* Autoscaling - new function instances created as new invocations come in
* One function instance handles ONLY ONE request AT A TIME
* Three concurrent function invocations => 3 function instances. However, one instance that has completed execution may be reused.
* Problem: Cold start - new function instance initialization from scratch can take time.
  * Solution: Configure minimum num of instances (Configure at least 10 instances)
    * Downside: increases costs
* 1st gen uses the typical serverless functions architecture
* 2nd gen - One function instance can handle multiple requests CONCURRENTLY. Max is 1000 per instance. Possible because it uses Cloud Run under the hood.

#### [gcloud functions deploy](https://cloud.google.com/functions/docs/create-deploy-gcloud)

```bash
gcloud functions deploy [NAME]
--docker-registry
--docker-repository
--gen2 # Use 2nd gen. Default is 1st gen
--runtime # nodejs, python, etc.
--service-account
--timeout
--max-instances
--min-instances
--source # Can be zip file from Google Cloud storage, source repo, or local file system
# Zip file
--source gs://my-sorce-bucket/my_function_source.zip
# Source repo
--source https://URL/projects/${PROJECT}/repos/${REPO}
# Local file system
--trigger-bucket
--trigger-http
--trigger-topic
--trigger-event-filters # only in 2nd gen
```

[1st and 2nd gen comparison](https://cloud.google.com/functions/docs/concepts/version-comparison)