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
  org: Cisco Systems
  abbrev: Cisco
  email: shwethab@cisco.com
- ins: B. Sulzen
  name: Bill Sulzen
  org: Cisco Systems
  abbrev: Cisco
  email: bsulzen@cisco.com
- ins: E. Voit
  name: Eric Voit
  org: Cisco Systems
  abbrev: Cisco
  email: evoit@cisco.com
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
  I-D.ietf-netconf-crypto-types: ietf-crypto-types
  I-D.birkholz-rats-reference-interaction-model: rats-interaction-models

informative:
  I-D.ietf-rats-architecture: rats-architecture


--- abstract

This document defines a YANG RPC and a minimal datastore tree required to retrieve attestation evidence about integrity measurements from a composite device with one or more roots of trust for reporting. Complementary measurement logs are also provided by the YANG RPC originating from one or more roots of trust of measurement. The module defined requires at least one TPM 1.2 or TPM 2.0 and corresponding Trusted Software Stack included in the device components of the composite device the YANG server is running on.

--- middle

# Introduction

This document is based on the terminology defined in the {{-rats-architecture}} and uses the interaction model and information elements defined in the {{-rats-interaction-models}} document. The currently supported hardware security modules (HWM) - sometimes also referred to as an embedded secure element (eSE) - is the Trusted Platform Module (TPM) version 1.2 and 2.0 specified by the Trusted Computing Group (TCG). One ore more TPMs embedded in the components of a composite device - sometimes also referred to as an aggregate device - are required in order to use the YANG module defined in this document. A TPM is used as a root of trust for reporting (RTR) in order to retrieve attestation evidence from a composite device (quote primitive operation). Additionally, it is used as a root of trust for storage (RTS) in order to retain shielded secrets and store system measurements using a folding hash function (extent primitive operation).

## Requirements notation

{::boilerplate bcp14}

# The YANG Module for Basic Remote Attestation Procedures

One or more TPMs MUST be embedded in the composite device that is providing attestation evidence via the YANG module defined in this document. The ietf-basic-remote-attestation YANG module enables a composite device to take on the role of Claimant and Attester in accordance with the Remote Attestation Procedures (RATS) architecture {{-rats-architecture}} and the corresponding challenge-response interaction model defined in the {{-rats-interaction-models}} document. A fresh nonce with an appropriate amount of entropy MUST be supplied by the YANG client in order to enable a proof-of-freshness with respect to the attestation evidence provided by the attester running the YANG datastore. The functions of this YANG module are restricted to 0-1 TPMs per hardware component.

## Tree Diagram

~~~ TREE
{::include ietf-tpm-remote-attestation.tree}
~~~

## YANG Module

This YANG module imports modules from {{-ietf-yang-types}}, {{-ietf-hardware}}, and {{-ietf-crypto-types}}.

~~~ YANG
<CODE BEGINS> file ietf-tpm-remote-attestation@2019-01-07.yang
{::include ietf-tpm-remote-attestation.yang}
<CODE ENDS>
~~~

#  IANA considerations

This document will include requests to IANA:

To be defined yet.

#  Security Considerations

There are always some.

#  Acknowledgements

Not yet.

#  Change Log

Changes from version 00 to version 01:

* Addressed author's comments
* Extended complementary details about attestation-certificates
* Relabeled chunk-size to log-entry-quantity
* Relabeled location with compute-node or tpm-name where appropriate
* Added a valid entity-mib physical-index to compute-node and tpm-name to map it back to hardware inventory
* Relabeled name  to tpm_name
* Removed event-string in last-entry

--- back
