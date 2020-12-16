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
  RFC2119:
  RFC8174:
  RFC6991: ietf-yang-types
  RFC8348: ietf-hardware
  I-D.ietf-netconf-keystore: ietf-keystore
  I-D.birkholz-rats-reference-interaction-model: rats-interaction-models
  I-D.ietf-rats-architecture: rats-architecture
  I-D.ietf-rats-tpm-based-network-device-attest: RIV

  TPM1.2:
    target: https://trustedcomputinggroup.org/resource/tpm-main-specification/
    title: "TPM 1.2 Main Specification"
    author:
      -
        ins: TCG
        name: Trusted Computing Group
    date: 2003-10-02
  TPM2.0:
    target: https://trustedcomputinggroup.org/resource/tpm-library-specification/
    title: "TPM 2.0 Library Specification"
    author:
      -
        ins: TCG
        name: Trusted Computing Group
    date: 2013-03-15
  TCG-Algos: 
    target: hhttp://trustedcomputinggroup.org/resource/tcg-algorithm-registry/
    title: "TCG_Algorithm_Registry_r1p32_pub"
    
informative:
  RFC6241:
  RFC8040:
  RFC6242:
  RFC5246:


--- abstract

This document defines a YANG RPC and a minimal datastore required to retrieve attestation evidence about integrity measurements from a device following the operational context defined in {{-RIV}}. Complementary measurement logs are also provided by the YANG RPC originating from one or more roots of trust of measurement. The module defined requires at least one TPM 1.2 or TPM 2.0 and corresponding Trusted Software Stack included in the device components of the composite device the YANG server is running on.

--- middle

# Introduction

This document is based on the terminology defined in the {{-rats-architecture}} and uses the operational context defined in {{-RIV}} as well as the interaction model and information elements defined in  {{-rats-interaction-models}}. The currently supported hardware security modules (HWM) are the Trusted Platform Module (TPM) {{TPM1.2}} and {{TPM2.0}} specified by the Trusted Computing Group (TCG). One ore more TPMs embedded in the components of a composite device - sometimes also referred to as an aggregate device - are required in order to use the YANG module defined in this document. A TPM is used as a root of trust for reporting (RTR) in order to retrieve attestation evidence from a composite device (quote primitive operation). Additionally, it is used as a root of trust for storage (RTS) in order to retain shielded secrets and store system measurements using a folding hash function (extend primitive operation).

## Requirements notation

{::boilerplate bcp14}

# The YANG Module for Basic Remote Attestation Procedures

One or more TPMs MUST be embedded in the composite device that is providing attestation evidence via the YANG module defined in this document. The ietf-basic-remote-attestation YANG module enables a composite device to take on the role of Claimant and Attester in accordance with the Remote Attestation Procedures (RATS) architecture {{-rats-architecture}} and the corresponding challenge-response interaction model defined in the {{-rats-interaction-models}} document. A fresh nonce with an appropriate amount of entropy MUST be supplied by the YANG client in order to enable a proof-of-freshness with respect to the attestation evidence provided by the attester running the YANG datastore. The functions of this YANG module are restricted to 0-1 TPMs per hardware component.

## Tree Diagram

~~~ TREE
{::include ietf-tpm-remote-attestation.tree}
~~~

## YANG Modules


### ietf-tpm-remote-attestation
This YANG module imports modules from {{-ietf-yang-types}}, {{-ietf-hardware}}, {{-ietf-keystore}}, ietf-tcg-algs.yang.

#### Identities

This module supports the following types of attestation event logs: \<ima\>, \<bios\>, and \<netequip_boot\>. 

#### RPCs

\<tpm12-challenge-response-attestation\> - Allows a Verifier to request a quote of PCRs from a TPM1.2 compliant cryptoprocessor.  When one or more \<certificate-name\> is not provided, all TPM1.2 compliant cryptoprocessors will respond.

\<tpm20-challenge-response-attestation\> - Allows a Verifier to request a quote of PCRs from a TPM2.0 compliant cryptoprocessor.  When one or more \<certificate-name\> is not provided, all TPM2.0 compliant cryptoprocessors will respond.

\<log-retrieval\> - Allows a Verifier to acquire the evidence which was extended into specific PCRs.

#### Data Nodes

container \<rats-support-structures\> - This exists when there are more than one TPM for a particular Attester.  This allows each specific TPM to identify on which \<compute-node\> it belongs.

container \<tpms\> - Provides configuration and operational details for each supported TPM, including the tpm-firmware-version, PCRs which may be quoted, certificates which are associated with that TPM, and the current operational status.  Of note is the certificates which are associated with that TPM.  As a certificate is associated with a single Attestation key, knowledge of the certificate allows a specific TPM to be identified.

container \<attester-supported-algos\> - Identifies which TCG algorithms are available for use the Attesting platform.  This allows an operator to limit algorithms available for use by RPCs to just a desired set from the universe of all allowed by TCG.


