# Hush
Hush is a minimalistic password manager written in Rust.

## Goals of the Project
There are three overarching goals for hush. They are:

- Be fast and lightweight (open quickly, search quickly)
- Be ergonomic (shouldn't have to use the mouse)
- Be modular (hush does not enforce a syncing solution; use your own)

## Technology Stack
- sqlite + diesel, for database management
- tantivy, for a full-text search engine
- tui-rs, for the terminal interface
- orion, for cryptography
