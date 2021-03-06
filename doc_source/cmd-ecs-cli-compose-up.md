# ecs\-cli compose up<a name="cmd-ecs-cli-compose-up"></a>

## Description<a name="cmd-ecs-cli-compose-up-description"></a>

If an Amazon ECS task definition does not already exist, creates one from your compose file and runs one instance of that task on your cluster\.

**Important**  
Some features described may only be available with the latest version of the ECS CLI\. To obtain the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-compose-up-syntax"></a>

**ecs\-cli compose \[\-\-verbose\] \[\-\-file *compose\-file*\] \[\-\-project\-name *project\-name*\] up \[*arguments*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-compose-up-options"></a>


| Name | Description | 
| --- | --- | 
|  `--verbose, --debug`  |  Increases the verbosity of command output to aid in diagnostics\. Required: No  | 
|  `--file, -f compose-file`  |  Specifies the Docker compose file to use\. At this time, the latest version of the Amazon ECS CLI supports [Docker compose file syntax](https://docs.docker.com/compose/compose-file/#versioning) versions 1 and 2\. If the `COMPOSE_FILE` environment variable is set when ecs\-cli compose is run, then the Docker compose file is set to the value of that environment variable\. Type: String Default: `./docker-compose.yml` Required: No  | 
|  `--project-name, -p project-name`  |  Specifies the project name to use\. If the `COMPOSE_PROJECT_NAME` environment variable is set when ecs\-cli compose is run, then the project name is set to the value of that environment variable\. Type: String Default: The current directory name\. Required: No  | 
|  `--task-role-arn role_value`  |  Specifies the short name or full Amazon Resource Name \(ARN\) of the IAM role that containers in this task can assume\. All containers in this task are granted the permissions that are specified in this role\. Type: String Required: No  | 
|  `--ecs-params`  |  Specifies the ECS parameters that are not native to Docker compose files\. For more information, see [Using Amazon ECS Parameters](cmd-ecs-cli-compose.md#cmd-ecs-cli-compose-ecsparams)\. Default: `./ecs-params.yml` Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--region, -r region`  |  Specifies the AWS region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
| \-\-launch\-type launch\_type | Specifies the launch type to use\. Available options are FARGATE or EC2\. For more information about launch types, see [Amazon ECS Launch Types](launch_types.md)\. This overrides the default launch type stored in your cluster configuration\. Type: StringRequired: No | 
|  `--create-log-groups`  |  Creates the CloudWatch log groups specified in your compose file\(s\)\. Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-compose-up-examples"></a>

### Register a Task Definition Using the AWS Fargate Launch Type with Task Networking<a name="cmd-ecs-cli-compose-up-example-1"></a>

This example creates a task definition with the project name `hello-world` from the `hello-world.yml` compose file\. Additional ECS parameters are specified for task and network configuration for the Fargate launch type\. Then one instance of the task is run using the Fargate launch type\.

Example ECS params file:

```
version: 1
task_definition:
  ecs_network_mode: awsvpc
  task_execution_role: ecsTaskExecutionRole
  task_size:
    cpu_limit: 512
    mem_limit: 2GB
  services:
    my_service:
      essential: false

run_params:
  network_configuration:
    awsvpc_configuration:
      subnets:
        - subnet-abcd1234
        - subnet-dcba4321
      security_groups:
        - sg-abcd1234
        - sg-dcba4321
      assign_public_ip: ENABLED
```

Command:

```
ecs-cli compose --project-name hello-world --file hello-world.yml --ecs-params ecs-params.yml up --launch-type FARGATE
```

Output:

```
INFO[0000] Using ECS task definition                     TaskDefinition=ecscompose-hello-world:5
```