---
layout: page
title: argo
description: Microservice application simulating DNS-functionalities as model use-case for my master thessis
img: assets/img/argo/argo-1.png
importance: 1
category: security
---

## Introduction

#### About the thesis

A master thesis typically serves as the final assignment of a master's degree, aiming at challenging students in a relevant domain of research. In my case, the last semester of my degree was dedicated to the thesis, presenting us with topics from all areas of information technology security.

#### Situating the topic

My topic is related to the domain of infrastructure and networking, more specifically focussing on on-premises container orchestration platforms. This is a relevant area of research since it is closely related to cloud computing deployments, which has been a prominent movement in the last years. Whether you leverage the cloud in a private, public or hybrid context, it is crucial to architect your workloads efficiently in order to reap the most benefits for the lowest cost.

#### Problem statement

My thesis aimed at finding suitable alternatives for Kubernetes for orchestration containerized workloads in a self-hosted production-grade environment. Kubernetes' popularity is undeniable, but so is the complexity associated with it, having a notoriously high learning curve. I wanted to research if other technologies could pose as a better solution for a use-case that specifically targets SME teams in charge of managing their own deployments. Through the practical evaluation of different orchestration candidates, I attempted to deliver insights in the technology landscape and in the domain of container orchestration in general.

## Preparation

#### Literature review

During the review of the state of the art, I analyzed different publications in the domains of Kubernetes complexity, managed Kubernetes cloud services and comparisons between different orchestration technologies. This gave me insights that I could use to design my use-case and select relevant technologies as testing candidates during the practical implementation. 

#### Design of use-case

I motivated the use-case with various technical and non-technical requirements. The application itself, which was called "ARGO", was designed as a microservice architecture in order to be a suitable candidate for a containerized deployment. The functionality was inspired by DNS, with some simplifications in terms of functionality and data model. The following image shows the high-level architecture of the "ARGO" application, with each purple rectangle representing a service consisting out of one or more containers.

<div class="row">
    <div class="col-sm mt-2 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/argo/argo-2.png" title="argo architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
   High-level overview of ARGO microservice architecture
</div>

#### Selection of technologies

With the insights gained from the literature review, I made a selection of orchestration features which were relevant for my use-case. These features formed the baseline criteria in order to make a selection of orchestration candidates. Some technologies that were considered include Red Hat OpenShift, k3s, k0s, Apache Mesos, HashiCorp Nomad and Docker Swarm. After further review, Nomad and Swarm were selected as candidates for the practical implementation, because of their relevance to the use-case and the amount and type of research done on these technologies.

#### Evaluation criteria

Prior to implementation the "ARGO" application, I revisited the criteria which I wanted to investigate, in order to have a structure review process. Criteria I wanted to analyze include the scheduling of containers, scaling capabilities, service discovery, fault tolerance during container or node failure, HA during application updates, network segmentation, shared storage between nodes, load balancing, encrypted internal communications and centralized secret management.

Additionally, I introduced a simple grading schema in order to quantify the findings in the final stages of the thesis. This formula included two factors, one for the documentation support and one for the implementation experience, with an additional weighing factor. This allowed me to give a final grade for each tested candidate.

## Implementation

#### Testing environment

The infrastructure used for the practical part of the thesis was provided by the university, in the form of eight VMs. Through the use of snapshots and IaC, I managed the configurations for both Swarm and Nomad, in such a way I could roll back to a stable state whenever necessary.

#### Base implementation

As a first step of the practical implementation, I developed the system and ran it locally using Docker Compose. This served as the base implementation from which I could make the necessary adjustments for the following orchestration candidates. The following snippet shows the entire stack running locally with Docker Compose.

```bash
> docker compose up -d
[+] Running 16/16
 ✔ Network argo_argo-backend       Created        0.1s 
 ✔ Container postgres              Started        0.4s 
 ✔ Container memcached             Started        0.4s 
 ✔ Container gitlab                Started        0.5s 
 ✔ Container admin-api             Started        0.5s 
 ✔ Container query-api             Started        0.5s 
 ✔ Container argo-gitlab-runner-2  Started        0.5s 
 ✔ Container argo-gitlab-runner-1  Started        0.7s 
 ✔ Container admin-site            Started        0.6s 
 ✔ Container prometheus            Started        0.7s 
 ✔ Container ot-app                Started        0.7s 
 ✔ Container query-site            Started        0.7s 
 ✔ Container cadvisor              Started        0.9s 
 ✔ Container grafana               Started        0.9s 
 ✔ Container postgres-exporter     Started        0.8s 
 ✔ Container node-exporter         Started        0.9s
```

<div class="caption">
   Base implementation on local system using Docker Compose
</div>

#### Swarm implementation

The implementation on Swarm went quite smoothly. The documentation was clear and included useable examples in order to get up and running quickly. A big advantage was that all necessary software components were already included by default into the Docker Engine, so there was no need for additional installation or configuration. Just some small steps to establish the cluster was all that was needed in order to start deployments.

