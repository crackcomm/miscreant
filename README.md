# <img alt="miscreant." src="https://miscreant.io/images/miscreant.svg">

[![Build Status][build-image]][build-link]
[![MIT Licensed][license-image]][license-link]
[![Gitter Chat][gitter-image]][gitter-link]

[build-image]: https://secure.travis-ci.org/miscreant/miscreant.svg?branch=master
[build-link]: http://travis-ci.org/miscreant/miscreant
[license-image]: https://img.shields.io/badge/license-MIT-blue.svg
[license-link]: https://github.com/miscreant/miscreant/blob/master/LICENSE.txt
[gitter-image]: https://badges.gitter.im/badge.svg
[gitter-link]: https://gitter.im/miscreant/Lobby

> The best crypto you've never heard of, brought to you by [Phil Rogaway]

A misuse resistant symmetric encryption library designed to support
authenticated encryption of individual messages, encryption keys,
message streams, or large files using the [AES-SIV] ([RFC 5297]) and
[CHAIN/STREAM] constructions.

Miscreant is available for several programming languages, including
[Go], [JavaScript], [Python], [Ruby], and [Rust].

[Phil Rogaway]: https://en.wikipedia.org/wiki/Phillip_Rogaway
[RFC 5297]: https://tools.ietf.org/html/rfc5297
[CHAIN/STREAM]: http://web.cs.ucdavis.edu/~rogaway/papers/oae.pdf
[Go]: https://github.com/miscreant/miscreant/tree/master/go
[JavaScript]: https://github.com/miscreant/miscreant/tree/master/js
[Python]: https://github.com/miscreant/miscreant/tree/master/python
[Ruby]: https://github.com/miscreant/miscreant/tree/master/ruby
[Rust]: https://github.com/miscreant/miscreant/tree/master/rust

## What is Miscreant?

**Miscreant** is a set of interoperable libraries implemented in several
languages providing a high-level API for misuse-resistant symmetric encryption.
Additionally, it provides support for "online" [authenticated encryption] use
cases such as streaming or incrementally encryption/decryption of large files.

The following constructions are provided by **Miscreant**:

* [AES-SIV]: (standardized in [RFC 5297]) combines the [AES-CTR]
  ([NIST SP 800-38A]) mode of encryption with the [AES-CMAC]
  ([NIST SP 800-38B]) or [AES-PMAC] function for integrity.
  Unlike most [authenticated encryption] algorithms, **AES-SIV** uses a
  special "encrypt-with-MAC" construction which combines the roles of an
  initialization vector (IV) with a message authentication code (MAC)
  using a construction called a *synthetic initialization vector* (SIV).
  It works in practice by first using **AES-CMAC** or **AES-PMAC** to derive
  an IV from a MAC of zero or more "header" values and the message
  plaintext, then encrypting the message under that derived IV.
  This approach provides not just the benefits of an authenticated
  encryption mode, but also makes it resistant to accidental reuse
  of an IV/nonce, something that would be catastrophic with a mode
  like **AES-GCM**. **AES-SIV** provides [nonce reuse misuse resistance],
  considered the gold standard in cryptography today.

* [CHAIN]: a construction which provides "online" chunked/multipart
  [authenticated encryption] when used in conjunction with a cipher like
  **AES-SIV**. **CHAIN** achieves the best-possible security for an online
  authenticated encryption scheme (OAE2).

* [STREAM]: a construction which provides streaming [authenticated encryption]
  and defends against reordering and truncation attacks. Unlike **CHAIN**,
  **STREAM** supports parallelization and seeking, allowing chunks within
  a message to be encrypted and decrypted in any order the user wants.
  **STREAM** provides nonce-based online authenticated encryption (nOAE),
  which the [CHAIN/STREAM] paper proves is equivalent to OAE2.

Though not yet described in an RFC, **CHAIN** and **STREAM** were designed by
[Phil Rogaway] (who also created **AES-SIV**) and are described in the paper
[Online Authenticated-Encryption and its Nonce-Reuse Misuse-Resistance], which
contains a rigorous security analysis proving them secure under the definitions
of OAE2 and nOAE respectively.

