# Crypt

The **crypt** library provides methods for the encryption and decryption of string data.

---

## crypt.base64

### crypt.base64.encode

```lua
function crypt.base64.encode(data: string): string
```

Encodes a string of bytes into Base64.

#### Parameters
* `data`: The data to encode.

#### Aliases
* `crypt.base64.encode`
* `crypt.base64_encode`
* `base64.encode`
* `base64_encode`

#### Example

```lua
local base64 = crypt.base64.encode("Hello, World!")
local raw = crypt.base64.decode(base64)

print(base64) --> SGVsbG8sIFdvcmxkIQ==
print(raw) --> Hello, World!
```

---

### crypt.base64.decode

```lua
function crypt.base64.decode(data: string): string
```

Decodes a Base64 string to a string of bytes.

#### Parameters
* `data`: The data to decode.

#### Aliases
* `crypt.base64.decode`
* `crypt.base64_decode`
* `base64.decode`
* `base64_decode`

#### Example

```lua
local base64 = crypt.base64.encode("Hello, World!")
local raw = crypt.base64.decode(base64)

print(base64) --> SGVsbG8sIFdvcmxkIQ==
print(raw) --> Hello, World!
```

---

## crypt.url

### crypt.url.encode

```lua
function crypt.url.encode(text: string): string
```

Url encodes text, so it can be sent through url the paramters.

#### Parameters
* `text`: The text to be url encoded.

#### Examples
```lua
local text = crypt.url.encode("Hello, World!")
print(text) --> Hello%2C%20World%21
```

---

### crypt.url.decode

```lua
function crypt.url.decode(text: string): string
```

Decodes text that is url encoded into normal text.

#### Parameters
* `text`: The url encoded text to decode.

#### Examples
```lua
local text = crypt.url.decode("Hello%2C%20World%21")
print(text) --> Hello, World!
```

---

## crypt.hex

### crypt.hex.encode

```lua
function crypt.hex.encode(text: string): string
```

Encodes a string into a sequence of hex numbers inside a string.

#### Parameters
* `text`: The text to be encoded.

#### Examples
```lua
local text = crypt.hex.encode("Hello, World!")
print(text) --> 48656c6c6f2c20576f726c6421
```

### crypt.hex.decode

```lua
function crypt.hex.decode(text: string): string
```

Decodes a hex encoded string back into normal text.

#### Paramters
* `text`: The hex encoded text to decode.

#### Examples
```lua
local text = crypt.hex.decode("48656c6c6f2c20576f726c6421")
print(text) --> Hello, World!
```

---

## crypt.encrypt

```lua
function crypt.encrypt(data: string, key: string, iv: string?, mode: string?): (string, string)
```

Encrypts an unencoded string using AES encryption. Returns the base64 encoded and encrypted string, and the IV.

If an AES IV is not provided, a random one will be generated for you, and returned as a 2nd base64 encoded string.

The cipher modes are `CBC`, `ECB`, `CTR`, `CFB`, `OFB`, and `GCM`. The default is `CBC`.

### Parameters
* `data`: The unencoded content.
* `key`: A base64 256-bit key.
* `iv`: Optional base64 AES initialization vector.
* `mode`: The AES cipher mode.

---

## crypt.decrypt

```lua
function crypt.decrypt(data: string, key: string, iv: string, mode: string): string
```

Decrypts the base64 encoded and encrypted content. Returns the raw string.

The cipher modes are `CBC`, `ECB`, `CTR`, `CFB`, `OFB`, and `GCM`.

### Parameters
* `data`: The base64 encoded and encrypted content.
* `key`: A base64 256-bit key.
* `iv`: The base64 AES initialization vector.
* `mode`: The AES cipher mode.

---

## crypt.generatebytes

```lua
function crypt.generatebytes(size: number): string
```

Generates a random sequence of bytes of the given size. Returns the sequence as a base64 encoded string.

### Parameters
* `size`: The number of bytes to generate.

### Example
```lua
local bytes = crypt.generatebytes(16)
print(bytes) --> For example: bXlzcWwgYm9vbGVhbnM=
print(#crypt.base64decode(bytes)) --> 16
```

---

## crypt.generatekey

```lua
function crypt.generatekey(): string
```

Generates a base64 encoded 256-bit key. The result can be used as the second parameter for the `crypt.encrypt` and `crypt.decrypt` functions.

### Aliases
* `crypt.random`

### Example

```lua
local bytes = crypt.generatekey()
print(#crypt.base64decode(bytes)) --> 32 (256 bits)
```

---

## crypt.hash

```lua
function crypt.hash(data: string, algorithm: string): string
```

Returns the result of hashing the data using the given algorithm.

Some algorithms include `sha1`, `sha384`, `sha512`, `md5`, `sha256`, `sha3-224`, `sha3-256`, and `sha3-512`.

### Parameters
* `data`: The unencoded content.
* `algorithm`: A hash algorithm.

### Example
```lua
local hash = crypt.hash("Hello, World!", "md5")
print(hash) --> 65A8E27D8879283831B664BD8B7F0AD4
```

---

## crypt.hashes

```lua
crypt.hashes: dictionary<string, string>
```

A dictionary that serves as a cache for previously computed hash values.

When `crypt.hash` is called, it first checks if it has already cached this inside `crypt.hashes`. If it has already hashed the data before, it'll return early. If not, it'll hash it and store the hashed data in `crypt.hashes`.

---
