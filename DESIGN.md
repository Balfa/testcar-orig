-# **Epic(s)**
#139610

[[_TOC_]]


# Design
TODO: This whole section
Drill into the specific design areas via the following sub-pages
* [Overview](CONCEPTS.md)
* [Data Operation Service](/Reference-Architecture/Platforms-and-Distributed-Systems/Data-Operation-Framework/Design-|-Concepts/Data-Operation-Service)
* [Cache Service](/Reference-Architecture/Platforms-and-Distributed-Systems/Data-Operation-Framework/Design-|-Concepts/Cache-Service)
* [Sproc Workflow](/Reference-Architecture/Platforms-and-Distributed-Systems/Data-Operation-Framework/Design-|-Concepts/SPROC-Workflow)
* [Data Op Manager](/Reference-Architecture/Platforms-and-Distributed-Systems/Data-Operation-Framework/Design-|-Concepts/Data-Op-Manager)
* [DOF K8 Operator](/Reference-Architecture/Platforms-and-Distributed-Systems/Data-Operation-Framework/Design-|-Concepts/DOF-K8-Operator)

## Tech Stack
Testcar is built on top of several technologies, some are core components that allow the system to work, while others are additive that could be replaced.

### Docker
Testcar is packaged into Docker run time image(s) for simple self containment, dependency gathering, and ease of deployment.

### Kubernetes
Testcar is designed as a Kubernetes sidecar. While it could theoretically run without K8s, doing so would call into question the reason Testcar exists.

### Artillery
Although it's hoped in the future it may support multiple integration test frameworks, Testcar is being built from the ground up as an enabler of Artillery tests. Modularization will come later, if at all.

### Shell script
Being a glue product between containers, Artillery, and authentication schemes, Testcar's most significant logic will be implemented as shell scripts.

### Testcar-ADO
Node.js and TypeScript are first class citizens when it comes to writing custom ADO pipeline tasks. As such, they will be used for this component.


## Authentication / Authorization
Testcar will be required to test its System Under Test (SUT) container as a black box. It needs to be able make HTTP requests using whatever authorization mechanism the SUT container's clients typically use. This lends itself strongly to an authorization-as-a-module approach, with the initial model being to use Kerberos authorizaiton against a token service to gain an OAuth token, and then using the OAuth token to make requests its SUT.

# Infrastructure
Testcar is intended to be hosted as a sidecar container inside the K8s pod of its SUT. Being an ancillary application, Testcar's needs should not significantly impact the choice of capabilities of the K8s cluster.

# DevOps
## Availability
Since this is a development tool and not within the customer's user flow, high availability is not as important. The fail-safe is that a newly deployed version of the SUT is not flagged as good. The existing SUT version will be unaffected and continue serving requests. In the event of a prolonged outage, manual execution of integration tests against a SUT deployment should be considered.

## Disaster Recovery
Testcar itself has no DR system in place, since there is no durable storage or recovery of data needed. Testcar only operates at time of deployment of its SUT, so any DR efforts to deploy the SUT to another cluster will result in Testcar being auto-injected and activated.

## Build/Deployment
//TODO

# Security
TODO: [Threat Model](/TODO)