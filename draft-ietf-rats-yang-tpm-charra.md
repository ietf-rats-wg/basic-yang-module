---
title: A YANG Data Model for Challenge-Response-based Remote Attestation Procedures using TPMs
abbrev: YANG-CHARRA for TPMs
docname: draft-ietf-rats-yang-tpm-charra-latest
wg: RATS Working Group
stand_alone: true
ipr: trust200902
area: Security
kw: Internet-Draft
cat: std
pi:
  toc: yes
  sortrefs: yes
  symrefs: yes

author:
- ins: H. Birkholz
  name: Henk Birkholz
  org: Fraunhofer SIT
  abbrev: Fraunhofer SIT
  email: henk.birkholz@sit.fraunhofer.de
  street: Rheinstrasse 75
  code: '64295'
  city: Darmstadt
  country: Germany
- ins: M. Eckel
  name: Michael Eckel
  org: Fraunhofer SIT
  abbrev: Fraunhofer SIT
  email: michael.eckel@sit.fraunhofer.de
  street: Rheinstrasse 75
  code: '64295'
  city: Darmstadt
  country: Germany
- ins: S. Bhandari
  name: Shwetha Bhandari
  org: ThoughtSpot
  abbrev: ThoughtSpot
  email: shwetha.bhandari@thoughtspot.com
- ins: E. Voit
  name: Eric Voit
  org: Cisco Systems
  abbrev: Cisco
  email: evoit@cisco.com
- ins: B. Sulzen
  name: Bill Sulzen
  org: Cisco Systems
  abbrev: Cisco
  email: bsulzen@cisco.com
- ins: L. Xia
  name: Liang Xia (Frank)
  org: Huawei Technologies
  abbrev: Huawei
  email: Frank.Xialiang@huawei.com
  street: 101 Software Avenue, Yuhuatai District
  city: Nanjing
  region: Jiangsu
  code: '210012'
  country: China
- ins: T. Laffey
  name: Tom Laffey
  org: Hewlett Packard Enterprise
  abbrev: HPE
  email: tom.laffey@hpe.com
- ins: G. Fedorkow
  name: Guy C. Fedorkow
  org: Juniper Networks
  abbrev: Juniper
  email: gfedorkow@juniper.net
  street: 10 Technology Park Drive
  city: Westford
  code: '01886'
  region: Massachusetts

