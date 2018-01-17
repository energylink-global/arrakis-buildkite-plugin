# Arrakis Buildkite Plugin

(c) COzero 2017

MIT (see [LICENCE](LICENCE))

![Arrakis](arrakis.jpg)

## Synopsis

Dynamically generates a buildkite pipeline to test and deploy your terraform infrastructure.

## Usage

To use, simply create your pipline.yml as follows

```
# .buildkite/pipeline.yml
steps:
  - label: ":unicorn_face: Building pipeline :facepunch:"
    plugins:
      github.com/cozero/arrakis-buildkite-plugin#v0.2.0:
        queue: my-infrastructure-stack
        tf_version: stable
    agents:
      queue: my-infrastructure-stack
```

You'll also need to specify terraform variables for "environment" and "platform" in your root terraform.tfvars - these are passed through to the dynamic pipeline.

```
# terraform.tfvars

variable environment = "production"

variable platform    = "energylink"

variable remote_state {
  # overridden by master_remote_state if provided
  bucket = "my-great-bucket"
  key    = "my-great-key"
  # optional and overriden by master_remote_state if provided
  region = "ap-southeast-2"
}

# optional
variable master_remote_state {
  bucket = "my-great-bucket"
  key    = "my-great-master-key"
  # optional
  region = "ap-southeast-2"
}

```

## Options

* `queue` - used to set the buildkite agent queue
* `tf_version` - used to specify the terraform docker tag that you would like to use
