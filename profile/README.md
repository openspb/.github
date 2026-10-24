## OpenSPB

OpenSPB hosts Elixir libraries for financial messaging, cryptography and PKI.
Some implement the message formats and security protocols of the Brazilian
financial system (SPB, the Sistema de Pagamentos Brasileiro), such as Pix and
the RSFN network. The others are general-purpose: key stores, signatures and
encryption that any Elixir application can use.

They share a few rules:

* **Pure Elixir.** Everything runs on `:crypto` and `:public_key`, which come
  with OTP: no NIF, no `openssl` binary, and no runtime dependencies.
* **Written from the public specifications.** RFCs, ISO 20022 schemas and the
  manuals that Banco Central do Brasil publishes.
* **Checked against other tools.** The test suites of pkcs12 and cmsex have
  OpenSSL, Python, Java and others read what the library writes, and read
  what those tools write.

### General-purpose libraries

| | |
|---|---|
| [**pkcs12**](https://github.com/openspb/pkcs12) [![Hex.pm](https://img.shields.io/hexpm/v/pkcs12.svg)](https://hex.pm/packages/pkcs12) | Reads and writes PKCS#12 files (`.pfx`, `.p12`): the private key, its certificate and the rest of the chain, ready for `:ssl` and `:public_key`. Opens the old RC2 and 3DES files too, even where OpenSSL 3 has no legacy provider. It plays the role of `openssl pkcs12`. |
| [**cmsex**](https://github.com/openspb/cmsex) [![Hex.pm](https://img.shields.io/hexpm/v/cmsex.svg)](https://hex.pm/packages/cmsex) | Cryptographic Message Syntax (RFC 5652): signs and verifies `.p7s` files with RSA, RSASSA-PSS, ECDSA, Ed25519 and Ed448, countersignatures and certificate bundles; encrypts to certificates, shared keys and passwords; digests and MACs. It plays the role of `openssl cms`. |

### The Brazilian financial system

| | |
|---|---|
| [**isox**](https://github.com/openspb/isox) [![Hex.pm](https://img.shields.io/hexpm/v/isox.svg)](https://hex.pm/packages/isox) | ISO 20022 codec for the message catalog of the SPI, the instant payment system behind Pix: `encode/2` and `decode/1` by envelope, a dictionary with the meaning of each field, and XMLDSig signatures in the profile of the Manual de Segurança do SFN. |
| [**sinetex**](https://github.com/openspb/sinetex) [![Hex.pm](https://img.shields.io/hexpm/v/sinetex.svg)](https://hex.pm/packages/sinetex) | The security envelope of the RSFN, the network that connects financial institutions, clearing houses and Banco Central do Brasil: the 588-byte security header, Security Protocol v3 (AES-256-GCM) and v2 (3DES). |

### How they fit together

A certificate and its key usually arrive as a `.pfx`. **pkcs12** opens it and
hands the key and the certificate, in DER, to **sinetex** to build the RSFN
envelope, to **isox** to sign a Pix message, or to **cmsex** to sign a
document.

### Contributing

Issues and pull requests are welcome in each repository. To report a
vulnerability, follow the `SECURITY.md` of the library. pkcs12, sinetex and
cmsex are under the Apache-2.0 license, and isox under MIT.
