+++
title = "Linux Security"
draft = true
+++

Contrary to the title this isn't Linux specific security, but it will be Linux skewed.


## PGP and GnuPG
GnuPG 2.3 was released in 2021, quite recently. Even the GnuPG 2.2 changes was quite recent (2017). It does make me curious why things were so aggressively changed.

```bash
gpg -k
# output: list of all public (pub) keys

gpg -K
# output: list of all private (sec) keys

gpg --list-signatures
# output: lists keys and signatures. similar to the public keys output, but with more data

gpg --check-signatures
# output: similar to --list-signatures, but it validates the signatures too

gpg --fingerprint
# output: similar to -k, but the data is formatted slightly differently?
```

#### `libgcrypt`
With GnuPG 2.x, the cryptography libraries used GnuPG were isolated and made into the standalone library `libgcrypt`.

#### Elliptical curve public keys
RSA seems to be the standard, but Elliptical curve keys are also supported. EC requires less data, but there seems to be a fear that EC is more solvable by quantum computing attacks. I guess that means stick with RSA keys, until a quantum resistant algorithm is introduced.

### Structure
<https://www.gnupg.org/documentation/manuals/gnupg/GPG-Configuration.html>

On Linux, the `~/.gnupg/` folder contains files of interest. Like `~/.ssh/` the folder it is `rwx` by only the user, public files are read only, private files are read/write only by the user.

#### `~/.gnupg/pubring.kbx` (formerly `pubring.gpg`)
The public keyring containing public keys.

**BACKUP THIS FILE**

In case of emergency, you _may_ have copies of this available elsewhere (GitHub), since they are for public use.

#### `~/.gnupg/private-keys-v1.d/` (formerly `secring.gpg`)
Folder containing the individual private key files (secrets).

**BACKUP THESE FILES**

#### `~/.gnupg/trustdb.gpg`
The local trust database for PGP's [web of trust](https://en.wikipedia.org/wiki/Web_of_trust).

<https://www.phildev.net/pgp/gpgtrust.html>

**YOU SHOULD EXPORT THIS FILE, BECAUSE SOME DATA CAN'T BE RECONSTRUCTED FROM A COPY**

```bash
gpg --export-ownertrust > trust-gpg.txt
```

#### `~/.gnupg/random_seed`
Used to preserve the state of the internal random pool. It appears this is only for performance. `--no-random-seed-file` can be used to not generate the file, at the cost of longer generation times.

#### `~/.gnupg/.#lk???.hostname.1234`
These are lock files.

<https://unix.stackexchange.com/a/246129>

These actually _shouldn't_ be left behind. The last part of the lockfile name is process id associated with the lock, but it appears there is a common bug related to cleaning these up (`gpg` gets terminated too soon). It's been suggested these are Yubikey related, but I'm unconvinced. 


### Notes on GNUPG prior to 2.2 (2017)
<https://www.gnupg.org/faq/whats-new-in-2.1.html#nosecring>

`pubring.gpg` and `secring.gpg` contained the public and secret (private) keys respectfully. It's also worth noting the `secring.gpg` file contained both. That was technically unnecessary, and that has since changed to `pubring.kbx` and the `private-keys-v1.d\` folder.