normative:
  RFC2104:
  RFC6020:
  RFC3688:
  RFC6991: ietf-yang-types
  RFC8348: ietf-hardware
  RFC6241:
  RFC8040:
  RFC6242:
  RFC6933:
  RFC8446:
  RFC8341:
  RFC8032:
  RFC8017:
  RFC9334: rats-architecture

  I-D.ietf-netconf-keystore: ietf-keystore
  I-D.ietf-rats-tpm-based-network-device-attest: RIV

  TPM1.2:
    target: https://trustedcomputinggroup.org/resource/tpm-main-specification/
    title: "TPM 1.2 Main Specification"
    author:
      -
        ins: TCG
        name: Trusted Computing Group
    date: 2003-10-02

  TPM1.2-Structures:
    target: https://trustedcomputinggroup.org/wp-content/uploads/TPM-Main-Part-2-TPM-Structures_v1.2_rev116_01032011.pdf
    title: "TPM Main Part 2 TPM Structures"

  TPM1.2-Commands:
    target: https://trustedcomputinggroup.org/wp-content/uploads/TPM-Main-Part-3-Commands_v1.2_rev116_01032011.pdf
    title: "TPM Main Part 3 Commands"

  TPM2.0:
    target: https://trustedcomputinggroup.org/resource/tpm-library-specification/
    title: "TPM 2.0 Library Specification"
    author:
      -
        ins: TCG
        name: Trusted Computing Group
    date: 2013-03-15

  TPM2.0-Arch:
    target: https://trustedcomputinggroup.org/wp-content/uploads/TCG_TPM2_r1p59_Part1_Architecture_pub.pdf
    title: "Trusted Platform Module Library - Part 1: Architecture"

  TPM2.0-Structures:
    target: https://trustedcomputinggroup.org/wp-content/uploads/TPM-Rev-2.0-Part-2-Structures-01.38.pdf
    title: "Trusted Platform Module Library - Part 2: Structures"

  TPM2.0-Key:
    target: https://trustedcomputinggroup.org/wp-content/uploads/TPM-2p0-Keys-for-Device-Identity-and-Attestation_v1_r12_pub10082021.pdf
    title: "TPM 2.0 Keys for Device Identity and Attestation, Rev12"
    author:
      -
        ins: TCG
        name: Trusted Computing Group
    date: 2021-10-08

  TCG-Algos:
    target: https://trustedcomputinggroup.org/wp-content/uploads/TCG-_Algorithm_Registry_r1p32_pub.pdf
    title: "TCG Algorithm Registry"

  BIOS-Log-Event-Type:
    target: https://trustedcomputinggroup.org/wp-content/uploads/TCG_PCClient_PFP_r1p05_v23_pub.pdf
    title: "TCG PC Client Platform Firmware Profile Specification"

  ISO-IEC-9797-1:
    target: https://www.iso.org/standard/50375.html
    title: "Message Authentication Codes (MACs) - ISO/IEC 9797-1:2011"

  ISO-IEC-9797-2:
    target: https://www.iso.org/standard/51618.html
    title: "Message Authentication Codes (MACs) - ISO/IEC 9797-2:2011"

  ISO-IEC-10116:
    target: https://www.iso.org/standard/64575.html
    title: "ISO/IEC 10116:2017 - Information technology"

  ISO-IEC-10118-3:
    target: https://www.iso.org/standard/67116.html
    title: "Dedicated hash-functions - ISO/IEC 10118-3:2018"

  ISO-IEC-14888-3:
    target: https://www.iso.org/standard/76382.html
    title: "ISO/IEC 14888-3:2018 - Digital signatures with appendix"

  ISO-IEC-15946-1:
    target: https://www.iso.org/standard/65480.html
    title: "ISO/IEC 15946-1:2016 - Information technology"

  ISO-IEC-18033-3:
    target: https://www.iso.org/standard/54531.html
    title: "ISO/IEC 18033-3:2010 - Encryption algorithms"

  IEEE-Std-1363-2000:
    target: https://standards.ieee.org/standard/1363-2000.html
    title: "IEEE 1363-2000 - IEEE Standard Specifications for Public-Key Cryptography"

  IEEE-Std-1363a-2004:
    target: https://ieeexplore.ieee.org/document/1335427
    title: "1363a-2004 - IEEE Standard Specifications for Public-Key Cryptography - Amendment 1: Additional Techniques"

  NIST-PUB-FIPS-202:
    target: https://csrc.nist.gov/publications/detail/fips/202/final
    title: "SHA-3 Standard: Permutation-Based Hash and Extendable-Output Functions"

  NIST-SP800-38C:
    target: https://csrc.nist.gov/publications/detail/sp/800-38c/final
    title: "Recommendation for Block Cipher Modes of Operation: the CCM Mode for Authentication and Confidentiality"

  NIST-SP800-38D:
    target: https://csrc.nist.gov/publications/detail/sp/800-38d/final
    title: "Recommendation for Block Cipher Modes of Operation: Galois/Counter Mode (GCM) and GMAC"

  NIST-SP800-38F:
    target: https://csrc.nist.gov/publications/detail/sp/800-38f/final
    title: "Recommendation for Block Cipher Modes of Operation: Methods for Key Wrapping"

  NIST-SP800-56A:
    target: https://csrc.nist.gov/publications/detail/sp/800-56a/rev-3/final
    title: "Recommendation for Pair-Wise Key-Establishment Schemes Using Discrete Logarithm Cryptography"

  NIST-SP800-108:
    target: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf
    title: "Recommendation for Key Derivation Using Pseudorandom Functions"

  bios-log:
    target: https://trustedcomputinggroup.org/wp-content/uploads/PC-ClientSpecific_Platform_Profile_for_TPM_2p0_Systems_v51.pdf
    title: "TCG PC Client Platform Firmware Profile Specification, Section 9.4.5.2"

  cel:
    target: https://trustedcomputinggroup.org/wp-content/uploads/TCG_IWG_CEL_v1_r0p41_pub.pdf
    title: "Canonical Event Log Format, Section 4.3"

  UEFI-Secure-Boot:
    target: https://uefi.org/sites/default/files/resources/UEFI_Spec_2_9_2021_03_18.pdf
    title: "Unified Extensible Firmware Interface (UEFI) Specification Version 2.9 (March 2021), Section 32.1 (Secure Boot)"

