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


## Advanced hardware key security
I was hoping to find a way to use a FIDO2 key with GnuPG, but no luck so far. I did find some related information.

### Using FIDO2 (and FIDO U2F) keys with OpenSSH

* <https://www.yubico.com/blog/github-now-supports-ssh-security-keys/>
* <https://developers.yubico.com/SSH/Securing_SSH_with_FIDO2.html>
* <https://xosc.org/u2fandssh.html>

### Using Yubikeys with GnuPG, SSH, etc
It's possible to use a Yubikey with GnuPG (instead of a FIDO2 key), but you do that by generating and storing an SSH key on the device.

* <https://www.stavros.io/posts/u2f-fido2-with-ssh/>
* <https://github.com/bsodmike/Yubikey-GPG-SSH-FIDO2-MFA-ZeroTrust>
* <https://www.thepolyglotdeveloper.com/2019/02/use-yubikey-pgp-signing-encryption-authentication/>

#### Securing SSH with PGP
Slightly different angle, making SSH credentials PGP's problem. Alas this doesn't solve the lack of generic FIDO2 support problem, but it's interesting.

* <https://developers.yubico.com/PIV/Guides/Securing_SSH_with_OpenPGP_or_PIV.html>



## Unsorted

* On PGP, noteworthy for clues on what you can store in and with PGP (subkeys?) <https://security.stackexchange.com/questions/29851/how-many-openpgp-keys-should-i-make>
* Docker uses PASS, a general linux password store. PASS uses PGP to encrypt passwords. Quite interesting! <https://www.passwordstore.org/>