# 757dev-cluster

## Overview

The 757 dev cluster aims to provide a place for users new or knowledgable with kubernetes to interact with a local cluster that can present common services and serve as a place to deploy and serve content from containerized applications. Users familiar with kubernetes may even go as far as to volunteer machines as “nodes” for other kubernetes users to deploy applications against. Essentially, it’s a “private” multi tenant cluster. Expect nodes to go down at any time, but also expect a form of HA via essential services such as ingress, authentication and remote container builders. Everyone from users to administrators can aim to gain practice from this cluster and it’s workloads. Expect a variety of different nodes from capabilities to container runtimes.

![Cluster](https://raw.github.com/sfxworks/757dev-cluster/master/757dev-cluster.svg?sanitize=true)

| Feature            | Status | Quota | Notes                                                                                                                                    |
| :----------------- | :----- | :---- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| Deployments        | G2G    | 10    | Ask for more                                                                                                                             |
| Pods               | G2G    | 20    | Ask for more                                                                                                                             |
| Namespace          | G2G    | 1     | Can’t have NS admin (usually not needed for new users)                                                                                   |
| Statefulset        | TBA    | NA    | Correlates with PVC                                                                                                                      |
| Persistent Volumes | TBA    | NA    | Would need volunteers to dedicate storage and a gameplan to keep  latency low                                                            |
| Ingress            | TBA    | 10    | Plans are to work with cloudflare argo or have a GCP central load balancer hooked with a NodePort + an internal nginx ingress controller |
| Load Balancer      | No     | NA    | Only 1 IP is referenced for ingress. If you need layer 4 routing, ask.                                                                   |
| Container Builder  | TBA    | NA    | Using a rootless buildkit daemon on the cluster.                                                                                         |

## Quotas and Limitations
While most of the above can be adjusted, some standards that should be noted are the lack of permission for creating hostPath volumes or containers with root privileges. Details TBA. 


[Joining a Node][https://github.com/sfxworks/757dev-cluster/blob/master/docs/joining-a-node.md]
[User Interract][https://github.com/sfxworks/757dev-cluster/blob/master/docs/user-interraction.md]


## TODO
- Automatically create a namespace when a user registers at keycloak
- Integrate cloudflare argo for ingress
- Figure out or allocate resources for stateful workloads
- Implement buildkit daemon for remote image builds with tutorials
