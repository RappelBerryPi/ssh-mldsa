---
title: "SSH Support of ML-DSA"
abbrev: "SSH ML-DSA"
category: info

docname: draft-sfluhrer-ssh-mldsa-06
submissiontype: IETF
consensus: true
date:
v: 3
area: "sec"
workgroup: "sshm"
stand_alone: true
ipr: trust200902
keyword:
 - ML-DSA
coding: UTF-8
venue:
  group: "Secure Shell Maintenance"
  type: "Security Area"
  mail: "ssh@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ssh/"
  github: "sfluhrer/ssh-mldsa"
  latest: "https://sfluhrer.github.io/ssh-mldsa/draft-sfluhrer-ssh-mldsa.html"

author:
  - fullname: "Scott Fluhrer"
    organization: Cisco Systems
    email: "sfluhrer@cisco.com"
  - fullname: "Ryan Appel"
    organization: Bank of America
    email: "ryan.appel@bofa.com"

normative:

  FIPS204:
    target: https://doi.org/10.6028/NIST.FIPS.204
    title: Module-Lattice-Based Digital Signature Standard
    seriesinfo:
      "NIST": "FIPS 204"
    date: August 2024

informative:

  RFC4250:
  RFC4251:
  RFC4253:
  RFC4255:
  IANA-SSH:
    target: https://www.iana.org/assignments/ssh-parameters
    title: Secure Shell (SSH) Protocol Parameters
  IANA-SSHFP:
    target: https://www.iana.org/assignments/dns-sshfp-rr-parameters)
    title: DNS SSHFP Resource Record Parameters

--- abstract

   This document describes the use of ML-DSA digital
   signatures in the Secure Shell (SSH) protocol.

--- middle

# Introduction

<<<<<<< HEAD
   A Cryptographically Relevant Quantum Computer (CRQC) is a quantum computer with sufficient compute capability and stability that it is able to break traditional asymmetric cryptographic algorithms:
   e.g RSA, ECDSA; which are currently the only authentication algorithms available for SSH.
   NIST has recently published the post-quantum cryptography (PQC) algorithm known as ML-DSA [FIPS204] which is a digital signature algorithm.
=======
   A Cryptographically Relevant Quantum Computer (CRQC) is a quantum computer with sufficient compute capability and
   stability that it is able to break traditional asymmetric cryptographic algorithms: e.g RSA, ECDSA; which are
   currently the only authentication algorithms available for SSH. NIST has recently published the post-quantum
   cryptography (PQC) algorithm known as ML-DSA [FIPS204] which is a digital signature algorithm.
>>>>>>> a130eec (Making changes as discussed with Mr. Fluhrer on the mailing list)

   This document describes how to use this algorithm for authentication within SSH [RFC4251], as a replacement for the
   traditional signature algorithms (e.g. RSA, ECDSA).

## Background on ML-DSA

<<<<<<< HEAD
   ML-DSA (as specified in FIPS 204) is a signature algorithm that is believed to be secure against attackers who have a CRQC available to them.
   There are three parameter sets defined for it which belong to a respective NIST Security Category of 2, 3, and 5 (ML-DSA-44, ML-DSA-65, and ML-DSA-87).
   In addition, for each defined parameter set, there are two versions, the 'pure' version (where ML-DSA computes the hash of the message internally and then signs that hash),
   and a 'prehashed' version (where ML-DSA signs a hash that was computed outside of ML-DSA).
   For this protocol, we will always use the pure version.
=======
   ML-DSA (as specified in FIPS 204) is a signature algorithm that is believed to be secure against attackers who have a
<<<<<<< HEAD
   CRQC available to them. There are three parameter sets defined for it which belong to a respective Security Category
   of 2, 3, and 5 (ML-DSA-44, ML-DSA-65, and ML-DSA-87). In addition, for each defined parameter set, there are two
   versions, the 'pure' version (where ML-DSA computes the hash internally of the message and then signs the internally
   computed hash), and a 'prehashed' version (where ML-DSA signs a hash that was computed outside of ML-DSA). For this
   protocol, we will always use the pure version.
