---
layout: page
title: mcr-ccs
description: Project for Cloud Computing Security tackling a hybrid cloud scenario
img: assets/img/mcr-ccs/mcr-ccs-logo.png
importance: 1
category: security
---

## Description 
For this project, the goal was to explore different cloud services and create a suitable setup for the requirements that we were giving.
We were tasked with creating a hybrid cloud setup with two cloud providers of our choice. One provider would host a database and the other would host a webservice, where some information from the database could be displayed.
Both providers should be connected via a site-to-site VPN which would be hardened to only allow the necessary web traffic.
Additionally, integrations with CI/CD and optionally IaC should also be setup.

## Overview
I choose to go the IaaS route, in order to have maximal control and learning experience. For the two providers I opted for Digital Ocean and Microsoft Azure.
The following diagram shows the setup I have created.

<div class="row">
    <div class="col-sm mt-2 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/mcr-ccs/mcr-ccs-diagram.png" title="mcr-ccs-diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
   Architectural overview of hybrid cloud setup. 
</div>

## Result
I was able to achieve the setup I had in mind, where I had full control over the cloud resources and the way I chose to setup the integrations.
I learned a lot about new technologies that I was previously unfamiliar with, such as NGINX, Ansible and Wireguard. Although this was certainly not the simplest approach for the given requirements, I was happy with the progress I made.

## References
- GitHub repository: [mcr-ccs](https://github.com/cadeke/mcr-ccs)
- [Digital Ocean](https://cloud.digitalocean.com/)
- [Microsoft Azure](https://azure.microsoft.com/)