As an example, the following snippet shows the entire stack running on Swarm, with some components being scaled to multiple instances, and others publishing their ports in Swarm's routing mesh.

```bash
student@vm01:~/stacks> docker service ls
ID             NAME                     MODE         REPLICAS   IMAGE                                PORTS
0bkfxgwyaxed   argo_admin-api           replicated   1/1        cadeke/argo-a-api:latest             *:8081->8080/tcp
o43bwx6rf9dt   argo_admin-site          replicated   1/1        cadeke/argo-a-site:latest            *:81->80/tcp
shffjtuqu40f   argo_cadvisor            global       8/8        gcr.io/cadvisor/cadvisor:latest      
w62isxkxn5q7   argo_grafana             replicated   1/1        grafana/grafana:latest               *:3000->3000/tcp
swlbridujx5e   argo_memcached           replicated   1/1        memcached:latest                     
5jumc3jhs4aw   argo_node-exporter       global       8/8        prom/node-exporter:latest            
fki27vumojaw   argo_ot-app              replicated   2/2        cadeke/argo-ot-app:v1.1              
q1af73patsks   argo_postgres            replicated   1/1        postgres:latest                      
ver1jegnvzqe   argo_postgres-exporter   replicated   1/1        wrouesnel/postgres_exporter:latest   
f8ww90gzdqhu   argo_prometheus          replicated   1/1        prom/prometheus:latest               
gbe3ibw9pqdx   argo_query-api           replicated   1/1        cadeke/argo-q-api:latest             *:8080->8080/tcp
kpkxqn0bgjbo   argo_query-site          replicated   1/1        cadeke/argo-q-site:latest            *:80->80/tcp
```

<div class="caption">
   Overview of services running in Swarm
</div>

#### Nomad implementation

Nomad required more initial configuration in order to establish a cluster. I also configured Consul as a dedicated service discovery solution, which made the entire stack more complex. Then, I was able to make deployments and test the different aspects, just as I did with Swarm.

As an example, the following snippet shows a successful application update in the Nomad cluster.

```bash
student@vm01:~/jobs> nomad status ot
# output omitted

Latest Deployment
ID          = 15534c3f
Status      = successful
Description = Deployment completed successfully

Deployed
Task Group  Auto Revert  Desired  Placed  Healthy  Unhealthy  Progress Deadline
ot-app      true         4        4       4        0          2025-04-15T18:38:44Z

Allocations
ID        Node ID   Task Group  Version  Desired  Status    Created     Modified
63aa90f6  d56fe237  ot-app      3        run      running   6m47s ago   6m36s ago
f4505f4d  d56fe237  ot-app      2        stop     failed    11m2s ago   6m47s ago
2695c2f6  fd7ca40d  ot-app      2        stop     failed    13m46s ago  11m2s ago
35675af4  6f594192  ot-app      2        stop     failed    15m38s ago  13m46s ago
5ea7f5be  3919c96c  ot-app      2        stop     failed    16m47s ago  15m38s ago
f4b35a2e  fd7ca40d  ot-app      3        run      running   18m9s ago   6m37s ago
4885fb67  d216928d  ot-app      3        run      running   18m26s ago  6m36s ago
1736e9f6  d56fe237  ot-app      3        run      running   18m42s ago  6m36s ago
d4ab2533  3919c96c  ot-app      1        stop     complete  18m55s ago  16m47s ago
c5b981ff  3919c96c  ot-app      0        stop     complete  19m31s ago  18m55s ago
e5545db3  d56fe237  ot-app      0        stop     complete  19m31s ago  18m42s ago
b4e1bc2d  fd7ca40d  ot-app      0        stop     complete  19m31s ago  18m9s ago
87308e66  d216928d  ot-app      0        stop     complete  19m31s ago  18m25s ago
```
<div class="caption">
   Overview of application update to a service running in Nomad
</div>

## Results

#### Conclusion

As a conclusion, I found that Swarm aligned more closely with the use-case's requirements, especially since it was quite straight forward to get up and running. Nomad felt more suited for medium to large deployments, allowing for more customization at the cost of an increased complexity.

#### Next steps

The insights gained from this research can be used in various ways. Technical leads can evaluate their orchestration needs and pick one of the solution I have covered as a starting point. They can also use certain parts of my use-case, or some of my experiences, for their own applications. Since I opted for a microservice architecture, all components are modular and can be swapped if this is required (e.g. for a standardized technology choice or similar reasons). Finally, organization can use the findings to better understand container orchestration in general, and prepare their developers and application in a more appropriate way, which would have a larger chance of success.

## References
- GitHub repo with all relevant documents, code and artifacts can be found [here](https://github.com/cadeke/argo)
- Full thesis can be found [here](https://github.com/cadeke/argo/blob/main/docs/thesis.pdf)
- Short paper can be found [here](https://github.com/cadeke/argo/blob/main/docs/paper.pdf)
