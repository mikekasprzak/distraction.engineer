+++
title = "Containers"
draft = true
+++

Containers are like Virtual Machines. VM's are an encapsulation of an entire system, where a Container is just the software. Like a VM, containers are designed to be operated standalone, but they're often run on a shared system using shared resources. For performance reasons, containers are not guaranteed to be fully isolated from each other (like VM's), but with technologies like LXD they can be.

In a nutshell, containers are often preferred because of greater performance, and dramatically faster startup times. They're not a replacement for VM's, but an evolution of the idea that modularized software doesn't always need a full dedicated OS.

Like Vagrant boxes, containers are designed to be easily spun up and disposed of, just they do it without the overhead of a full VM (i.e. Vagrant).

Container's can be managed by Orchestration software like Google's Kubernetes and Hashicorp's Nomad. These systems take the disposability of containers to heart, so be _extra careful_ in container design and "symphony" design so that important data doesn't get lost.


## OCI, the Open Container Initiative
In 2015 Docker and co established the [Open Container Initiative](https://opencontainers.org). The OCI exists to host two standards:

* The Image Specification - <https://github.com/opencontainers/image-spec/blob/main/spec.md>
* The Runtime Specification - <https://github.com/opencontainers/runtime-spec/blob/main/spec.md>

Both specs are based on the work done by Docker. I've seen "OSI containers" and "Docker support" mentioned a lot in my digging, and I suspect this is why.

### The Moby Project
[Moby](https://github.com/moby/moby) is an open source fork (?) of Docker's efforts, by Docker. I'm not super clear why this exists, but my guess is that Docker's corporate focus has shifted to enterprise clients, and this is meant to be more welcoming to non-Docker collaborators.

EDIT: Looks like Docker was acquired by Mirantis, a company that seems to have bet the farm on Kubernetes (and won). They have since renamed the Docker enterprise products to Mirantis.


## Elements of the Container Ecosystem
### Authoring Tools
Docker, Buildah (Redhat), and Buildkit (Moby) are tools that can be used to create Images. All of the tools let you create images from a `Dockerfile`, though Buildah also supports `Containerfile`'s. Yes this is analogous to Vagrant's `Vagrantfile`'s, but the `Dockerfile` and `Containerfile` formats seems a proprietary BASH-like/MAKE-like format versus Vagrant's Ruby format.


### OCI Images
A snapshot of a complete system.

The thing that makes containers fast is that they _don't_ need to waste startup time installing updates, and what makes them reliable is they all run the exact same software. You _do_ need to install updates, but that's done at the authoring (build) stage. Your orchestrator can spin up a container with the new image, and spin down containers running the old image.


### Container Runtimes
The thing that actually runs the images. This further breaks down into Runners and Managers.

#### Runners
* [runc](https://github.com/opencontainers/runc) - The standard, used by Docker, Podman, CRI-O, etc. 
* [LXC](https://linuxcontainers.org/) - Used exclusively by LXD. 


#### Managers
* [Docker Engine](https://docs.docker.com/engine/) - The (former) standard. Criticized for running as a daemon (i.e. as root).
* [Podman](https://podman.io/), by Redhat - A "daemonless" alternative to Docker Engine. Can be used as a drop-in replacement for Docker (`alias docker=podman`). Runs in user space.
* [containerd](https://containerd.io/) - The current standard, and the default in both Docker and Kubernetes. Also pitched as an abstraction layer for system calls.
  * <https://www.docker.com/blog/what-is-containerd-runtime/>
* [CRI-O](https://cri-o.io/) - Lightweight Kubernetes specific OSI runtime
* [LXD](https://linuxcontainers.org/) - Pitched as a "System" container manager.

There is a lot of conflicting information about Docker vs Podman. This comment seems to shed some light.

<https://www.reddit.com/r/docker/comments/eulimv/comment/ffs8pao/?utm_source=share&utm_medium=web2x&context=3>

In a nutshell, Podman+Buildah may exist primarily to keep customers out of "Docker Enterprise" ecosystem and _in_ the Redhat ecosystem. While Podman seems to be well supported, commentary like the above suggests it's lacking in some ways, and the past criticisms of Docker have been resolved for quite some time.

#### Application Containers vs System Containers vs Virtual Machines
LXD calls what it does managing ["System"](https://linuxcontainers.org/lxd/introduction/#application-containers-vs-system-containers) containers. System containers live between Application containers (i.e. Docker) and Virtual Machines. System containers are functionally the same as Virtual Machines when it comes to isolation, but System containers have (nearly) direct access to the kernel of the underlying system.

An alternative to LXD is [OpenVZ](https://openvz.org/), which I've seen used by web service providers. I'm not having much luck finding strong comparisons of one versus the other.

LXD isn't as popular as Docker. Most examples I've found run Docker (containerd?) inside the LXD container, which technically means you could run multiple "Application Containers" inside a single "System Container". In essence you're provisioning the System container as if it was a standalone machine. I'm not sure how I feel about that, but does strike me as slightly wasteful. 

IMO it should be possible to deploy an OCI image directly to an LXD container, but that doesn't appear to be part of LXD's feature-set. I did stumble across some efforts like [LXE](https://github.com/automaticserver/lxe), but it's still pretty niche.

It's unclear to me how problematic Application container security is. Hosting inside an LXD "System" container would only protect against non-kernel exploits.


### Orchestration
Orchestration manages multiple machines running Container Runtimes.

* Kubernetes, by Google
  * Complete solution.
  * YAML or JSON config files
* [Nomad](https://www.nomadproject.io), by Hashicorp
  * Not as popular, but more scalable.
  * Also regarded as easier to use, especially for small teams.
  * Requires other Hashicorp addons like Consul, Vault, and Terraform to make it a complete solution.
  * HCL (Hashicorp Config Language) config files, looks a bit like a mashup of YAML and JSON
* Docker Swarm - Obsolete

<https://www.imaginarycloud.com/blog/nomad-vs-kubernetes/>


## What to use
My best guesses:

* Docker CE for authoring (BuildKit and `containerd` are built-in)
* 


## TODO and more references

* <https://docs.docker.com/develop/develop-images/dockerfile_best-practices/>
* Investigate Chroot and Jails for comparison (since IMO application containers work kind-of the same way)
* <https://galeracluster.com/library/documentation/docker.html>
