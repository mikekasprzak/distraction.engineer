+++
title = "Object Storage"
+++

[Object Storage](https://en.wikipedia.org/wiki/Object_storage) is the general name for [Amazon S3](https://en.wikipedia.org/wiki/Amazon_S3) style storage. Objects (files) are stored in buckets instead of in folder hierarchies, though objects can emulate folders by using slashes in their names.

Object Storage isn't as flexible as traditional file systems. There is no concept of renaming or moving files, but it can be emulated by copying and deleting the original.

Some Object Storage services support versioning of files, and automatic cleanup of older versions&mdash;you need to pay to store older versions.

Object Storage is a nice alternative to web servers, as it saves you from running and maintaining actual servers. The downside is you can only really do static things with it. There are also some quirks around DNS entries that I hope to cover in these notes.


## Services Summary
This is not a comprehensive list. I will not be using every service listed here either.

* [Linode Object Storage](https://www.linode.com/docs/products/storage/object-storage/)
  * **Killer feature**: Fastest "All NVMe" infrastructure
  * Minimum $5/mo
  * Competitive storage fees - 0.02/GB, i.e. $5 for 256 GB (versus 0.022/GB for S3)
  * Excellent transfer fees - 1 TB for free. Then 0.01/GB, i.e. $5 for 500 GB (versus 0.09/GB for S3)
  * Only available in 4 of their data centers (New Jersey, Atlanta, Frankfurt, Singapore)
  * Powered by Ceph, with S3 compatible API
  * Must pay $5/mo minimum, no free trial, though you'll only be billed for the days you're subscribed
  * Not a CDN, so no image optimizer addon available
* [BackBlaze B2](https://www.backblaze.com/b2/docs/)
  * **Killer feature**: Lowest cost for archival, Free transfer when paired with CloudFlare
  * Versioning
  * Minimum $5/mo
  * Best storage fees - 0.005/GB, i.e. $5 for 1 TB
  * Great transfer fees - 1 GB free everyday. Then 0.01/GB, i.e. $5 for 500 GB
    * Free if you use it with CloudFlare
  * Only available in 2 regions (US and EU), and each account is limited to one region
  * Proprietary API and S3 compatible API
  * Generous free plan to get started: 10 GB
  * With CloudFlare, image optimizer addon available ($20/mo)
* [Bunny.net Edge Storage](https://docs.bunny.net/reference/storage-api) and [CDN](https://docs.bunny.net/reference/bunnynet-api-overview) (formerly BunnyCDN)
  * Proprietary simplified Object Storage (not S3 like)
  * Pay as you go - i.e. fund an account, get charged (cents) for usage
  * Competitive storage fees - 0.01/GB, i.e. $5 for 500 GB. Then 0.005/GB per replication
  * Choice of replication cost - Starts at 0.005/GB, i.e. $5 per 1 TB. More costly CDN options available.
  * 5 available regions, with 3 upcoming. Replicated to all regions you choose (for a fee).
  * Proprietary API, simple but **NOT** S3 compatible
  * Recently stopped a "dark pattern" where you had to top-up your account every year else lose your credit
  * Claim better performance then competition (specifically BackBlaze), though Linode actually has this crown
  * Trial available
  * Image optimizer addon available ($9.50/mo)
* [Akamai NetStorage](https://techdocs.akamai.com/netstorage/docs)
  * **Killer feature**: "Serve from ZIP"
  * Proprietary Object Storage (not S3 like)
  * I can't talk about cost
  * Proprietary API
  * Trial available
  * Image optimizer addon available


## General thoughts
### Linode Object Storage
I'll be exploring them for image storage. I am blindly assuming the I/O of the "all NVMe" storage will be faster then other options, but this is an incomplete solution. I would have to pair it with CloudFlare or another CDN (Bunny, Akamai) to have a functional image hosting service.


### BackBlaze B2
At the moment I'm considering keeping backups on BackBlaze B2. It has potential to be more, but I'm interested in spending some time working with CEPH (Linode) for the performance and because it's open source.


### Bunny.net
Bunny.net is a complete solution (storage and CDN), and you have the option to use their CDN without their storage service. In the past I found them to be slow, their API's are proprietary, and their previous "dark pattern" didn't leave a great taste in my mouth. They could potentially still be a good service, but the space is extremely competitive. 


### Akamai
Full disclosure: We've partnered with Akamai, and we will be exploring their services for storage, CDN, security, etc.

I am genuinely excited about the potential of "Serve from ZIP" to easily add web-game support. To my knowledge there is no other drop in solution for this.


## Object Storage and CNAME workarounds
Every Object Storage system I've come across recommends using a DNS CNAME entry to customize or mask the URL. The problem is they all seem to have clumsy limitations, in contrast to CloudFlare pages where you can specify what domains to accept.


### Linode Object Storage
To use a custom domain with Linode's Object Storage, you need to name the bucket after the domain. i.e. `mywebsite.com` is both your domain and your bucket name.

This suuuuuucks! If you could rename buckets that might be less awful, but you can't. This is a problem for me since I need to support multiple aliases for the same data. I can get by with one _for now_, but it's going to bite me in the ass in a few years.


### BackBlaze B2
You can use any domain with BackBlaze because your files are stored in a subfolder (i.e. `mydomain.com/my-bucket/`). That also means you can access anybody's public B2 data with your domain (`mydomain.com/jeremys-bucket/`).

Yuck! This is a **HUGE** security venerability. If you're not especially careful, CORS and your cookies could be exploited.

On the upside, thanks to their partnership with CloudFlare, you _can_ workaround this using Transform Rules and Page Rules. A transform rule can be used to append your bucket name for all requests to a specific domain (i.e. `static.mydomain.com/image.png` becomes `static.mydomain.com/my-bucket/image.png` before the request hits your server). In theory this _should_ stop access to other users buckets, but to be sure you need at least 2 page rules: one to allow traffic to your bucket, and one to deny/redirect all other traffic. CloudFlare's free plan comes with 10 transform rules, and 3 page rules. You can buy additional page rules.

For me, I wanted to separate classes of data into different buckets. One for images, one for uploads, and (potentially) one for embedded web games. Securing my data with Page Rules, I could use 2 on the same domain before having to pay for more rules.


### Bunny.net
I am not exploring this.


### Akamai
Still researching.

