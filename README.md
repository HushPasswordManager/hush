![hush: do few things, and do them well](./assets/hush-logo.svg)

# Hush
Hush is a minimalistic password manager written in Rust.

## Goals of the Project
There are four overarching goals for Hush.

- Be fast and lightweight (open quickly, search quickly)
- Be ergonomic (shouldn't have to use the mouse)
- Be modular (hush does not enforce a syncing solution; use your own)
- Be extensible (the hush daemon interfaces using standard protocols, i.e. msgpack)

## Security
### WARNING
**Hush should not yet be used where real security is required.** This is for two reasons. First, the cryptographic library underlying Hush (orion, see [Technology Stack](#technology-stack)) has not undergone a formal security audit. Second, Hush itself has not undergone a security audit. Before this warning is removed, at the very least orion will need a thorough security audit. If/when it gets one, the developers of Hush will seek an expert opinion on whether the same should be done for Hush itself.

### Cryptographic Components
Hush takes an approach similar to [WireGuard](https://www.wireguard.com/) when it comes to selection of cryptographic primitives and algorithms. The developers endeavor to choose the best possible (most secure) primitives, and give the user no options, so as to prevent users from accidentally compromising their own security. If one of Hush's primitives becomes outdated, it will be repealed and replaced with whatever is the current state of the art. Here are the current choices.
- Argon2i for password hashing
- XSalsa20-Poly1305 for encrypting data
- HMAC (SHA2-256) for inter-process message authentication
- OS-provided random numbers for anything involving random numbers (e.g. passphrase generation, nonces, etc.)

## Why Not to Use Hush
To save you some time reading the rest of this document, here are some reasons you may not want to use Hush.
- You need security guarantees. Those don't exist for Hush. Not yet. See the [Security](#Security) section. 
- You want complex organization tools. Hush uses a simple, yet powerful system of key-value pairs with associated tags. If you need the ability to create abstract "bins" or "containers" or "folders", Hush isn't for you. Consider, though, whether tags could neatly replace your use of those "organizational" tools. 
- You need it all *right now*. Hush isn't really ready yet. Its developers are content to take their time and get the software right. Priority is given to `hush-daemon`, the process that rules all other processes. Work on the various front ends is somewhat secondary. Consider building your own! We've tried to make it easy to do so.

## Specific Features
What *actually* makes Hush a better password manager? Here are a list of features that are planned and/or implemented. You can decide for yourself if what is available now or what may be available in the future sounds interesting enough to make the switch.
- [ ] **Free and open source.** This could matter to you in two ways. First, you may be an idealist about FOSS, in which case your choice of password managers is somewhat limited. Second, you may care about knowing exactly what your password manager does under the hood. Either way, Hush fits the bill.
- [ ] **Security.** Hush uses modern standards to ensure that your data is secure. Read more about this in the [Security] section.
- [ ] **Near-instant start-up time.** The Hush daemon runs in the background constantly, so starting "Hush" is only starting a frontend. This should and will be very fast. The Hush developers (especially with respect to the daemon) take into careful consideration the memory footprint, CPU consumption, and binary size of Hush.
- [ ] **Ergonomic.** The API for `hush-daemon` is intended to be well-documented and both high- and low-level. It should fulfill any and all search and data access queries that a front end could need. The various front ends supported by the Hush developers aim to present a fast, mouse-optional workflow for users. The developers' ideal password manager works as follows. A global shortcut opens a text prompt, which in turn accepts a fuzzy search for a key and another fuzzy search for tags. The fuzzy search results update as the user types. Regex can be substituted for fuzzy search anywhere. Choosing a result shows its different fields, and choosing a field results in copying its associated value. All choices can be made via the keyboard.
- [ ] **Password suggestions.** It's crucial for a password manager to be able to generate passwords of arbitrary strength. Strength, for Hush, is measured by the zxcvbn algorithm (thanks to Dropbox). Given a strength and various password/passphrase configuration settings (words?, spaces?, numbers?, etc), Hush will generate a password for you.
- [ ] **Modular.** Hush (the daemon) focuses on one specific task: managing a database of encrypted key-value pairs (with tags) and presenting a simple API to access this data. Syncing solutions and frontends are outside of its scope, and are therefor not restricted. That said, the Hush developers maintain several different frontends for `hush-daemon`.
- [ ] **Standards-based.** Hush is based on widely-used FOSS standards such as msgpack, all of its [cryptography choices](#cryptographic-components), and its persistent storage format (SQLite). 
  - [ ] **Extensible.**
  - [ ] **Safe.**
- [ ] **2FA.** The Hush developers plan to support both hardware (e.g. [Yubikey](https://www.yubico.com/)) and software (e.g. [Authy](https://authy.com/)) 2-factor authentication.
- [ ] **Optional web interface.** Users should be able to access their data from anywhere with an internet connection. It will be easy to deploy Hush to a web server that presents a secure, performant web UI, and also a REST API for those who want to build it into 3rd-party applications.
- [ ] **Support migration.** Hush uses basic nested key-value stores with associated tags, so it should be able to integrate all the data from any of the following applications. Support for each of them is the eventual goal.
  - [ ] 1Password
  - [ ] BitWarden
  - [ ] Dashlane
  - [ ] KeyPass
  - [ ] LastPass

## Technology Stack
### Security
- [orion](https://crates.io/crates/orion), for cryptography
- [zxcvbn](https://crates.io/crates/zxcvbn-rs), for password-strength estimation

### Non-Security Core Stack
- [msgpack](https://msgpack.org/), for inter-process communication
- [sqlite](https://sqlite.org/index.html) + [diesel](http://diesel.rs/), for database management
- [strsim](https://crates.io/crates/strsim), for fuzzy searching
- [regex](https://crates.io/crates/regex), for regular-expression searching

### User Interfaces
- [tui-rs](https://crates.io/crates/tui), for the terminal interface
