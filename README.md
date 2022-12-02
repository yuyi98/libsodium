Libsodium wrapper for V.

```
v install libsodium
```

(Tested to work on Ubuntu 20.04, libsodium 1.0.18).

C function definitions were generated by C2V.

A simple wrapper is added on top, to make the API
easier to use in V.

The wrapper is very similar to PyNaCl.

Usage:

```v
import libsodium

// Random numbers generation
println(libsodium.randombytes_random())

// Secret-key cryptography:
box := libsodium.new_secret_box('key')
encrypted := box.encrypt_string('hello')
decrypted := box.decrypt_string(encrypted)
assert decrypted == 'hello'
println(decrypted)

encrypted_bytes := box.encrypt([u8(0), 1, 2, 3])
decrypted_bytes := box.decrypt(encrypted_bytes)
assert decrypted_bytes == [u8(0), 1, 2, 3]

// Public-key cryptography:
key_alice := libsodium.new_private_key()
key_bob := libsodium.new_private_key()

alice_box := libsodium.new_box(key_alice, key_bob.public_key)
bob_box := libsodium.new_box(key_bob, key_alice.public_key)

pkey_encrypted := bob_box.encrypt_string('hello')
pkey_decrypted := alice_box.decrypt_string(pkey_encrypted)

println(pkey_decrypted)
assert pkey_decrypted == 'hello'
```

## performance test:

v run libsodium_test.v

```bash
2568688128
nr of ms for 1million iterations: 820 for test symmetric encryption
nr iterations per sec for symmetric encryption: 1219000
nr of ms for 10thousand iterations: 1413 for test asymm encryption
nr iterations per sec for asymm encryption: 7000
```