informative:
  I-D.ietf-rats-reference-interaction-models: rats-interaction-models

  IMA-Kernel-Source:
    target: https://github.com/torvalds/linux/blob/df0cc57e057f18e44dac8e6c18aba47ab53202f9/security/integrity/ima/
    title: "Linux Integrity Measurement Architecture (IMA): Kernel Sourcecode"

  NIST-915121:
    target: https://tsapps.nist.gov/publication/get_pdf.cfm?pub_id=915121
    title: "True Randomness Can’t be Left to Chance: Why entropy is important for information security"

  yang-parameters:
    target: https://www.iana.org/assignments/yang-parameters/yang-parameters.xhtml
    title: YANG Parameters

  xml-registry:
    target: https://www.iana.org/assignments/xml-registry/xml-registry.xhtml
    title: IETF XML Registry

--- abstract

This document defines YANG Remote Procedure Calls (RPCs) and a few configuration nodes required to retrieve attestation evidence about integrity measurements from a device, following the operational context defined in TPM-based Network Device Remote Integrity Verification. Complementary measurement logs are also provided by the YANG RPCs, originating from one or more roots of trust for measurement (RTMs). The module defined requires at least one TPM 1.2 or TPM 2.0 as well as a corresponding TPM Software Stack (TSS), or equivalent hardware implementations that include the protected capabilities as provided by TPMs as well as a corresponding software stack, included in the device components of the composite device the YANG server is running on.

--- middle

# Introduction

This document is based on the general terminology defined in the {{-rats-architecture}} and uses the operational context defined in {{-RIV}} as well as the interaction model and information elements defined in {{-rats-interaction-models}}. The currently supported hardware security modules (HSMs) are the Trusted Platform Modules (TPMs) {{TPM1.2}} and {{TPM2.0}} as specified by the Trusted Computing Group (TCG). One TPM, or multiple TPMs in the case of a Composite Device, are required in order to use the YANG module defined in this document. Each TPM is used as a root of trust for storage (RTS) in order to store system security measurement Evidence.  And each TPM is used as a root of trust for reporting (RTR) in order to retrieve attestation Evidence.  This is done by using a YANG RPC to request a quote which exposes a rolling hash of the security measurements held internally within the TPM.

Specific terms imported from {{-rats-architecture}} and used in this document include: Attester, Composite Device, Evidence.

Specific terms imported from {{TPM2.0-Key}} and used in this document include: Endorsement Key (EK), Initial Attestation Key (IAK), Attestation Identity Key (AIK), Local Attestation Key (LAK).

## Requirements notation

{::boilerplate bcp14}

# The YANG Module for Basic Remote Attestation Procedures

One or more TPMs MUST be embedded in a Composite Device that provides attestation Evidence via the YANG module defined in this document. The ietf-tpm-remote-attestation YANG module enables a composite device to take on the role of an Attester, in accordance with the Remote Attestation Procedures (RATS) architecture {{-rats-architecture}}, and the corresponding challenge-response interaction model defined in the {{-rats-interaction-models}} document. A fresh nonce with an appropriate amount of entropy {{NIST-915121}} MUST be supplied by the YANG client in order to enable a proof-of-freshness with respect to the attestation Evidence provided by the Attester running the YANG datastore. Further, this nonce is used to prevent replay attacks. The method for communicating the relationship of each individual TPM to specific measured component within the Composite Device is out of the scope of this document.

## YANG Modules

In this section the several YANG modules are defined.

### 'ietf-tpm-remote-attestation'

