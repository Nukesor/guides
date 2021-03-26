# Gpg

## Caveats

- Main key is only meant for signing. Encryption needs subkeys.
- Use `--expert` flag, if you want ECDSA-25519


## Useful commands

Maintenance:

- `gpg --refresh-keys` Refresh all keys in local keyring.
- `gpg --delete-key $KEY` Remove public key.

Overview:

- `gpg --list-secret-keys --keyid-format LONG` Show own private keys.
- `gpg --list-keys --keyid-format LONG` Show public keys in own keyring.

Trust: 

- `gpg --edit-key $KEY trust` Change trust level of key.
- `gpg --sign-key $KEY && gpg --send-keys $KEY` Sign and send another key to a keyserver (Show trust).

## Keyservers

- keyserver.ubuntu.com
- keys.openpgp.org
- pgp.mit.edu
