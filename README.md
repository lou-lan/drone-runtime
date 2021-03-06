The drone runtime package implements the execution model for container-based pipelines. It is effectively a lightweight container orchestration engine optimized for pipelines.

## Definition File

The runtime package accepts a pipeline definition file as input, which is a simple json file. This file is not intended to be read or written by humans. It is considered an intermediate representation, and should be generated by a computer program from more user-friendly formats such a yaml.

Example hello world definition file:

```json
{
	"metadata": {
		"uid": "uid_AOTCIPBf3XdTFs2j",
		"namespace": "ns_JVzesGoyteu5koZK",
		"name": "test_hello_world"
	},
	"steps": [
		{
			"metadata": {
				"uid": "uid_8a7IJsL9zSJCCchd",
				"namespace": "ns_JVzesGoyteu5koZK",
				"name": "greetings"
			},
			"docker": {
				"args": [
					"-c",
					"echo hello world"
				],
				"command": [
					"/bin/sh"
				],
				"image": "alpine:3.6",
				"pull_policy": "default"
			}
		}
	],
	"docker": {}
}
```

## Local Testing

The runtime package includes a simple command line utility allowing you to test pipeline execution locally. You should use this for local development and testing.

The runtime package includes sample definition files that you can safely execute on any machine with Docker installed. These sample files should be used for research and testing purposes.

Example commands:

```text
drone-runtime samples/1_hello_world.json
drone-runtime samples/2_on_success.json
drone-runtime samples/3_on_failure.json
drone-runtime samples/4_volume_host.json
drone-runtime samples/5_volume_temp.json
drone-runtime samples/6_redis.json
drone-runtime samples/7_redis_multi.json
drone-runtime samples/8_postgres.json
drone-runtime samples/9_working_dir.json
drone-runtime samples/10_docker.json
```

Example command tests docker login:

```text
drone-runtime --config=path/to/config.json samples/11_requires_auth.json
```

## Runtime Engines

The default runtime engine targets Docker, but can be extended using plugins. The plugin loader expects your custom engine is exposed using the following convention:

```text
package main

import "github.com/drone/drone-runtime/engine"

func Engine() (engine.Engine, error) {
  // return your custom engine implementation
}
```

Test your plugin using the command line utility:

```text
drone-runtime --plugin=custom/plugin.so samples/1_hello_world.json
```