This YANG module imports modules from {{-ietf-yang-types}} with prefix 'yang', {{-ietf-hardware}} with prefix 'hw', {{-ietf-keystore}} with prefix 'ks', and 'ietf-tcg-algs.yang' {{ref-ietf-tcg-algs}} with prefix 'taa'.  Additionally, references are made to {{RFC8032}}, {{RFC8017}}, {{RFC6933}}, {{TPM1.2-Commands}}, {{TPM2.0-Arch}}, {{TPM2.0-Structures}}, {{TPM2.0-Key}}, {{TPM1.2-Structures}}, {{bios-log}}, {{BIOS-Log-Event-Type}}, as well as {{ima}} and {{netequip-boot-log}}.

#### Features

This module supports the following features:

- 'mtpm': Indicates that multiple TPMs on the device can support remote attestation. For example, this feature could be used in cases where multiple line cards are present, each with its own TPM.

- 'bios': Indicates that the device supports the retrieval of BIOS/UEFI event logs. {{bios-log}}

- 'ima': Indicates that the device supports the retrieval of event logs from the Linux Integrity Measurement Architecture (IMA, see {{ima}}).

- 'netequip_boot': Indicates that the device supports the retrieval of netequip boot event logs. See {{ima}} and {{netequip-boot-log}}.

#### Identities

This module supports the following types of attestation event logs: 'bios', 'ima', and 'netequip_boot'.

#### Remote Procedure Calls (RPCs)

In the following, RPCs for both TPM 1.2 and TPM 2.0 attestation procedures are defined.

##### 'tpm12-challenge-response-attestation'

This RPC allows a Verifier to request signed TPM PCRs (*TPM Quote* operation) from a TPM 1.2 compliant cryptoprocessor. Where the feature 'mtpm' is active, and one or more 'certificate-name' is not provided, all TPM 1.2 compliant cryptoprocessors will respond. A YANG tree diagram of this RPC is as follows:

~~~ TREE
{::include-dedent tpm12-challenge-response-attestation.tree}
~~~

##### 'tpm20-challenge-response-attestation'

This RPC allows a Verifier to request signed TPM PCRs (*TPM Quote* operation) from a TPM 2.0 compliant cryptoprocessor. Where the feature 'mtpm' is active, and one or more 'certificate-name' is not provided, all TPM 2.0 compliant cryptoprocessors will respond. A YANG tree diagram of this RPC is as follows:

~~~ TREE
{::include-dedent tpm20-challenge-response-attestation.tree}
~~~

An example of an RPC challenge requesting PCRs 0-7 from a SHA-256 bank could look like the following:

~~~
<rpc message-id="101" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <tpm20-attestation-challenge
      xmlns="urn:ietf:params:xml:ns:yang:ietf-tpm-remote-attestation">
    <certificate-name>
      (identifier of a TPM signature key with which the Attester is
      supposed to sign the attestation data)
    </certificate-name>
    <nonce>
      0xe041307208d9f78f5b1bbecd19e2d152ad49de2fc5a7d8dbf769f6b8ffdeab9
    </nonce>
    <tpm20-pcr-selection>
      <tpm20-hash-algo
          xmlns="urn:ietf:params:xml:ns:yang:ietf-tcg-algs">
        TPM_ALG_SHA256
      </tpm20-hash-algo>
      <pcr-index>0</pcr-index>
      <pcr-index>1</pcr-index>
      <pcr-index>2</pcr-index>
      <pcr-index>3</pcr-index>
      <pcr-index>4</pcr-index>
      <pcr-index>5</pcr-index>
      <pcr-index>6</pcr-index>
      <pcr-index>7</pcr-index>
    </tpm20-pcr-selection>
  </tpm20-attestation-challenge>
</rpc>
~~~

A successful response could be formatted as follows:

~~~
<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <tpm20-attestation-response
      xmlns="urn:ietf:params:xml:ns:yang:ietf-tpm-remote-attestation">
    <certificate-name
        xmlns="urn:ietf:params:xml:ns:yang:ietf-keystore">
      (instance of Certificate name in the Keystore)
    </certificate-name>
    <attestation-data>
      (raw attestation data, i.e., the TPM quote; this includes,
      among other information, a composite digest of requested PCRs,
      the nonce, and TPM 2.0 clock information.)
    </attestation-data>
    <quote-signature>
      (signature over attestation-data using the TPM key
      identified by sig-key-id)
    </quote-signature>
  </tpm20-attestation-response>
