# miscreant.rb [![Latest Version][gem-shield]][gem-link] [![Build Status][build-image]][build-link] [![Code Climate][codeclimate-image]][codeclimate-link] [![MIT licensed][license-image]][license-link] [![Gitter Chat][gitter-image]][gitter-link]

[gem-shield]: https://badge.fury.io/rb/miscreant.svg
[gem-link]: https://rubygems.org/gems/miscreant
[build-image]: https://secure.travis-ci.org/miscreant/miscreant.svg?branch=master
[build-link]: http://travis-ci.org/miscreant/miscreant
[codeclimate-image]: https://codeclimate.com/github/miscreant/miscreant/badges/gpa.svg
[codeclimate-link]: https://codeclimate.com/github/miscreant/miscreant
[license-image]: https://img.shields.io/badge/license-MIT-blue.svg
[license-link]: https://github.com/miscreant/miscreant/blob/master/LICENSE.txt
[gitter-image]: https://badges.gitter.im/badge.svg
[gitter-link]: https://gitter.im/miscreant/Lobby

> The best crypto you've never heard of, brought to you by [Phil Rogaway]

Ruby implementation of **Miscreant**: Advanced symmetric encryption using the
AES-SIV ([RFC 5297]) and [CHAIN/STREAM] constructions, providing easy-to-use (or
rather, hard-to-misuse) encryption of individual messages or message streams.

**AES-SIV** provides [nonce-reuse misuse-resistance] (NRMR): accidentally
reusing a nonce with this construction is not a security catastrophe,
unlike it is with more popular AES encryption modes like [AES-GCM].
With **AES-SIV**, the worst outcome of reusing a nonce is an attacker
can see you've sent the same plaintext twice, as opposed to almost all other
AES modes where it can facilitate [chosen ciphertext attacks] and/or
full plaintext recovery.

For more information, see the [toplevel README.md].

[Phil Rogaway]: https://en.wikipedia.org/wiki/Phillip_Rogaway
[AES-SIV]: https://www.iacr.org/archive/eurocrypt2006/40040377/40040377.pdf
[RFC 5297]: https://tools.ietf.org/html/rfc5297
[CHAIN/STREAM]: http://web.cs.ucdavis.edu/~rogaway/papers/oae.pdf
[nonce-reuse misuse-resistance]: https://www.lvh.io/posts/nonce-misuse-resistance-101.html
[AES-GCM]: https://en.wikipedia.org/wiki/Galois/Counter_Mode
[chosen ciphertext attacks]: https://en.wikipedia.org/wiki/Chosen-ciphertext_attack
[toplevel README.md]: https://github.com/miscreant/miscreant/blob/master/README.md

## Help and Discussion

Have questions? Want to suggest a feature or change?

* [Gitter]: web-based chat about miscreant projects including **miscreant.rb**
* [Google Group]: join via web or email ([miscreant-crypto+subscribe@googlegroups.com])

[Gitter]: https://gitter.im/miscreant/Lobby
[Google Group]: https://groups.google.com/forum/#!forum/miscreant-crypto
[miscreant-crypto+subscribe@googlegroups.com]: mailto:miscreant-crypto+subscribe@googlegroups.com?subject=subscribe

## Security Notice

Though this library is written by cryptographic professionals, it has not
undergone a thorough security audit, and cryptographic professionals are still
humans that make mistakes. Use this library at your own risk.

## Requirements

This library is tested against the following MRI versions:

- 2.2
- 2.3
- 2.4

Other Ruby versions may work, but are not officially supported.

## Installation

Add this line to your application's Gemfile:

```ruby
gem "miscreant"
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install miscreant

## API

### Miscreant::AEAD

The `Miscreant::AEAD` class provides the main interface to the **AES-SIV**
misuse resistant authenticated encryption function.

To make a new instance, pass in a binary-encoded 32-byte or 64-byte key.
Note that these options are twice the size of what you might be expecting
(AES-SIV uses two AES keys).

```ruby
secret_key = Miscreant::AEAD.generate_key
encryptor = Miscreant::AEAD.new("AES-SIV", secret_key)
```

#### Encryption (#seal)

The `Miscreant::AEAD#seal` method encrypts a binary-encoded message along with
a set of *associated data* message headers.

It's recommended to include a unique "nonce" value with each message. This
prevents those who may be observing your ciphertexts from being able to tell
if you encrypted the same message twice. However, unlike other cryptographic
algorithms where using a nonce has catastrophic security implications such as
key recovery, reusing a nonce with AES-SIV only leaks repeated ciphertexts to
attackers.

Example:

```ruby
message = "Hello, world!"
nonce = Miscreant::AEAD.generate_nonce
ciphertext = key.seal(message, nonce)
```

#### Decryption (#open)

The `Miscreant::AEAD#open` method decrypts a binary-encoded ciphertext with the
given key.

Example:

```ruby
message = "Hello, world!"
nonce = Miscreant::AEAD.generate_nonce
ciphertext = key.seal(message, nonce)
plaintext = key.open(ciphertext, nonce)
```

## Code of Conduct

We abide by the [Contributor Covenant][cc] and ask that you do as well.

For more information, please see [CODE_OF_CONDUCT.md].

[cc]: https://contributor-covenant.org
[CODE_OF_CONDUCT.md]: https://github.com/miscreant/miscreant/blob/master/CODE_OF_CONDUCT.md

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/miscreant/miscreant

## Copyright

Copyright (c) 2017 [The Miscreant Developers][AUTHORS].
See [LICENSE.txt] for further details.

[AUTHORS]: https://github.com/miscreant/miscreant/blob/master/AUTHORS.md
[LICENSE.txt]: https://github.com/miscreant/miscreant/blob/master/LICENSE.txt
