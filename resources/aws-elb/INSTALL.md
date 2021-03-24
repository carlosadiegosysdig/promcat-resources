# Setting up permissions in the AWS account
## Creating an AWS IAM policy
The exporter needs permissions to access the resources from the AWS account.

First, create an AWS IAM policy on your AWS infrastructure allowing to read CloudWatch metrics and get resources by tags.
An example AWS IAM configuration is given below:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "CloudWatchExporterPolicy",
            "Effect": "Allow",
            "Action": [
                "tag:GetResources",
                "cloudwatch:ListTagsForResource",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:GetMetricData",
                "cloudwatch:ListMetrics"
            ],
            "Resource": "*"
        }
    ]
}
```

## Creating the credentials for the exporter
Create a `$HOME/.aws/credentials` file as follows. Substitute the values with your key and password:

```ini
# CREDENTIALS FOR AWS ACCOUNT
aws_region = us-east-1
aws_access_key_id = AYYYYYZZZZZZ3BLXXXXX
aws_secret_access_key = bXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

# Monitor the resources
The YACE exporter uses an API call that filters the resources by tags.
Therefore, if you want to monitor a resource, ensure that **it has at least one tag** associated with it. A resource without a tag will not be scraped.

# Installing the exporter
To install the exporter do the following:

1. Download the `elb-deploy.yaml` file.
2. Change the following lines in the configmap with the AWS region where the resources to monitor are located:
```
region: us-east-1
```
3. Change the field 'credentials' of the secret 'yace-elb-credentials' and paste the content of the following command:
```
cat ~/.aws/credentials | base64
```
4. Apply the deployment:
```
kubectl apply -f elb-deploy.yaml
```
5. You can check that the exporter is working checking that the pods are running:
```
kubectl -n yace get pods
```

# Sysdig Agent configuration
In the yace Deployment we will include the Prometheus annotations configuring the port of the exporter as scraping port.    

Also, in the Sysdig Agent configuration, be sure to have these lines of configuration to scrape the containers with Prometheus annotations.
```yaml
process_filter:
  - include:
      kubernetes.pod.annotation.prometheus.io/scrape: true
      conf:
        path: "{kubernetes.pod.annotation.prometheus.io/path}"
        port: "{kubernetes.pod.annotation.prometheus.io/port}"
```

You can download the sample configuration file below and apply it by:
```bash
kubectl apply -f sysdig-agent-config.yaml
```