</rpc-reply>
~~~

#### 'log-retrieval'

This RPC allows a Verifier to acquire the Evidence which was extended into specific TPM PCRs. A YANG tree diagram of this RPC is as follows:

~~~ TREE
{::include-dedent log-retrieval.tree}
~~~

#### Data Nodes

This section provides a high level description of the data nodes containing the configuration and operational objects with the YANG model. For more details, please see the YANG model itself in {{ref-ietf-tpm-remote-attestation}}.

Container 'rats-support-structures':

: This houses the set of information relating to remote attestation for a device. This includes specific device TPM(s), the compute nodes (such as line cards) on which the TPM(s) reside, and the algorithms supported across the platform.

Container 'tpms':

: Provides configuration and operational details for each supported TPM, including the tpm-firmware-version, PCRs which may be quoted, certificates which are associated with that TPM, and the current operational status. Of note are the certificates which are associated with that TPM. As a certificate is associated with a particular TPM attestation key, knowledge of the certificate allows a specific TPM to be identified.

~~~ TREE
{::include-dedent tpms.tree}
~~~

container 'attester-supported-algos' - Identifies which TCG hash algorithms are available for use on the Attesting platform. An operator will use this information to limit algorithms available for use by RPCs to just a desired set from the universe of all allowed hash algorithms by the TCG.

~~~ TREE
{::include-dedent attester-supported-algos.tree}
~~~

container 'compute-nodes' - When there is more than one TPM supported, this container maintains the set of information related to the compute node associated with a specific TPM. This allows each specific TPM to identify to which 'compute-node' it belongs.

~~~ TREE
{::include-dedent compute-nodes.tree}
~~~

#### YANG Module