>>>>>>> a130eec (Making changes as discussed with Mr. Fluhrer on the mailing list)
=======
   CRQC available to them. There are three parameter sets defined for it which belong to a respective NIST Security
   Category of 2, 3, and 5 (ML-DSA-44, ML-DSA-65, and ML-DSA-87). In addition, for each defined parameter set, there are
   two versions, the 'pure' version (where ML-DSA computes the hash of the message internally and then signs that hash),
   and a 'prehashed' version (where ML-DSA signs a hash that was computed outside of ML-DSA). For this protocol, we will
   always use the pure version.
>>>>>>> fc95c63 (Small text updates)

   In addition, ML-DSA also has a 'context' input, which is a short string that is common to the sender and the
   receiver. It is intended to allow for domain separation between separate uses of the same public key. This protocol
   always uses an empty (zero length) context.

   FIPS 204 also allows ML-DSA to be run in either deterministic or 'hedged' mode (where randomness is applied during
   the signature generation operation). We recommend that implementations use hedged mode, as it blocks certain side
   channel attacks. However, as both are interoperable, we do not place any requirement on which is used within this
   protocol.

<<<<<<< HEAD
   FIPS 204 also allows ML-DSA to be run in either determanistic or 'hedged' mode (where randomness is applied during the signature generation operation).
   We recommend that implementations use hedged mode, as it blocks certain side channel attacks.
   However, as both are interoperable, we do not place any requirement on which is used within this protocol.

=======
>>>>>>> a130eec (Making changes as discussed with Mr. Fluhrer on the mailing list)
# Conventions and Definitions

{::boilerplate bcp14-tagged}

   The descriptions of key and signature formats use the notation
   introduced in [RFC4251], Section 3, and the string data type from
   [RFC4251], Section 5.  Identifiers and terminology from ML-DSA
   [FIPS204] are used throughout the document.

# Public Key Algorithms

<<<<<<< HEAD
<<<<<<< HEAD
   This document describes three public key algorithms for use with SSH, as
   per [RFC4253], Section 6.6, corresponding to the three parameter sets of ML-DSA.
   The names of the algorithms are "ssh-mldsa-44", "ssh-mldsa-65" and "ssh-mldsa-87", to match the level 2, 3 and 5 parameter sets [FIPS204].
   These algorithm support only signing; it does not support encryption.
=======
   This document describes one public key algorithm for use with SSH, as per [RFC4253], Section 6.6, corresponding to
   the three parameter sets of ML-DSA. The name of the algorithm is "ssh-mldsa", and internally uses the public key
   format as a domain separator between the parameter sets [FIPS204].  This algorithm only supports signing, it does not
   support encryption.
>>>>>>> a130eec (Making changes as discussed with Mr. Fluhrer on the mailing list)
=======
   This document describes three public key algorithms for use with SSH, as per [RFC4253], Section 6.6, corresponding to
   the three parameter sets of ML-DSA. The names of the algorithms are "ssh-mldsa-44", "ssh-mldsa-65" and
   "ssh-mldsa-87", to match the level 2, 3 and 5 parameter sets [FIPS204]. These algorithms support only
   signing/verification; they do not support encryption/decryption.
>>>>>>> fc95c63 (Small text updates)

   The below table lists the public key sizes and the signature size (in bytes) for the three parameter sets.

   | Public Key Algorithm Name | Public Key Size | Signature Size |
   | ------------------------- | --------------- | -------------- |
   | ssh-mldsa-44              | 1312            | 2420           |
   | ssh-mldsa-65              | 1952            | 3309           |
   | ssh-mldsa-87              | 2592            | 4627           |

# Public Key Format

   The key format for all three parameter sets have the following encoding:

   string "ssh-mldsa-44" (or "ssh-mldsa-65" or "ssh-mldsa-87")

   string key

   Here, 'key' is the public key described in [FIPS204].

# Signature Algorithm

   Signatures are generated according to the procedure in Section 5.2 [FIPS204], using the "pure" version of ML-DSA,
   with an empty context string.

