+++
title = "Create S3 Bucket Job"
weight =  1
date = '2026-01-18T17:33:23+02:00'
type = 'page'
menus = 'main'
+++

# Create S3 Bucket Job

I wanted to have a `Jenkins` job that creates an `S3` bucket for me.

Assumptions:
* Endpoint URL shall be stored somewhere, as the object storage service provides is not the Amazon,
* Bucket name shall be parameterized,
* Credentials shall be taken from `Jenkins` credentials.

Let's deep dive into details

## Credentials

Credentials shall be added as `AWS Credentials`, please specify the identifier, then fill `Access Key ID` field as well as `Secret Access Key`

The snippet you will use later shall contain the following:

```groovy
withCredentials([[ $class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'identifier-you-chose']]) {
```

This block will use given credentials and expose `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` variables to `aws cli`.

## Configuration

`Jenkins` can store configuration values and serve them as files. Create new file by visiting `Manage Jenkins` -> `Managed Files` -> `Add a new config`, then pick `Json file` and specify some human readable id.

The content can look like this:

```json
{
  "my_url": "https://some.url"
}
```

## Use AWS cli

As `AWS` plugin for `Jenkins` does not provide bucket creation, we shall use `aws cli`, like below:

```sh
$ aws s3 mb s3://bucket-name --endpoint-url https://some-url
```

## Full pipeline

Full pipeline looks as follows:

```groovy
pipeline {
    agent any
    parameters {
        string (name: 'BUCKET', description: 'Enter new bucket name')
    }
    stages {
        stage('Make bucket') {
            steps {
                script {
                    if (params.BUCKET.isEmpty()) {
                        currentBuild.result = 'ABORTED'
                        error("Bucket name is empty")
                    }
                }
                script {
                    configFileProvider(
                        [configFile(fileId: 'someconfig', variable: 'SOME_CONFIG')]) {
                        def settings = readJSON file: "${SOME_CONFIG}";
                        withCredentials([[ $class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'my_aws_credentials']]){
                            sh("aws s3 mb s3://${params.BUCKET} --endpoint-url ${settings.my_url}")
                        }
                    }
                }
            }
        }
    }
}
```

### Some explanation needed

To have bucket name be parametrized, you need this:

```groovy
parameters {
    string (name: 'BUCKET', description: 'Enter new bucket name')
}
```

This one checks if bucket name is not empty:

```groovy
if (params.BUCKET.isEmpty()) {
    currentBuild.result = 'ABORTED'
     error("Bucket name is empty")
}
```

This block exposes managed config file, available as `someconfig`, to the filesystem -- its name is now accessible via `${SOME_CONFIG}` variable

```groovy
configFileProvider(
    [configFile(fileId: 'someconfig', variable: 'SOME_CONFIG')]) {
        def settings = readJSON file: "${SOME_CONFIG}";
        (...)
```

`$settings` now holds a dictionary, read from a `JSON` file (`${SOME_CONFIG}`)

You can access its element(s) like here: `${settings.my_url}`

{{% hint warning %}}
**You need to have `Pipeline Utility Steps` installed for `readJSON`**

You need to have `Pipeline Utility Steps` installed in case you use `readJSON`, otherwise the job fail with ambiguous stacktrace.

Here's [plugin page](https://plugins.jenkins.io/pipeline-utility-steps/) (follow the instructions and you're good to go)
{{% /hint %}}