{: #ref-ietf-tpm-remote-attestation}

~~~ YANG
<CODE BEGINS> file "ietf-tpm-remote-attestation.yang@24-07-29.yang"
{::include-dedent ietf-tpm-remote-attestation.yang}
<CODE ENDS>
~~~

### 'ietf-tcg-algs'

This document has encoded the TCG Algorithm definitions of {{TCG-Algos}}, revision 1.32. By including this full table as a separate YANG file within this document, it is possible for other YANG models to leverage the contents of this model. Specific references to {{RFC2104}}, {{RFC8017}}, {{ISO-IEC-9797-1}}, {{ISO-IEC-9797-2}}, {{ISO-IEC-10116}}, {{ISO-IEC-10118-3}}, {{ISO-IEC-14888-3}}, {{ISO-IEC-15946-1}}, {{ISO-IEC-18033-3}}, {{IEEE-Std-1363-2000}}, {{IEEE-Std-1363a-2004}}, {{NIST-PUB-FIPS-202}}, {{NIST-SP800-38C}}, {{NIST-SP800-38D}}, {{NIST-SP800-38F}}, {{NIST-SP800-56A}}, {{NIST-SP800-108}}, {{bios-log}}, as well as {{ima}} and {{netequip-boot-log}} exist within the YANG Model.

#### Features

There are two types of features supported: 'TPM12' and 'TPM20'. Support for either of these features indicates that a cryptoprocessor supporting the corresponding type of TCG TPM API is present on an Attester. Most commonly, only one type of cryptoprocessor will be available on an Attester.

#### Identities

There are three types of identities in this model:

1. Cryptographic functions supported by a TPM algorithm; these include: 'asymmetric', 'symmetric', 'hash', 'signing', 'anonymous_signing', 'encryption_mode', 'method', and 'object_type'. The definitions of each of these are in Table 2 of {{TCG-Algos}}.

2. API specifications for TPM types: 'tpm12' and 'tpm20'

3. Specific algorithm types: Each algorithm type defines what cryptographic functions may be supported, and on which type of API specification. It is not required that an implementation of a specific TPM will support all algorithm types. The contents of each specific algorithm mirrors what is in Table 3 of {{TCG-Algos}}.

{: #ref-ietf-tcg-algs}

#### YANG Module

~~~~ YANG
<CODE BEGINS> file "ietf-tcg-algs@2022-03-23.yang"
{::include-dedent ietf-tcg-algs@2021-11-05.yang}
<CODE ENDS>
~~~~

Note that not all cryptographic functions are required for use by `ietf-tpm-remote-attestation.yang`. However, the full definition of Table 3 of {{TCG-Algos}} will allow use by additional YANG specifications.

# IANA Considerations

This document registers the following namespace URIs in the
{{xml-registry}} as per {{RFC3688}}:

URI:
: urn:ietf:params:xml:ns:yang:ietf-tpm-remote-attestation

  Registrant Contact:
  : The IESG.

  XML:
  : N/A; the requested URI is an XML namespace.

URI:
: urn:ietf:params:xml:ns:yang:ietf-tcg-algs

  Registrant Contact:
  : The IESG.

  XML:
  : N/A; the requested URI is an XML namespace.


This document registers the following YANG modules in the
registry {{yang-parameters}} as per Section 14 of {{RFC6020}}:

Name:
: ietf-tpm-remote-attestation

  Namespace:
  : urn:ietf:params:xml:ns:yang:ietf-tpm-remote-attestation

  Prefix:
  : tpm

  Reference:
  : draft-ietf-rats-yang-tpm-charra (RFC form)

Name:
: ietf-tcg-algs

  Namespace:
  : urn:ietf:params:xml:ns:yang:ietf-tcg-algs

  Prefix:
  : taa

  Reference:
  : draft-ietf-rats-yang-tpm-charra (RFC form)

# Security Considerations

The YANG module ietf-tpm-remote-attestation.yang specified in this document defines a schema for data that is designed to be accessed via network management protocols such as NETCONF {{RFC6241}} or RESTCONF {{RFC8040}}. The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{RFC6242}}. The lowest RESTCONF layer is HTTPS, and the mandatory-to-implement secure transport is TLS {{RFC8446}}.

There are a number of data nodes defined in this YANG module that are writable/creatable/deletable (i.e., *config true*, which is the default). These data nodes may be considered sensitive or vulnerable in some network environments. Write operations (e.g., *edit-config*) to these data nodes without proper protection can have a negative effect on network operations. These are the subtrees and data nodes as well as their sensitivity/vulnerability:

Container '/rats-support-structures/attester-supported-algos':

: 'tpm12-asymmetric-signing', 'tpm12-hash', 'tpm20-asymmetric-signing', and 'tpm20-hash'. All could be populated with algorithms that are not supported by the underlying physical TPM installed by the equipment vendor. A vendor should restrict the ability to configure unsupported algorithms.

Container: '/rats-support-structures/tpms':

: 'name': Although shown as 'rw', it is system generated. Therefore, it should not be possible for an operator to add or remove a TPM from the configuration.

: 'tpm20-pcr-bank': It is possible to configure PCRs for extraction which are not being extended by system software. This could unnecessarily use TPM resources.

: 'certificates': It is possible to provision a certificate which does not correspond to an Attestation Identity Key (AIK) within the TPM 1.2, or an Attestation Key (AK) within the TPM 2.0 respectively. In such a case, calls to an RPC requesting this specific certificate could result in either no response or a response for an unexpected TPM.

RPC 'tpm12-challenge-response-attestation':

: The receiver of the RPC response must verify that the certificate is for an active AIK, i.e., the certificate has been confirmed by a third party as being able to support Attestation on the targeted TPM 1.2.

RPC 'tpm20-challenge-response-attestation':

: The receiver of the RPC response must verify that the certificate is for an active AK, i.e., the private key confirmation of the quote signature within the RPC response has been confirmed by a third party to belong to an entity legitimately able to perform Attestation on the targeted TPM 2.0.

RPC 'log-retrieval':

: Requesting a large volume of logs from the Attester could require significant system resources and create a denial of service.


Information collected through the RPCs above could reveal that specific versions of software and configurations of endpoints that could identify vulnerabilities on those systems. Therefore, RPCs should be protected by NACM {{RFC8341}} with a default setting of deny-all to limit the extraction of attestation data by only authorized Verifiers.

For the YANG module ietf-tcg-algs.yang, please use care when selecting specific algorithms. The introductory section of {{TCG-Algos}} highlights that some algorithms should be considered legacy, and recommends implementers and adopters diligently evaluate available information such as governmental, industrial, and academic research before selecting an algorithm for use.


--- back

# Integrity Measurement Architecture (IMA) {#ima}

IMA extends the principles of Measured Boot {{TPM2.0-Arch}} and Secure Boot {{UEFI-Secure-Boot}} to the Linux operating system, applying it to operating system applications and files.
IMA has been part of the Linux integrity subsystem of the Linux kernel since 2009 (kernel version 2.6.30). The IMA mechanism represented by the YANG module in this specification is rooted in the kernel version 5.16 {{IMA-Kernel-Source}}.
IMA enables the protection of system integrity by collecting (commonly referred to as measuring) and storing measurements (called Claims in the context of IETF RATS) of files before execution so that these measurements can be used later, at system runtime, in remote attestation procedures.
IMA acts in support of the appraisal of Evidence (which includes measurement Claims) by leveraging Reference Values stored in extended file attributes.

In support of the appraisal of Evidence, IMA maintains an ordered list (with no duplicates) of measurements in kernel-space, the Stored Measurement Log (SML), for all files that have been measured before execution since the operating system was started.
Although IMA can be used without a TPM, it is typically used in conjunction with a TPM to anchor the integrity of the SML in a hardware-protected secure storage location, i.e., Platform Configuration Registers (PCRs) provided by TPMs.
IMA provides the SML in both binary and ASCII representations in the Linux security file system *securityfs* (`/sys/kernel/security/ima/`).

IMA templates define the format of the SML, i.e., which fields are included in a log record.
Examples are file path, file hash, user ID, group ID, file signature, and extended file attributes.
IMA comes with a set of predefined template formats and also allows a custom format, i.e., a format consisting of template fields supported by IMA.
Template usage is typically determined by boot arguments passed to the kernel.
Alternatively, the format can also be hard-coded into custom kernels.
IMA templates and fields are extensible in the kernel source code.
As a result, more template fields can be added in the future.

IMA policies define which files are measured using the IMA policy language.
Built-in policies can be passed as boot arguments to the kernel.
Custom IMA policies can be defined once during runtime or be hard-coded into a custom kernel.
If no policy is defined, no measurements are taken and IMA is effectively disabled.

A comprehensive description of the content fields in native Linux IMA TLV format can be found in Table 16 of the Canonical Event Log (CEL) specification {{cel}}. The CEL specification also illustrates the use of templates to enable extended or customized IMA TLV formats in Section 5.1.6.

# IMA for Network Equipment Boot Logs {#netequip-boot-log}

Network equipment can generally implement similar IMA-protected functions to generate measurements (Claims) about the boot process of a device and enable corresponding remote attestation.
Network Equipment Boot Logs combine the measurement and logging of boot components and operating system components (executables and files) into a single log file in a format identical to the IMA format.
Note that the format used for logging measurement of boot components in this scheme differs from the boot logging strategy described elsewhere in this document.

During the boot process of the network device, i.e., from BIOS to the end of the operating system and user-space, all files executed can be measured and logged in the order of their execution.
When the Verifier initiates a remote attestation process (e.g., challenge-response remote attestation as defined in this document), the network equipment takes on the role of an Attester and can convey to the Verifier Claims that comprise the measurement log as well as the corresponding PCR values (Evidence) of a TPM.

The Verifier can appraise the integrity (compliance with the Reference Values) of each executed file by comparing its measured value with the Reference Value.
Based on the execution order, the Verifier can compute a PCR Reference Value (by replaying the log) and compare it to the Measurement Log Claims obtained in conjunction with the PCR Evidence to assess their trustworthiness with respect to an intended operational state.

Network equipment usually executes multiple components in parallel. This holds not only during the operating system loading phase, but also even during the BIOS boot phase.
With this measurement log mechanism, network equipment can take on the role of an Attester, proving to the Verifier the trustworthiness of its boot process.
Using the measurement log, Verifiers can precisely identify mismatching log entries to infer potentially tampered components.

This mechanism also supports scenarios that modify files on the Attester that are subsequently executed during the boot phase (e.g., updating/patching) by simply updating the appropriate Reference Values in Reference Integrity Manifests that inform Verifiers about how an Attester is composed.
