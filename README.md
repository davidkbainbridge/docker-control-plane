docker-control-plane
====================

The purpose of this project is to provide control plan capabilities around the docker remote API that can opperate in a distributed manner over multiple physical / virtual hosts as long as they are running the docker-control-plane agent.

Basic Concepts
==============

Docker Daemon
-------------
This is the standard docker daemon that will be required on each host.

Docker Image / Container Repository
-----------------------------------
Standard docker capabilities. The repository should be accessible to each control plane agent.

Control Plane Agent
-------------------
The basic concept is to have a small agent deployed on each "host" that acts as the contact point for the distributed control plane. This agent provides local control over any docker containers running on the host.

These agents will allow the control plane to monitor and influence the behavior of the hosts as well as let the hosts send events to the control plane for processing in the policy base.

The idea is to have these agents act as a collective hive that can invoke docker containers based on policy over an entire deployment.

__How will this be different from a standard docker daemon? If we can get away with just using the standard docker daemon life would be better.__

__The agent should just be another docker container definition. This might help only have to require the docker remote API on each host, as we could start the agent remotely if we need an agent.__

API
---
The main input into the distribtued control plane is a REST based API. This API virtualizes the collective hive of docker agents and abstracts specifics of where a container is executed away from the caller.

The API will also expose (with proper authentication) the underlying operational and configuration information models (data).

Presentation
------------
A web based presentation will be provided. This presentation layer will leverage the API for all capabilities.

Interfaces
----------
A suite of interfaces will be defined to allow the control plane to monitor running containers. If these interfaces are implemented within a container's processes then the control plane can received and send information that may be valuable to the processes within a container. The values (events) received from a container's processes will be processed by the control plane's policies.

Policy
------
Control plane decisions will be influenced by a policy base. This policy base will determine actions based on events and control messages recieved from control plane agents, container instances, and the API.

This is essentially a rule base and should be "live" in that there is a state associated with it and that state changes over time as additional data is added to the knowledge base. Based on the data, actions may be triggered.

