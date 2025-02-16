[![Build Status](https://github.com/rustls/webpki/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/rustls/webpki/actions/workflows/ci.yml?query=branch%3Amain)
[![Coverage Status (codecov.io)](https://codecov.io/gh/rustls/webpki/branch/main/graph/badge.svg)](https://codecov.io/gh/rustls/webpki/)
[![Documentation](https://docs.rs/rustls-webpki/badge.svg)](https://docs.rs/rustls-webpki/)
[![Chat](https://img.shields.io/discord/976380008299917365?logo=discord)](https://discord.gg/MCSB76RU96)

webpki is a library that validates Web PKI (TLS/SSL) certificates. It's
used by [Rustls](https://github.com/rustls/rustls) to handle certificate-related
tasks required for implementing TLS clients and servers.

webpki is written in [Rust](https://www.rust-lang.org/) and uses
[*ring*](https://github.com/briansmith/ring) for cryptographic operations and
low-level parsing.

This is a fork of the [original webpki project](https://github.com/briansmith/webpki)
which adds a number of features required by the rustls project.  This fork is
released as the `rustls-webpki` crate, with versions starting 0.100.0 so as to
not confusingly overlap with `webpki` versions.


Features
===============

* Representing trust anchors - webpki requires the caller to bootstrap trust by 
  explicitly specifying a set of trust anchors using the `TrustAnchor` type.

* Parsing certificates - webpki can convert from the raw encoded form of
  a certificate into something that can be used for making trust decisions.

* Path building - webpki can determine if a certificate for an end entity like
  a website or client identity was issued by a trust anchor, or a series of
  intermediate certificates the trust anchor has endorsed.

* Name/usage validation - webpki can determine if a certificate is valid for
  a given DNS name or IP address by considering the allowed usage of the
  certificate and additional constraints.

  
Limitations
===============

webpki offers a minimal feature set tailored to the needs of Rustls. Notably it
does not offer:

* Certificate or keypair generation
* Access to arbitrary certificate extensions
* Parsing/representation of certificate subjects, or human-friendly display of
  these fields

For these tasks you may prefer using webpki in combination with libraries like
[x509-parser](https://github.com/rusticata/x509-parser) and
[rcgen](https://github.com/est31/rcgen).


Changelog
=========

* Next release:
  - MSRV increased to Rust 1.60.
* 0.101.1 (2023-07-05)
  - Fixed 32-bit architecture compatibility.
* 0.101.0 (2023-07-04)
  - *Breaking change*: added `EndEntity::verify_is_valid_tls_client_cert`
    argument for certificate revocation lists.
  - *Breaking change*: removed `Time::try_from`.
  - *Breaking change*: removed `From<DnsNameRef<'_>` impl for `DnsName`.
  - *Breaking change*: replaced `AsRef<[u8]>` with `AsRef<str>` for `DnsNameRef`.
  - Added certificate revocation list (CRL) support.
  - Improved specificity of errors returned from
    `EndEntityCert::verify_is_valid_tls_client_cert` and
    `EndEntityCert::verify_is_valid_tls_server_cert`.
  - Improved error specificity for malformed subject alternate names.
  - Added `EndEntityCert::dns_names` method for returning a list of DNS subject
    alternate names from an end entity cert.
  - Changed `EndEntityCert::verify_is_valid_for_subject_name` to ignore invalid
    names when verifying cert is valid for a provided subject.
  - MSRV increased to Rust 1.57.
* 0.100.1 (2023-03-28)
  - Relax constraint on serial number length.
* 0.100.0 (2023-03-13) - first release of `rustls-webpki` crate.
  - Allow verification of certificates with IP address subjectAltNames.
    `EndEntityCert::verify_is_valid_for_subject_name` was added, and
    `EndEntityCert::verify_is_valid_for_dns_name` was removed.
  - Make `Error` type non-exhaustive.
  - Reject non-contiguous netmasks in IP address name constraints.
  - Name constraints of type dNSName and iPAddress now work and are tested.
    directoryName name constraints are not implemented and will prevent
    path building where they appear.
  - Relax requirement that serial numbers are positive to deal with issuers
    that cannot generate correct ASN.1 but nevertheless persist in doing so.
* 0.22.0 (2021-04-10) - last upstream release of `webpki` crate.


Demo
====

See https://github.com/rustls/rustls#example-code for an example of using
webpki.


License
=======

See [LICENSE](LICENSE). This project happily accepts pull requests without any
formal copyright/contributor license agreement.


Bug Reporting
=============

Please refer to the [SECURITY](SECURITY.md) policy for security issues. All
other bugs should be reported as [GitHub issues](https://github.com/rustls/webpki/issues/new).