# Signature Format

   The "ssh-mldsa" signature format has the following encoding:

   string "ssh-mldsa-44" (or "ssh-mldsa-65" or "ssh-mldsa-87")

   string signature

   Here, 'signature' is the signature produced in accordance with the previous section.

# Verification Algorithm

<<<<<<< HEAD
<<<<<<< HEAD
   Signatures are verified in two steps.
   For the first step, the length of the signature must be checked against the parameter set,
   if the signature length does not match the expected signature length for the parameter set, it must be rejected.
   Then the signature is verified according to the procedure in
   [FIPS204], Section 5.3, using the "pure" version of ML-DSA, with an empty context string.
=======
   Signatures are verified in two steps. For the first step, the length must be checked against the parameter-set, if
   the signature length does not match the parameter-set then it must be rejected. Then a normal verification is run
   according to 'Algorithm 3' in Section 5.3 [FIPS204], using the "pure" version of ML-DSA, with an empty context
   string. 
>>>>>>> a130eec (Making changes as discussed with Mr. Fluhrer on the mailing list)
=======
   Signatures are verified in two steps. For the first step, the length of the signature must be checked against the
   parameter set, if the signature length does not match the expected signature length for the parameter set, it must be
   rejected. Then the signature is verified according to the procedure in [FIPS204], Section 5.3, using the "pure"
   version of ML-DSA, with an empty context string.
>>>>>>> fc95c63 (Small text updates)

# SSHFP DNS Resource Records

   Usage and generation of the SSHFP DNS resource record is described in [RFC4255].  This section illustrates the
   generation of SSHFP resource records for ML-DSA keys, and this document also specifies the corresponding code point
   to "SSHFP RR Types for public key algorithms" in the "DNS SSHFP Resource Record Parameters" IANA registry
   [IANA-SSHFP].

   The generation of SSHFP resource records keys for ML-DSA is described as follows.

<<<<<<< HEAD
<<<<<<< HEAD
   The encoding of ML-DSA public keys is described as above in section 4.
=======
   The encoding of ML-DSA public keys are described as above in section 4.
>>>>>>> a130eec (Making changes as discussed with Mr. Fluhrer on the mailing list)
=======
   The encoding of ML-DSA public keys is described as above in section 4.
>>>>>>> fc95c63 (Small text updates)

   The SSHFP Resource Record for an ML-DSA key fingerprint (with a SHA-256 fingerprint) would, for example, be:

   pqserver.example.com. IN SSHFP TBD 2 (
                    a87f1b687ac0e57d2a081a2f28267237
                    34d90ed316d2b818ca9580ea384d9240 )

   Replace TBD with the value eventually allocated by IANA.

# IANA Considerations

  This document augments the Public Key Algorithm Names in [RFC4250], Section 4.11.3.

   IANA is requested to add the following entries to "Public Key Algorithm Names" in the "Secure Shell (SSH) Protocol
   Parameters" registry [IANA-SSH]:

   | Public Key Algorithm Name | Reference |
   | ------------------------- | --------- |
   | ssh-mldsa-44              | THIS-RFC  |
   | ssh-mldsa-65              | THIS-RFC  |
   | ssh-mldsa-87              | THIS-RFC  |

   IANA is requested to add the following entries to "SSHFP RR Types for public key algorithms" in the "DNS SSHFP
   Resource Record Parameters" registry [IANA-SSHFP]:

   | Value | Description | Reference |
   | ----- | ----------- | --------- |
   | TBD1  | ML-DSA-44   | THIS RFC  |
   | TBD2  | ML-DSA-65   | THIS RFC  |
   | TBD3  | ML-DSA-87   | THIS RFC  |

# Security Considerations

   The security considerations in [RFC4251], Section 9 apply to all SSH implementations, including those using ML-DSA.

   The security considerations in ML-DSA [FIPS204] apply to all uses of ML-DSA, including those in SSH.

   Cryptographic algorithms and parameters are usually broken or weakened over time.  Implementers and users need to
   continuously re-evaluate that cryptographic algorithms continue to provide the expected level of security.

--- back

# Acknowledgments
{:numbered="false"}

   The text of draft-josefsson-ssh-sphincs was used as a template for this document.
