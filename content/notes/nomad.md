+++
title = "Hashicorp Nomad"
draft = true
+++

## Requirements
<https://www.nomadproject.io/docs/install/production/requirements>

The server requirements according to the docs seem pretty hefty.

#### Servers
3-5 machines with these specs:

* 4-8 Cores
* 16-32 GB of RAM
* 40-80 GB of fast disk space
* Lots of internet bandwidth
* 10ms ping between servers
* userspace

Again, 3-5 of them.


#### Clients
* 100ms ping
* `root`

### Deployment
<https://learn.hashicorp.com/tutorials/nomad/production-deployment-guide-vm-with-consul>

* <https://learn.hashicorp.com/tutorials/nomad/outage-recovery?in=nomad/manage-clusters>

### Raft Concensus

* Great animated slideshow that describes raft, the replication <http://thesecretlivesofdata.com/raft/>
* https://raft.github.io/


## Other Resources

* <https://www.nomadproject.io/docs>
* <https://learn.hashicorp.com/tutorials/nomad/clustering?in=nomad/manage-clusters>
* <https://github.com/hashicorp/nomad-guides/tree/master/application-deployment>
* <https://srivastavaankita080.medium.com/practical-hashicorp-nomad-and-consul-part-4-consul-kv-store-ef837e0e4ffc>
* <https://www.nomadproject.io/docs/job-specification/template>