_NOTE:_ this library does not yet support **CHAIN** and **STREAM**! Please see
the tracking issues [CHAIN support (OAE2)] and [STREAM support (Nonce-based OAE)]
to follow progress on adding support.

[authenticated encryption]: https://en.wikipedia.org/wiki/Authenticated_encryption
[AES-SIV]: https://www.iacr.org/archive/eurocrypt2006/40040377/40040377.pdf
[AES-CTR]: https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Counter_.28CTR.29
[AES-CMAC]: https://en.wikipedia.org/wiki/One-key_MAC
[AES-PMAC]: http://web.cs.ucdavis.edu/~rogaway/ocb/pmac-bak.htm
[nonce reuse misuse resistance]: https://www.lvh.io/posts/nonce-misuse-resistance-101.html
[misuse resistant]: https://www.lvh.io/posts/nonce-misuse-resistance-101.html
[CHAIN]: http://web.cs.ucdavis.edu/~rogaway/papers/oae.pdf
[STREAM]: http://web.cs.ucdavis.edu/~rogaway/papers/oae.pdf
[Online Authenticated-Encryption and its Nonce-Reuse Misuse-Resistance]: http://web.cs.ucdavis.edu/~rogaway/papers/oae.pdf
[CHAIN support (OAE2)]: https://github.com/miscreant/miscreant/issues/33
[STREAM support (Nonce-based OAE)]: https://github.com/miscreant/miscreant/issues/32


## Help and Discussion

Have questions? Want to suggest a feature or change?

* [Gitter]: web-based chat about Miscreant
* [Google Group]: join via web or email ([miscreant-crypto+subscribe@googlegroups.com])

[Gitter]: https://gitter.im/miscreant/Lobby
[Google Group]: https://groups.google.com/forum/#!forum/miscreant-crypto
[miscreant-crypto+subscribe@googlegroups.com]: mailto:miscreant-crypto+subscribe@googlegroups.com?subject=subscribe


## Cipher Comparison

### Miscreant Ciphers

| Name              | [Authenticated Encryption] | [Misuse Resistance] | Performance        | Standardization   |
|-------------------|----------------------------|---------------------|--------------------|-------------------|
| AES-SIV           | :green_heart:              | :sparkling_heart:   | :yellow_heart:     | [RFC 5297]        |
| AES-PMAC-SIV      | :green_heart:              | :sparkling_heart:   | :green_heart:      | None              |

### Other Constructions

| Name              | [Authenticated Encryption] | [Misuse Resistance] | Performance        | Standardization   |
|-------------------|----------------------------|---------------------|--------------------|-------------------|
| AES-GCM-SIV       | :green_heart:              | :green_heart:†      | :sparkling_heart:♣ | Forthcoming‡      |
| AES-GCM           | :green_heart:              | :broken_heart:      | :sparkling_heart:♣ | [NIST SP 800-38D] |
| AES-CCM           | :green_heart:              | :broken_heart:      | :yellow_heart:     | [NIST SP 800-38C] |
| AES-CBC           | :broken_heart:             | :broken_heart:      | :sparkling_heart:  | [NIST SP 800-38A] |
| AES-CTR           | :broken_heart:             | :broken_heart:      | :sparkling_heart:  | [NIST SP 800-38A] |
| ChaCha20+Poly1305 | :green_heart:              | :broken_heart:      | :sparkling_heart:  | [RFC 7539]        |
| XSalsa20+Poly1305 | :green_heart:              | :broken_heart:      | :green_heart:      | None              |

### Legend

| Heart             | Meaning   |
|-------------------|-----------|
| :sparkling_heart: | Excellent |
| :green_heart:     | Great     |
| :yellow_heart:    | Fine      |
| :broken_heart:    | Bad       |

† Previous drafts of the AES-GCM-SIV specification were vulnerable to [key recovery attacks].
  These attacks are being addressed in newer drafts of the specification.

‡ Work is underway in the IRTF CFRG to provide an informational RFC for AES-GCM-SIV.
  For more information, see [draft-irtf-cfrg-gcmsiv][AES-GCM-SIV].

