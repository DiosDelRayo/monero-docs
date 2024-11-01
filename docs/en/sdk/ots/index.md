# Offline Transaction Signing

## Offline side
This is the cold wallet end, the device normally never goes online, like a Offline Signer or
an Hardware wallet.

### C++ library
The library compiled out of the monero source, without further dependencies, specific for the
usecase of offline signer and hardware wallets.

* [Quickstart](quickstart.md)
* [Reference](reference/)

The library provides following features:
    * Monero Seed generation
    * Polyseed generation
    * Address and Key generation
    * Account and Subaddress managment
    * Address verification
    * Import outputs
    * Export Key Images
    * Unsigned Transaction handling (verification and signing)

### C ABI
Are actually the C ABI `extern "C"` statements in the C++ library where the compiler creates an interface which can be used from C but still using the C++ source code. At this point you can use the library in C instead C++ and enables a lot, if not all, languages to use the library, continure reading in [Wrappers](#Wrappers)

* [Quickstart](c/quickstart.md) for using in C or writing an wrapper.
* [Reference](c/reference/)

### Wrappers
With the C ABI provided it can be used by a lot of languages via FFI, a more practical approach is to provide a wrapper so the developer in the language used does not even think about it, but can instead simply use the library in the language own natural way.

#### Python
The Python wrapper uses cffi and can be found here: [monero-ots-python](https://github.com/DiosDelRayo/monero-ots-python)

* [Quickstart](python/quickstart.md)
* [Reference](python/reference/)
* [PyPy]()
* [Buildroot package]()

#### Java
The Java wrapper using JNI will be to be found there: [monero-ots-java](https://github.com/DiosDelRayo/monero-ots-java)

#### Dart
The Dart/Flutter wrapper will be to be found there: [monero-ots-dart](https://github.com/DiosDelRayo/monero-ots-dart)

*[ABI]: Application Binary Interface
*[FFI]: Foreign Function Interface
