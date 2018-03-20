+++
title = "Service Brokers and the OSBAPI Spec"
date = "2018-03-20T20:08:05Z"
draft = true
tags = "on-demand-services"

+++

One of the advantages of using Cloud Foundry as your Platform as a Service of
choice is the existence of a marketplace of services, like MySQL or Redis, that
you can pick, play and extend as you need.

To make new services available in the marketplace, the service author needs to
integrate the service by way of APIs, usually by developing a **Service
Broker**. A Service Broker, or just Broker, is a component that glues together
your service and the platform and allows the App Developer to request Service
Instances (SI) and credentials on demand. On CF, for example, that would be
done with the `cf create-service` and `cf bind-service` commands.

The [Open Service Broker API
Spec](https://github.com/openservicebrokerapi/servicebroker/blob/v2.13/spec.md)
defines that, an agent (usually the platform marketplace) interacting with the
broker should be able to execute the following actions: list of available
services, provision a Service Instance, update an SI, deprovision an SI, query
the Last Operation performed for a particular SI, create a Service Binding and
delete a Service Binding. In order to do that, the following API endpoints
should be implemented by the broker:

1. `GET    /v2/catalog`
1. `PUT    /v2/service_instances/:instance_id`
1. `PATCH  /v2/service_instances/:instance_id`
1. `DELETE /v2/service_instances/:instance_id`
1. `GET    /v2/service_instances/:instance_id/last_operation`
1. `PUT    /v2/service_instances/:instance_id/service_bindings/:binding_id`
1. `DELETE /v2/service_instances/:instance_id/service_bindings/:binding_id`

Note that, by writing a service broker that speaks OSBAPI, you will
automatically write a broker that works both with Cloud Foundry and Kubernetes,
as both Cloud Foundry Marketplace and Kubernetes the [Service
Catalog](https://kubernetes.io/docs/concepts/service-catalog/) interact
with brokers through the OSBAPI specification.

To illustrate, imagine that we are developing a Service Broker for a MySQL
service. The Catalog could return that the broker can create MySQL service
instances, and would return the plans available for the service. Services Plans
are usually used to decide resources allocation and costs. For our MySQL
service, a possible plan would be "500MB" or "1GB", which would allocate 500MB
or 1GB of disk for that particular instance.

The Provision endpoint could create a database in an existent MySQL instance.
The Binding endpoint would then create a user with permissions on this
database. The unbind endpoint would destroy this user, and the Deprovision
endpoint would destroy the particular database.

By writing a broker as the example, we would only support a "multi-tenant"
approach, that is, multiple applications sharing a single service instance.
Sometimes that's exactly what we need, for example when developing the
application -- we don't really care if other developers are using the same
instance at a given point. However, for critical applications, one would prefer
to have a dedicated instance rather than sharing one.

To have that, on provision your broker could, instead of creating a database,
deploy a brand new MySQL cluster, and then, on binding, create the database and
user. 

The [On Demand Service
Broker](https://github.com/pivotal-cf/on-demand-service-broker-release) is an
open source project that aims to reduce the overhead of writing brokers that do
exactly that: deploy new service instances on demand. On this series of posts,
we'll go through what is needed to create our own broker using BOSH, ODB and CF.
Stay tuned!
