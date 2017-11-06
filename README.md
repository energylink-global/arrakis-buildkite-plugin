# Arrakis Buildkite Plugin

(c) COzero 2017

MIT (see [LICENCE](LICENCE))

![Arrakis](arrakis.jpg)

## Synopsis

Dynamically generates a buildkite pipeline to test and deploy your terraform infrastructure.

## Usage

To use, simply create your pipline.yml as follows

```
# pipeline.yml
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

## FAQ

### Oh man this is great

Technically that's not a question, but thanks anyway
