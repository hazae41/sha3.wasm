# sha3.wasm

WebAssembly port of SHA-3

```bash
npm install @hazae41/sha3-wasm
```

[**📦 NPM**](https://www.npmjs.com/package/@hazae41/sha3-wasm)

## Features
- Reproducible building
- Pre-bundled and streamed
- Zero-copy memory slices

## Modules
- sha3

## Algorithms
- Keccak-256 (direct and incremental)

## Usage

### Direct

```typescript
import { load, Memory, keccak256 } from "@hazae41/sha3.wasm";

await load()

// Data to be hashed
const hello = new TextEncoder().encode("Hello World")

// Pass to WASM
using memory = new Memory(hello)

// Grab the digest
using digest = keccak256(memory)

console.log(digest.bytes) // Uint8Array
```

### Incremental

```typescript
import { load, Memory, Keccak256Hasher } from "@hazae41/sha3.wasm";

await load()

// Create a hash
using hasher = new Keccak256Hasher()

// Data to be hashed
const hello = new TextEncoder().encode("Hello World")

// Pass to WASM
using memory = new Memory(hello)

// Update the hash with your data
hasher.update(memory)

// Grab the digest
using digest = hasher.finalize()

// Update the hash another time
hasher.update(memory)

// Grab the digest
using digest2 = hasher.finalize()

// digest !== digest2
console.log(digest.bytes)
console.log(digest2.bytes)
```

## Building

### Reproducible building

You can build the exact same bytecode

```bash
npm run compile && npm run prepack
```

Then check that all the files are the same using `npm diff`

```bash
npm diff
```

If the output is empty then the bytecode is the same as the one I published on NPM.

### Automated checks

Each time I release a new version on GitHub, the GitHub's CI clones the GitHub repository, reproduces the build, and throws an error if the NPM release is different. If a version is present on NPM but not on GitHub, do not use it!
