+++
title = "The Anatomy of the On-Demand Service Broker"
date = "2018-05-12T15:12:34+01:00"
draft = false
tags = ["on-demand-service-broker", "bosh", "cloud-foundry"]
+++

As seen [here]({{< relref "blog/odb-part-I.md" >}}), it is relatively
straightforward to write your own service broker. However, to make it create
instances on-demand requires considerably more effort, as the broker would need
to deploy a brand new service instance on request.

The [on-demand service broker
release](https://github.com/pivotal-cf/on-demand-service-broker-release) is an
open-source project that, by leveraging [BOSH](https://bosh.io), simplifies the
process of writing on-demand service brokers. In a nutshell, ODB frees the
Service Author of the responsibility of respecting the OSBAPI Spec and handling
HTTP requests, allowing them to focus on the behaviour that is specific for
their services.

The ODB release packages the [On-demand service
broker](https://github.com/pivotal-cf/on-demand-service-broker), a generic
on-demand broker that is designed to work in with BOSH, and the
[brokerapi](https://github.com/pivotal-cf/brokerapi), a Golang implementation of
the v2 Open Service Broker API. To use the on-demand broker, the Service Author
needs to build what is called the Service Adapter. As ODB uses BOSH behind the
scenes, it's also necessary to have a BOSH release for the desired service.

## The Service Adapter

The Service Adapter is an executable that's invoked by ODB and should implement
the following subcommands:

1. `generate-manifest`: responsible for generating and outputting to the stdout
   a BOSH manifest for the service instance being deployed (or upgraded).
1. `dashboard-url`: generates an URL for a web-based management interface for the
   service instance. Optional. 
1. `generate-plan-schema`: generates a JSON schema to validate user-provided
   parameters (see
   [here](https://github.com/openservicebrokerapi/servicebroker/blob/v2.13/spec.md#schema-object)).
   Optional.
1. `create-binding`: outputs a JSON representing a binding. Usually goes to the
   service instance and create a set of credentials that will be returned.
1. `delete-binding`: deletes an existing binding, usually by invalidating or
   deleting users/credentials.

All subcommands must be implemented, even when the output is options (e.g.
`dashboard-url`). ODB will inspect the exit code to determine the success of the
invocation using the following table:

| **Exit Code** | **Status**                  |
| ------------: | :-------------------------- |
|             0 | Success                     |
|            10 | Subcommand not implemented  |
| non-zero code | Failure                     |

When exiting with a non-zero status code, the on-demand broker will send the
contents of stdout back to the client, and log both stdout and stderr for the
BOSH operator. For that reason, service adapter authors should not log
sensitive data.

If building the Service Adapter in Golang, one can use the [on-demand services
SDK](https://github.com/pivotal-cf/on-demand-services-sdk) to speed up even more
the development process. The SDK allows the Service Author to focus on
implementing the logic behind the subcommands, and will handle the command line
invocation, parsing the input parameters, serializing the response and handling
error cases. 

In the next post, we'll go deeper into the subcommands and start to write our own
Service Adapter. See you soon!
