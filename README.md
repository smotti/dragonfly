# Description

A set of monitoring scripts that publish metrics to AWS CloudWatch.

**Note the namespace used is SCONE/Custom!**

# Requirements

* Python 3.x
  * boto3 (AWS SDK)

# Usage

```
./dragonfly CONFIG
```

# Configuration

See config.json.example for a configuration example.

Security credentials to authenticate with AWS are looked up in the
following order:

1. When *role* is specified in CONFIG check for temporary security credentials
via instance meta-data
2. If no role or no temporary security credentials were found look for
environment variables *ACCESS\_KEY\_ID* and *SECRET\_ACCESS\_KEY*.
3. Use values from CONFIG by looking for the keys *access\_key\_id* and
*secret\_access\_key*.

A list of allowed options:

* role
  * The role that is attached to the instance, that allows it to access AWS
    services
* access\_key\_id
  * The AWS access key id used for authentication
* secret\_access\_key
  * The AWS secret key used for authentication
* processes
  * A list of dicts defining processes that should be counted
  * A dict must have the following keys:
      * process: A name for the process to count. This is used to create the
        metric name as follows *ProcessCount<process>*
      * regex: A regular expression that is used to count matches in the systems
        process table that identify the specified process
* dimensions
  * A way of grouping the metrics (see [here](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ec2-metricscollected.html#ec2-metric-dimensions) for examples)
  * A list of dicts defining dimensions
  * A dict must contain the following keys (capitalization **important**):
      * Name: The name of the dimension (i.e. InstanceId, AutoScalingGroupName, ...)
      * Value: The value for that dimension (i.e. if the Name is InstanceId the
        Value would be the id of the instance that publishes the metrics)