#### YANG Module
~~~ YANG
<CODE BEGINS> file ietf-tpm-remote-attestation@2020-12-09.yang
{::include ietf-tpm-remote-attestation.yang}
<CODE ENDS>
~~~

###  ietf-tcg-algs

Cryptographic algorithm types were initially included within -v14 NETCONF's iana-crypto-types.yang.  Unfortunately all this content including the algorithms needed here failed to make the -v15 used WGLC.   As a result this document has encoded the TCG Algorithm definitions of {{TCG-Algos}}, revision 1.32.  By including this full table as a separate YANG file within this document, it is possible for other YANG models to leverage the contents of this model.

#### Features

There are two types of features supported \<TPM12\> and \<TPM20\>. Support for either of these features indicates that a cryptoprocessor supporting the corresponding type of TCG API is present on an Attester.  Most commonly, only one type of cryptoprocessor will be available on an Attester.

#### Identities

There are three types of identities in this model.  

The first are the cryptographic functions supportable by a TPM algorithm, these include: \<asymmetric\>, \<symmetric\>, \<hash\>, \<signing\>, \<anonymous_signing\>, \<encryption_mode\>, \<method\>, and \<object_type\>.  The definitions of each of these are in Table 2 of {{TCG-Algos}}.

The second are API specifications for tpms: \<tpm12\> and \<tpm2\>.

The third are specific algorithm types.   Each algorithm type defines what cryptographic functions may be supported, and on which type of API specification.  It is not required that an implementation of a specific TPM will support all algorithm types.  The contents of each specific algorithm mirrors what is in Table 3 of {{TCG-Algos}}.

#### YANG Module
~~~~ YANG
<CODE BEGINS> ietf-tcg-algs@2020-09-18.yang
{::include ietf-tcg-algs@2020-09-18.yang}
<CODE ENDS>
~~~~

Note that not all cryptographic functions are required for use by ietf-tpm-remote-attestation.yang.  However the full definition of Table 3 of {{TCG-Algos}} will allow use by additional YANG specifications.

#  IANA considerations

This document will include requests to IANA:

To be defined yet.  But keeping up with changes to ietf-tcg-algs.yang will be necessary.

#  Security Considerations

The YANG module specified in this document defines a schema for data that is designed to be accessed via network management protocols such as NETCONF {{RFC6241}} or RESTCONF {{RFC8040}}.  The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{RFC6242}}.  The lowest RESTCONF layer is HTTPS, and the mandatory-to-implement secure transport is TLS {{RFC5246}}.

There are a number of data nodes defined in this YANG module that are writable/creatable/deletable (i.e., config true, which is the default).  These data nodes may be considered sensitive or vulnerable in some network environments.  Write operations (e.g., edit-config) to these data nodes without proper protection can have a negative effect on network operations.  These are the subtrees and data nodes and their sensitivity/vulnerability:

Container: \</rats-support-structures/attester-supported-algos\>

* \<tpm12-asymmetric-signing\>, \<tpm12-hash\>, \<tpm20-asymmetric-signing\>, and \<tpm20-hash\> all could be populated with algorithms which are not supported by the underlying physical TPM installed by the equipment vendor.

Container: \</rats-support-structures/tpms\>

* \<tpm-name\> - Although shown as 'rw', it is system generated

* \<tpm20-pcr-bank\> - It is possible to configure PCRs for extraction which are not being extended by system software.  This could unnecessarily use TPM resources.

* \<certificates\> - It is possible to provision a certificate which does not correspond to a Attestation Identity Key (AIK) within the TPM.

RPC: \<tpm12-challenge-response-attestation\> - Need to verify that the certificate is for an active AIK.

RPC: \<tpm20-challenge-response-attestation\> - Need to verify that the certificate is for an active AIK.

RPC: \<log-retrieval\> - Pulling lots of logs can chew up system resources.

#  Acknowledgements

Not yet.

#  Change Log

Changes from version 03 to version 04:

* TPM1.2 Quote1 eliminated
* YANG model simplifications so redundant info isn't exposed

Changes from version 02 to version 03:

* moved to tcg-algs
* cleaned up model to eliminate sources of errors
* removed key establishment RPC
* added lots of XPATH which must all be scrubbed still
* Descriptive text added on model contents.

Changes from version 01 to version 02:

* Extracted Crypto-types into a separate YANG file
* Mades the algorithms explicit, not strings
* Hash Algo as key the selected TPM2 PCRs
* PCR numbers are their own type
* Eliminated nested keys for node-id plus tpm-name
* Eliminated TPM-Name of "ALL"
* Added TPM-Path

Changes from version 00 to version 01:

* Addressed author's comments
* Extended complementary details about attestation-certificates
* Relabeled chunk-size to log-entry-quantity
* Relabeled location with compute-node or tpm-name where appropriate
* Added a valid entity-mib physical-index to compute-node and tpm-name to map it back to hardware inventory
* Relabeled name  to tpm_name
* Removed event-string in last-entry

--- back
