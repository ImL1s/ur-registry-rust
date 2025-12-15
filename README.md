# UR Registry Rust - TRON Support Fork

> **This is a fork of [KeystoneHQ/ur-registry-rust](https://github.com/KeystoneHQ/ur-registry-rust) with TRON blockchain support.**

## Why This Fork?

The original `ur-registry-rust` supports Bitcoin, Ethereum, Solana, and Cardano, but **does not include TRON support**. This fork adds:

- ✅ `tron-sign-request` UR type for transaction signing requests
- ✅ `tron-signature` UR type for signed transaction responses
- ✅ Flutter/Dart bindings for TRON types
- ✅ Pre-compiled Android libraries (arm64-v8a, armeabi-v7a)

## What's New

### Rust FFI Layer
- `libs/ur-registry-ffi/src/tron/`
  - `tron_sign_request.rs` - TRON sign request with CBOR encoding
  - `tron_signature.rs` - TRON signature response parser
  - `mod.rs` - Module exports

### Flutter/Dart Layer
- `interfaces/ur_registry_flutter/lib/registries/tron/`
  - `tron_sign_request.dart` - Dart class for creating sign requests
  - `tron_signature.dart` - Dart class for parsing signatures

### Pre-compiled Libraries
- `jniLibs/arm64-v8a/libur_registry_ffi.so` (1.7 MB)
- `jniLibs/armeabi-v7a/libur_registry_ffi.so` (1.2 MB)

## Usage

### Creating a TRON Sign Request (Flutter)

```dart
import 'package:ur_registry_flutter/registries/tron/tron_sign_request.dart';

final request = TronSignRequest.factory(
  signData: unsignedTxBytes,
  path: "m/44'/195'/0'/0/0",
  xfp: "12345678",
  address: "TRxxx...",
  origin: "TRON MultiSig Wallet",
  dataType: TronSignRequest.transaction,
);

// Get UR encoder for QR code display
final urEncoder = request.toUREncoder();
```

### Parsing TRON Signature Response

```dart
import 'package:ur_registry_flutter/registries/tron/tron_signature.dart';

void onScanSuccess(NativeObject object) {
  final signature = object as TronSignature;
  final sigBytes = signature.getSignature(); // hex string
  final requestId = signature.getRequestId();
}
```

---

## Original README

Yet another implementation for BC-UR registries. 

### Libs
- [UR-Registry](./libs/ur-registry/README.md)
- [UR-Registry-FFI](./libs/ur-registry-ffi/README.md)

### Interfaces
- [Flutter](./interfaces/ur_registry_flutter/README.md)

## Build

1. Install Android NDK 27.x or later
2. Install Rust nightly toolchain:
   ```bash
   rustup install nightly
   rustup default nightly
   rustup target add aarch64-linux-android armv7-linux-androideabi
   ```
3. Install cargo-ndk:
   ```bash
   cargo install cargo-ndk
   ```
4. Build:
   ```bash
   export ANDROID_NDK_HOME=/path/to/ndk
   cargo ndk -t arm64-v8a -t armeabi-v7a -o ./jniLibs build --release -p ur-registry-ffi
   ```

## Releases

Pre-compiled Android libraries are available in [GitHub Releases](../../releases).

## License

MIT License - Same as original project.
