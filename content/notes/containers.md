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
[Moby](https://github.com/moby/moby) is the Docker upstream project. It's where all active development happens. It exists to help eliminate confusion with all of the Docker brand products. If you're someone that wants to contribute to Docker, you contribute to the Moby project instead.

Docker Engine (AKA Docker CE) is the standard free Docker downstream product. It is based on Moby.

Mirantis (formerly Docker Enterprise) is named after the company that acquired Docker. This is also a Moby downstream product.

The rationale seems to be "projects" vs "products", development happens in a project, but you _use_ a product. You can also pay to get support for a product. Anyone can create products based on Moby, kinda like how Percona has their own database product based on MySQL and MariaDB.


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
After so much confusion, I think I've finally settled on the following.

* Docker CE for authoring (BuildKit is built-in)
* Docker CE for deployments (`containerd` is used under the hood)
* An orchestrator: I'm leaning towards Nomad myself, but Kubernetes seems a perfectly reasonable choice
  * Nomad documentations suggests Docker by default (i.e. `containerd` wrapped by docker)
    * Roblox provides an optional direct `containerd` plugin
  * Kubernetes doesn't depend on Docker anymore and uses `containerd` directly

The documentation I've read suggests BuildKit needs to be specifically enabled. It's been suggested that BuildKit exposes the security changes that don't require containers be run with root privileges, but this might not be accurate anymore.

Moby is the upstream product, not exactly like Chrome or Firefox "Nightly", but it's not a supported or "guaranteed stable" product. Docker CE isn't an LTS product either, but it seems the most stable, widely used choice.

Much of my confusion came from Podman and the debate about Docker running as a daemon. Hearing that the key criticisms that lead to Podman may not be a problem anymore, it makes me more comfortable considering the "industry standard". I should still deep-dive and figure out _how_ to ensure containers aren't unnecessarily running with elevated permissions though.


## How containers (actually) work
{{ youtube(id="8fi7uSYlOdc") }}


## Docker Setup
### On Windows
Docker Desktop is a paid application for Windows and Mac.

Alternatively, Docker CE can be run freely on WSL2 (the Windows Subsystem for Linux v2).

<https://dev.to/felipecrs/simply-run-docker-on-wsl2-3o8>

IMPORTANT: Docker does not start automatically when Windows starts. It will not start until the service is started. As mentioned in the link above, you can utilize startup scripts or your Windows 11 WSL configuration file to auto-start the service. You still have to open a WSL window, but chances are you need one to do your work anyway.

## Docker Security
Docker containers effectively have full access to the machine they run on, but through the use of Linux Namespaces they can be restricted.

* <https://www.linux.com/news/understanding-and-securing-linux-namespaces/>
* <https://docs.docker.com/engine/security/userns-remap/>

### Linux Users
Users on Linux have a user id or `uid`. The `root` user is always `0`, and any process run by `root` is considered a privileged process. Unprivileged Processes are run by users, and a user may require special permissions or "capabilities" to make certain system calls. Privileges are given to users and groups, and users can be members of multiple groups.

```bash
id username
# output: username's uid, gid, and member groups

id
# output: current user's uid, gid, and member groups
```

 <https://medium.com/devops-dudes/docker-under-the-hood-user-space-kernel-syscalls-permissions-setuid-setgid-and-capabilities-3a5c58584b4f>

`setuid` is a curious feature of Linux file systems. It replaces the `x` in file permissions with an `s`. What it does is it makes executables run as the owner of the file, rather then the current user.

```bash
chmod +s my-executable
```

This is used by `ping` because it requires additional permissions to perform network operations.

`setgid` is similar, but it applies to the group instead of the user. This used by `crontab`.

The article above provides an example that fiddles with the ping command. I've summarized it below.

```bash
ping "google.com"

getcap /bin/ping
# output: /bin/ping cap_net_raw=ep

cp /bin/ping ./my-ping

# not supposed to work, but it does (is it because I'm a sudoer?)
./my-ping "google.com"

# will work
sudo ./my-ping "google.com"

getcap ./my-ping
# output:

sudo setcap "cap_net_raw+p" ./my-ping

getcap ./my-ping
# output: my-ping cap_net_raw=p
```



## TODO and more references

* <https://docs.docker.com/develop/develop-images/dockerfile_best-practices/>
* Investigate Chroot and Jails for comparison (since IMO application containers work kind-of the same way)
* <https://galeracluster.com/library/documentation/docker.html>
* docker can use PASS to store credentials, which uses PGP to encrypt them. Super interesting <https://www.passwordstore.org/>