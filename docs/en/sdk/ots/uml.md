---
title: UML Diagram
---
!!! warning
    This was the initial design of the third iteration, which is not in sync with the actual development at the moment, as it will be synced when there are (probably) no more changes.
    Even the [reference](./reference/index.html) will be at the most probably, behind how it is not yet automatically synced - which should happen at a later point in time.

```mermaid
classDiagram

class OTSException {
    +OTSException(const std::string& msg)
}

class runtime_error["std::runtime_error"] {
    std::string msg
}

OTSException --|> runtime_error
```

```mermaid
classDiagram
class OTS {
    +OTS()
    +validAddress(const std::string& address, Network network = Network::MAIN) : bool
}
```

```mermaid
classDiagram
class Network {
    <<enumeration>>
    MAIN
    TEST
    STAGE
}
```

```mermaid
classDiagram
class Address {
    +Address(const std::string& address)
    +isValid() : const bool noexep
    +index(const Wallet& wallet) : const std::pair<int, int>
    +operator std::string(): const std::string&
}
```

```mermaid
classDiagram

class SeedLanguage {
    +name() : const std::string&
    +englishName() : const std::string&
    +code() : const std::string&
    +fromName(const std::string& name) : const SeedLanguage&
    +fromEnglishName(const std::string& name) : const SeedLanguage&
    +fromCode(const std::string& code) : const SeedLanguage&
    +list()$ : const std::vector<SeedLanguage>&
    +default()$ : const SeedLanguage&
}
```

```mermaid
classDiagram

class Seed {
    <<interface>>
    +phrase(const SeedLanguage& language) : std::string
    +values() : std::vector<int>
    +fingerprint(const Network& network) : std::string
    +storeKey(KeyJar& keyJar, const std::string& label = std::string()) : key_handle_t
    +valid() : bool
    +birthday() : uint64_t
    +height() : uint64_t
    +encrypted() : bool
    +network() : Network
    +languageSupported(const SeedLanguage& language) : bool
}

class EncryptableSeed {
    <<interface>>
    +encrypt(const std::string& password) : bool
    +decrypt(const std::string& password) : bool
}

class LegacySeed {
    +decode(const std::string& phrase, const SeedLanguage& language, uint64_t height = 0, uint64_t time = 0, Network network = Network::MAIN)$ : LegacySeed
    +decode(const std::vector<int>& values, uint64_t height = 0, uint64_t time = 0, Network network = Network::MAIN)$ : LegacySeed
}

class MoneroSeed {
    +decode(const std::string& phrase, const SeedLanguage& language, uint64_t height = 0, uint64_t time = 0, bool encrypted = false, Network network = Network::MAIN)$ : MoneroSeed
    +decode(const std::vector<int>& values, uint64_t height = 0, uint64_t time = 0, bool encrypted = false, Network network = Network::MAIN)$ : MoneroSeed
    +create(uint64_t height = 0, uint64_t time = 0, Network network = Network::MAIN)$ : MoneroSeed
}

class Polyseed {
    +create(uint64_t time, const SeedLanguage& language, Network network = Network::MAIN)$ : Polyseed
    +decode(const std::string& phrase, Network network = Network::MAIN)$ : Polyseed
    +decode(const std::vector<int>& values, Network network = Network::MAIN)$ : Polyseed
}

LegacySeed --|> Seed
EncryptableSeed --|> Seed
MoneroSeed --|> EncryptableSeed
Polyseed --|> EncryptableSeed
```

```mermaid
classDiagram

class SeedJar {
    +instance()$ : const SeedJar&
    +store(const Seed&) : const seed_handle_t
    +get(const seed_handle_t handle) : const Seed&
    +get(const std::string& fingerprint) : const Seed&
    +has(const seed_handle_t handle) : bool
    +has(const std::string& fingerprint) : bool
    +list() : const std::vector<Seed&>
}
```

```mermaid
classDiagram

class SecureKeyEntry {
    std::unique_ptr<crypto::secret_key> key
    std::string label
    uint64_t access_count = 0
    Seed* seed = null_ptr
    Network network = Network::MAIN
}

class KeyJar {
    +instance()$ : const KeyJar&
    +store(const crypto::secret_key& key, const std::string& label = std::string(), Seed* seed = null_ptr) : key_handle_t
    +get(const std::string& label) : const crypto::secret_key
    +get(const Seed& seed) : const crypto::secret_key
    +get(key_handle_t handle) : const crypto::secret_key
    +has(const std::string& label) : bool
    +has(const Seed& seed) : bool
    +remove(key_handle_t handle) : bool
    +getSeed(key_handle_t handle) : const Seed&
    +getLabel(key_handle_t handle) : const std::string&
    +getNetwork(key_handle_t handle) : Network
    +getWallet(key_handle_t handle) : Wallet
}
```

```mermaid
classDiagram

class Wallet {
    +address(int account = 0, int index = 0) : const Address&
    +accounts(int max = 10) : const std::vector<const Address&>
    +subAddresses(int account = 0, int max = 10) : const std::vector<const Address&>
    +hasAddress(const std::string& address) : bool
    +hasAddress(const Address& address) : bool
    +addressIndex(const std::string& address) : std::pair<int, int>
    +addressIndex(const Address& address) : std::pair<int, int>
    +importOutputs(const std::string& outputs) : uint64_t
    +exportKeyImages() : const std::string
    +describeTransaction(const std::string& unsignedTransaction) : TxDescription
    +checkTransaction(const std::string& unsignedTransaction) : const std::vector<TxWarning>
    +checkTransaction(const TxDescription& description) : const std::vector<TxWarning>
    +signTransaction(const std::string& unsignedTransaction) : const std::string
    +signData(const std::string& data) : const std::string
    +verifyData(const std::string& data, const std::string& address, const std::string& signature) : bool
}

class  TxDescription

class TxWarning
```
