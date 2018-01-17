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
* `tf_vars_file` - the file you'd like to target
* `tf_version` - used to specify the terraform docker tag that you would like to use

## Extra

This plugin has a three main requirements-

* Ensure that plans are always run against the correct environment.
* Ensure that we can update terraform easily and centrally
* Enable a traditional dev » staging » production workflow

Terraform updates often require tweaks to the  init / plan / apply plans - by centrally managing these commands (via the Bitbucket/Buildkite module), repos more easily be kept up to date.

Multi environment repos typically have 2 different state files - production and staging. It could be disastrous to apply a terraform plan against the wrong state file.

In order to overcome the multi-environment problem, we simply keep the two tfvars files in the same repo

* production.tfvars
* staging.tfvars

The tfvars file contain a reference to the state file they will be applied against - thereby preventing the wrong tfvars being applied to the wrong state.

## Tfvars file selection logic

There are three ways to select which tfvars file to run when you call this plugin:

1. Commit just a single tfvars file to your repo. We'll use that one.

2. Specify the tfvars file in the plugin with the `tf_vars_file` variable. We'll use that one.

3. Don't do either of the above. We'll use `staging.tfvars`.

## masterplan stage

If you run this plugin on the staging branch, you get an extra magic pipeline step called masterplan. This basically triggers a new build from this commit against production.tfvars. This will encourage you to continually deploy workign changes from staging to production.