♣ Only applies to platforms which provide a hardware accelerated version of the
  **GHASH**/**POLYVAL** functions. On platforms that do **NOT** provide
  acceleration for these functions (e.g. microcontrollers/IoT platforms),
  these ciphers receive a :yellow_heart: (or potentially :broken_heart: if too
  constrained)

When standardization work around [AES-GCM-SIV] is complete, it will be
[seriously considered for inclusion in this library](https://github.com/miscreant/miscreant/issues/60).
**AES-GCM-SIV** has the advantage of the [GHASH] (technically **POLYVAL**)
function being able to run in parallel, versus **AES-CMAC**'s sequential
operation.

**AES-SIV** has the advantages of stronger security guarantees, simplicity,
and that it can be implemented using the AES encryption function alone, making
it a better choice for environments where a hardware accelerated version of the
**GHASH** function is unavailable, such as low-powered mobile devices and
so-called "Internet of Things" embedded use cases.

[Misuse Resistance]: https://www.lvh.io/posts/nonce-misuse-resistance-101.html
[NIST SP 800-38A]: https://dx.doi.org/10.6028/NIST.SP.800-38A
[NIST SP 800-38B]: https://dx.doi.org/10.6028/NIST.SP.800-38B
[NIST SP 800-38C]: https://dx.doi.org/10.6028/NIST.SP.800-38C
[NIST SP 800-38D]: https://dx.doi.org/10.6028/NIST.SP.800-38D
[RFC 7539]: https://tools.ietf.org/html/rfc7539
[key recovery attacks]: https://mailarchive.ietf.org/arch/attach/cfrg/pdfL0pM_N.pdf
[AES-GCM-SIV]: https://datatracker.ietf.org/doc/draft-irtf-cfrg-gcmsiv/
[GHASH]: https://en.wikipedia.org/wiki/Galois/Counter_Mode#Mathematical_basis


## Language Support

**Miscreant** libraries are available for the following languages:

| Language               | Version                              |
|------------------------|--------------------------------------|
| [Go][go-link]          | N/A                                  |
| [JavaScript][npm-link] | [![npm][npm-shield]][npm-link]       |
| [Python][pypi-link]    | [![pypi][pypi-shield]][pypi-link]    |
| [Ruby][gem-link]       | [![gem][gem-shield]][gem-link]       |
| [Rust][crate-link]     | [![crate][crate-shield]][crate-link] |

[go-link]: https://github.com/miscreant/miscreant/tree/master/go
[npm-shield]: https://img.shields.io/npm/v/miscreant.svg
[npm-link]: https://www.npmjs.com/package/miscreant
[pypi-shield]: https://img.shields.io/pypi/v/miscreant.svg
[pypi-link]: https://pypi.python.org/pypi/miscreant/
[gem-shield]: https://badge.fury.io/rb/miscreant.svg
[gem-link]: https://rubygems.org/gems/miscreant
[crate-shield]: https://img.shields.io/crates/v/miscreant.svg
[crate-link]: https://crates.io/crates/miscreant


## AES-SIV

This section provides a more in-depth exploration of how the **AES-SIV**
function operates.

### Encryption

<img src="https://miscreant.io/images/siv-encrypt.svg" width="410px" height="300px">

#### Inputs:

* **AES-CMAC** and **AES-CTR** *keys*: *K<sub>1</sub>* and *K<sub>2</sub>*
* Zero or more message *headers*: *H<sub>1</sub>* through *H<sub>m</sub>*
* Plaintext *message*: *M*

#### Outputs:

* Initialization vector: *IV*
* *Ciphertext* message: *C*

#### Description:

**AES-SIV** first computes **AES-CMAC** on the message headers *H<sub>1</sub>*
through *H<sub>m</sub>* and messages under *K<sub>1</sub>*, computing a
*synthetic IV* (SIV). This IV is used to perform **AES-CTR** encryption under
*K<sub>2</sub>*

### Decryption

<img src="https://miscreant.io/images/siv-decrypt.svg" width="410px" height="368px">

#### Inputs:

* **AES-CMAC** and **AES-CTR** *keys*: *K<sub>1</sub>* and *K<sub>2</sub>*
* Zero or more message *headers*: *H<sub>1</sub>* through *H<sub>m</sub>*
* Initialization vector: *IV*
* *Ciphertext* message: *C*

#### Outputs:

* Plaintext *message*: *M*

#### Description:

To decrypt a message, **AES-SIV** first performs an **AES-CTR** decryption of
the message under the provided synthetic IV. The message headers
*H<sub>1</sub>* through *H<sub>m</sub>* and candidate decryption message are
then authenticated by **AES-CMAC**. If the computed `IV’` does not match the
original one supplied, the decryption operation is aborted. Otherwise, we've
authenticated the original plaintext and can return it.


## Frequently Asked Questions (FAQ)

### 1. Q: If AES-SIV is so great, why have I never heard of it?

A: Good question! It's an underappreciated gem in cryptography.

### 2. Q: What does "SIV" stand for?

A: SIV stands for "synthetic initialization vector" and refers to the process
of deriving/"synthesizing" the initialization vector (i.e. the starting counter)
for AES-CTR encryption from the given message headers and plaintext message.

Where other schemes might have you randomly generate an IV, SIV modes
pseudorandomly "synthesize" one from the key, plaintext, and additional message
headers including optional associated data and nonce.

### 3. Q: What's the tl;dr for why I should use this?

A: It provides stronger security properties as compared to **AES-GCM**.
On Intel-based CPUs there is a ~50% performance hit, however on devices without
hardware acceleration for GCM's GHASH function (e.g. IoT/embedded), the
performance should be much better.

We hope to have benchmarks soon so we can show exactly how much performance is
lost/gained over **AES-GCM** and **AES-GCM-SIV** on various platforms, however
the scheme is still amenable to full hardware acceleration/parallelization
(at least in **AES-PMAC-SIV**'s case)  and should still remain very fast.

The **CHAIN** construction achieves the best-possible security for an online
[authenticated encryption] scheme (OAE2).

The **STREAM** construction provides nonce-based online
[authenticated encryption] (nOAE) and supports parallel encryption/decryption
as well as random access while defending against reordering and truncation
attacks.

There are other libraries that try to solve this problem, such as [saltpack],
however these libraries do not provide constructions with security proofs,
nor do they provide misuse resistant authenticated encryption. In particular
[saltpack] is a rather complicated amateur construction which does many
repitious and redundant HMAC operations with little justification as to why
or if all relevant data is actually cryptographically bound, much less a
rigorous security proof.

[saltpack]: https://saltpack.org/

### 4. Q: Are there any disadvantages to AES-SIV?

A: Using the AES function as a MAC (i.e. **AES-CMAC**) is more expensive than
faster hardware accelerated functions such as **GHASH** and **POLYVAL** (which
use _CLMUL_ instructions on Intel CPUs). Additionally **AES-CMAC** relies on
chaining and therefore cannot run in parallel. This makes **AES-SIV** slower
than **AES-GCM-SIV** on Intel systems, however **AES-SIV** provides better
security guarantees and will be faster on systems that do not have hardware
acceleration for **GHASH**. We hope to post benchmark numbers soon.

Due to the 128-bit size of the AES block function, **AES-SIV** can only be
safely used to encrypt up to approximately 2<sup>64</sup> messages under the
same key before the "birthday bound" is hit and repeated IVs become probable
enough to be a security concern. Though this number is relatively large, it is
not outside the realm of possibility.

(Note that the [1k-PMAC_Plus] construction might be able to address this)

[1k-PMAC_Plus]: https://github.com/miscreant/miscreant/issues/76

### 5. Q: Are there any disadvantages to the SIV approach in general?

A: SIV encryption requires making a complete pass over the input in order to
calculate the IV. This is less cache efficient than modes which are able
to operate on the plaintext block-by-block, performing encryption and
authentication at the same time. This makes SIV encryption slightly slower
than non-SIV encryption.

However, this does not apply to SIV decryption: since the IV is (allegedly)
known in advance, SIV decryption and authentication can be performed
block-by-block, making it just as fast as the corresponding non-SIV mode
(which for **AES-SIV** would be **AES-EAX** mode).

### 6. Q: Isn't MAC-then-encrypt bad? Shouldn't you use encrypt-then-MAC?

A: Though SIV modes run the MAC operation first, then the encryption function
second, they are a bit different from what is typically referred to as
"MAC-then-encrypt". SIV modes cryptographically bind the encryption and
authentication together by using the authentication tag as an input to the
encryption cipher, making them provably secure for all the same classes of
attacks as encrypt-then-MAC modes. You can think of SIV modes as being a
completely different class of combining encryption and a MAC, which can
be described as something like "encrypt-with-MAC" or "encrypt-under-MAC".

Another common source of problems with MAC-then-encrypt is padding oracles,
which are commonly seen with CBC modes. **AES-SIV** is based on CTR mode, which
is a stream cipher and therefore doesn't need padding, making it immune to
padding oracles by design.

Authenticating the decrypted data does involve decrypting it, however. This
means decrypted data is, at one point in time, in memory before it is
authenticated. This increases the risk that attacker-controlled plaintext
might wind up being used due to authentication bugs.

These libraries attempt to ensure unauthenticated plaintext is never exposed.
Furthermore some libraries will perform the **AES-CTR** portion of **AES-GCM**
decryption without checking the GCM tag, so encrypt-then-MAC is not a
bulletproof solution to preventing exposure of unauthenticated plaintexts.
To some degree you will always be trusting the implementation quality of a
particular library to ensure it operates in a secure manner.

### 7: Q: Why did you implement the same algorithms 5 times instead of just implementing it in Rust and then wrapping the Rust version, which has better security and performance?

A: We certainly plan on making bindings to the Rust implementation available as optional backends for the other language versions. So we are picking both options.

Miscreant is directly implemented in the respective language to make it more portable and easier to install. While the Rust implementation is the best the library presently offers, it has a lot of drawbacks: first, you have to install Rust to compile it. Second, it requires a nightly Rust compiler, which means the implementation must keep up with nightly changes to Rust. Third, the Rust implementation only supports the Intel [AES-NI] extension, and therefore won't work on anything but modern Intel CPUs.

[AES-NI]: https://www.intel.com/content/www/us/en/architecture-and-technology/advanced-encryption-standard--aes-/data-protection-aes-general-technology.html

### 8. Q: Is this algorithm NIST approved / FIPS compliant?

A: **AES-SIV** is the combination of two NIST approved algorithms:
**AES-CTR** encryption as described in [NIST SP 800-38A], and
**AES-CMAC** authentication as described in [NIST SP 800-38B].

However, while **AES-SIV** was [submitted to NIST] as a [proposed mode],
it has never received official approval from NIST.

If you are considering using this software in a FIPS 140-2 environment, please
check with your FIPS auditor before proceeding. It may be possible to justify
the use of **AES-SIV** based on its NIST approved components, but we are not
FIPS auditors and cannot give prescriptive advice here.

Please note that none of this code has undergone a FIPS audit to begin with, so
if you intend to use it in that capacity, you're on your own and should
fork/vendor your own copy.

[submitted to NIST]: http://csrc.nist.gov/groups/ST/toolkit/BCM/documents/proposedmodes/siv/siv.pdf
[proposed mode]: http://csrc.nist.gov/groups/ST/toolkit/BCM/modes_development.html

### 9. Q: Are there any patent concerns around the AES-SIV/AES-PMAC-SIV modes?

A: No, there are [no IP rights concerns] with either the **AES-SIV** mode or
**AES-PMAC-SIV** modes (see the "[What about patents?]" section of Rogaway's PMAC
FAQ for imformation about PMAC).

To the best of our knowledge, the algorithm is entirely in the public domain. 

[no IP rights concerns]: http://csrc.nist.gov/groups/ST/toolkit/BCM/documents/proposedmodes/siv/ip.pdf
[What About Patents?]: http://web.cs.ucdavis.edu/~rogaway/ocb/pmac-bak.htm

### 10. Q: Why not wait for the winner of the CAESAR competition to be announced?

A: The [CAESAR] competition (to select a next generation authentication encryption
cipher) seems to be taking much longer than was originally expected. Even when
it concludes, it will be some time before relevant standards are written as to
the usage and deployment of its winner.

Meanwhile [RFC 5297] is nearly a decade old, and **AES-SIV** has seen some
organic usage. While not entirely optimal by the metrics of the CAESAR
competition, it's a boring, uncontroversial solution we can use off-the-shelf today.

Furthermore, Miscreant has a big advantage over any of the CAESAR contestants: simplicity.
The Rust version, for example, which provides both **AES-SIV** and a fully parallelized
implementation of **AES-PMAC-SIV**, implementing all parts of the constituent algorithms
from scratch on top of the AES block encryption function alone, is a little more than 800
lines of code.

[CAESAR]: https://competitions.cr.yp.to/caesar-submissions.html

### 11. Q: Do you plan on supporting additional ciphers? (e.g. AES-GCM-SIV, HS1-SIV)

A: In the future, we may consider adding support for **AES-GCM-SIV**.

Please see [Issue #60: AES-GCM-SIV](https://github.com/miscreant/miscreant/issues/60)
for more information.

### 12. Q: This project mentions security proofs several times. Where do I find them?

A: Please see the paper [Deterministic Authenticated-Encryption: A Provable-Security Treatment of the Key-Wrap Problem]

### 13. Q: Where are CHAIN/STREAM? I can't find them!

A: The many claims of support in the READMEs are actually lies! They are not implemented yet.
Support is forthcoming, sorry!

Please see the tracking issues [CHAIN support (OAE2)] and [STREAM support (Nonce-based OAE)]
to follow progress on adding support.

## Code of Conduct

We abide by the [Contributor Covenant][cc] and ask that you do as well.

For more information, please see [CODE_OF_CONDUCT.md].

[cc]: https://contributor-covenant.org
[CODE_OF_CONDUCT.md]: https://github.com/miscreant/miscreant/blob/master/CODE_OF_CONDUCT.md

## Key Rap

The paper describing AES-SIV,
[Deterministic Authenticated-Encryption: A Provable-Security Treatment of the Key-Wrap Problem]
contains this explanatory rap song at the end, which goes out to all the
chronic IV misusing miscreants in the land:

> Yo! We’z gonna’ take them keys an’ whatever you pleaze<br>
> We gonna’ wrap ’em all up looks like some ran’om gup<br>
> Make somethin’ gnarly and funky won’t fool no half-wit junkie<br>
> So the game’s like AE but there’s one major hitch<br>
> No coins can be pitched there’s no state to enrich<br>
> the IV’s in a ditch dead drunk on cheap wine<br>
> Now NIST and X9 and their friends at the fort<br>
> suggest that you stick it in a six-layer torte<br>
> S/MIME has a scheme there’s even one more<br>
> So many ways that it’s hard to keep score<br>
> And maybe they work and maybe they’re fine<br>
> but I want some proofs for spendin’ my time<br>
> After wrappin’ them keys gonna’ help out some losers<br>
> chronic IV abusers don’t read no directions<br>
> risk a deadly infection If a rusty IV’s drippin’ into yo’ veins<br>
> and ya never do manage to get it exchanged<br>
> Then we got ya somethin’ and it comes at low cost<br>
> When you screw up again not all ’ill be lost

[Deterministic Authenticated-Encryption: A Provable-Security Treatment of the Key-Wrap Problem]: http://web.cs.ucdavis.edu/~rogaway/papers/keywrap.pdf

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/miscreant/miscreant

## Copyright

Copyright (c) 2017 [The Miscreant Developers][AUTHORS].
Distributed under the MIT license. See [LICENSE.txt] for further details.

Some language-specific subprojects include sources from other authors with more
specific licensing requirements, though all projects are MIT licensed.
Please see the respective **LICENSE.txt** files in each project for more
information.

[AUTHORS]: https://github.com/miscreant/miscreant/blob/master/AUTHORS.md
[LICENSE.txt]: https://github.com/miscreant/miscreant/blob/master/LICENSE.txt
