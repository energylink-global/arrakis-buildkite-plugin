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
      github.com/cozero/arrakis-buildkite-plugin#v0.0.1:
        master: true
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
```

## Options

* `master` - run pipeline mode for a master account
* `queue` - used to set the buildkite agent queue
* `state_file` - custom path for the terraform state file
* `tf_version` - used to specify the terraform docker tag that you would like to use
* `unify` - set to true to run in "unification" mode, with multi-environment tfvars stored in namespaced files
