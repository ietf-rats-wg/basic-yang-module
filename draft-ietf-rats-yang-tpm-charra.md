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
  I-D.ietf-rats-reference-interaction-models: rats-interaction-models
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
    
  TPM2.0-Key:
    target: https://trustedcomputinggroup.org/wp-content/uploads/TCG_IWG_DevID_v1r2_02dec2020.pdf
    title: "TPM 2.0 Keys for Device Identity and Attestation, Rev10"
    author:
      -
        ins: TCG
        name: Trusted Computing Group
    date: 2021-04-14  
  
  TCG-Algos:
    target: hhttp://trustedcomputinggroup.org/resource/tcg-algorithm-registry/
    title: "TCG_Algorithm_Registry_r1p32_pub"

informative:
  RFC6241:
  RFC8040:
  RFC6242:
  RFC8446:


--- abstract

This document defines a YANG RPC and a small number of configuration node required to retrieve attestation evidence about integrity measurements from a device following the operational context defined in TPM-based Network Device Remote Integrity Verification. Complementary measurement logs are also provided by the YANG RPC originating from one or more roots of trust of measurement. The module defined requires at least one TPM 1.2 or TPM 2.0 and corresponding Trusted Software Stack included in the device components of the composite device the YANG server is running on.

--- middle

# Introduction

This document is based on the general terminology defined in the {{-rats-architecture}} and uses the operational context defined in {{-RIV}} as well as the interaction model and information elements defined in {{-rats-interaction-models}}. The currently supported hardware security modules are the Trusted Platform Module (TPM) {{TPM1.2}} and {{TPM2.0}} specified by the Trusted Computing Group (TCG). One or more TPMs embedded in the components of a Composite Device are required in order to use the YANG module defined in this document. A TPM is used as a root of trust for reporting in order to retrieve attestation Evidence from a composite device (quote primitive operation). Additionally, it is used as a root of trust for storage (RTS) in order to retain shielded secrets and store system measurements using a folding hash function (extend primitive operation).

Specific terms imported from {{-rats-architecture}} and used in this document include: Attester, Composite Device, Evidence.

Specific terms imported from {{TPM2.0-Key}} and used in this document include: Endorsement Key (EK), Initial Attestation key (IAK), Local Attestation Key (LAK). 

## Requirements notation

{::boilerplate bcp14}

# The YANG Module for Basic Remote Attestation Procedures

One or more TPMs MUST be embedded in the composite device that is providing attestation evidence via the YANG module defined in this document. The ietf-basic-remote-attestation YANG module enables a composite device to take on the role of Claimant and Attester in accordance with the Remote Attestation Procedures (RATS) architecture {{-rats-architecture}} and the corresponding challenge-response interaction model defined in the {{-rats-interaction-models}} document. A fresh nonce with an appropriate amount of entropy MUST be supplied by the YANG client in order to enable a proof-of-freshness with respect to the attestation evidence provided by the attester running the YANG datastore. The functions of this YANG module are restricted to 0-1 TPMs per hardware component.

## YANG Modules


### ietf-tpm-remote-attestation
This YANG module imports modules from {{-ietf-yang-types}}, {{-ietf-hardware}}, {{-ietf-keystore}}, ietf-tcg-algs.yang {{ref-ietf-tcg-algs}}.

#### Features

This module supports the following features:

'TPMs' - Indicates that multiple TPMs on the device can support remote attestation,  This feature is applicable in cases where multiple line cards, each with its own TPM.

'bios'  - Indicates the device supports the retrieval of bios event logs.

'ima' - Indicates the device supports the retrieval of Integrity Measurement Architecture event logs.

'netequip_boot' - Indicates the device supports the retrieval of netequip boot event logs.

#### Identities

This module supports the following types of attestation event logs: 'ima', 'bios', and 'netequip_boot'.

#### RPCs

##### 'tpm20-challenge-response-attestation'
This RPC allows a Verifier to request a quote of PCRs from a TPM2.0 compliant cryptoprocessor.  Where the feature 'TPMs' is active, and one or more 'certificate-name' is not provided, all TPM2.0 compliant cryptoprocessors will respond.   A YANG tree diagram of this RPC is as follows:

~~~ TREE
{::include tpm20-challenge-response-attestation.tree}
~~~

An example of an RPC challenge requesting PCRs 0-7 from a SHA256 bank could look like the following:

~~~
<rpc message-id="101" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <tpm20-challenge-response-attestation>
      xmlns="urn:ietf:params:xml:ns:yang:ietf-tpm-remote-attestation">
    <nonce>110101010110011011111001010010100</nonce>
    <tpm20-pcr-selection>
      <tpm20-hash-algo
          xmlns:taa="urn:ietf:params:xml:ns:yang:ietf-tcg-algs">
        taa:TPM_ALG_SHA256
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
  </tpm20-challenge-response-attestation>
</rpc>
~~~

and a successful response might be formated as follows:

~~~
<rpc-reply message-id="101"
  xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <tpm12-attestation-response
    xmlns="urn:ietf:params:xml:ns:yang:ietf-tpm-remote-attestation">
    <certificate-name
        xmlns:ks=urn:ietf:params:xml:ns:yang:ietf-keystore>
       ks:(instance of Certificate name in the Keystore)
    </certificate-name>
    <TPMS_QUOTE_INFO>
       (raw information from the TPM Quote, this includes a digest
       across the requested PCRs, the nonce, TPM2 time counters.)
    </TPMS_QUOTE_INFO>
    <quote-signature>
        (signature across TPMS_QUOTE_INFO)
    </quote-signature>
  </tpm12-attestation-response>
</rpc-reply>
~~~

#### 'tpm12-challenge-response-attestation'

This RPC allows a Verifier to request a quote of PCRs from a TPM1.2 compliant cryptoprocessor.  Where the feature 'TPMs' is active, and one or more 'certificate-name' is not provided, all TPM1.2 compliant cryptoprocessors will respond.  A YANG tree diagram of this RPC is as follows:

~~~ TREE
{::include tpm12-challenge-response-attestation.tree}
~~~

#### 'log-retrieval'

This RPC allows a Verifier to acquire the evidence which was extended into specific PCRs.   A YANG tree diagram of this RPC is as follows:

~~~ TREE
{::include log-retrieval.tree}
~~~

#### Data Nodes

This section provides a high level description of the data nodes containing the configuration and operational objects with the YANG model. For more details, please see the YANG model itself in {{ref-ietf-tpm-remote-attestation}}.

container 'rats-support-structures' - This houses the set of information relating to a device's TPM(s).

container 'tpms' - Provides configuration and operational details for each supported TPM, including the tpm-firmware-version, PCRs which may be quoted, certificates which are associated with that TPM, and the current operational status.  Of note is the certificates which are associated with that TPM.  As a certificate is associated with a single Attestation key, knowledge of the certificate allows a specific TPM to be identified.

~~~ TREE
{::include tpms.tree}
~~~

container 'attester-supported-algos' - Identifies which TCG algorithms are available for use the Attesting platform.  This allows an operator to limit algorithms available for use by RPCs to just a desired set from the universe of all allowed by TCG.

~~~ TREE
{::include attester-supported-algos.tree}
~~~

container 'compute-nodes' - When there is more than one TPM supported, this container maintains the set of information related to the compute associated with a specific TPM.  This allows each specific TPM to identify on which 'compute-node' it belongs.

~~~ TREE
{::include compute-nodes.tree}
~~~


#### YANG Module
{: #ref-ietf-tpm-remote-attestation}
~~~ YANG
<CODE BEGINS> file "ietf-tpm-remote-attestation@2021-05-11.yang"
{::include ietf-tpm-remote-attestation.yang}
<CODE ENDS>
~~~

### ietf-tcg-algs

Cryptographic algorithm types were initially included within -v14 NETCONF's iana-crypto-types.yang.  Unfortunately all this content including the algorithms needed here failed to make the -v15 used WGLC.   As a result this document has encoded the TCG Algorithm definitions of {{TCG-Algos}}, revision 1.32.  By including this full table as a separate YANG file within this document, it is possible for other YANG models to leverage the contents of this model.

#### Features

There are two types of features supported 'TPM12' and 'TPM20'. Support for either of these features indicates that a cryptoprocessor supporting the corresponding type of TCG API is present on an Attester.  Most commonly, only one type of cryptoprocessor will be available on an Attester.

#### Identities

There are three types of identities in this model.

The first are the cryptographic functions supportable by a TPM algorithm, these include: 'asymmetric', 'symmetric', 'hash', 'signing', 'anonymous_signing', 'encryption_mode', 'method', and 'object_type'.  The definitions of each of these are in Table 2 of {{TCG-Algos}}.

The second are API specifications for tpms: 'tpm12' and 'tpm2'.

The third are specific algorithm types.   Each algorithm type defines what cryptographic functions may be supported, and on which type of API specification.  It is not required that an implementation of a specific TPM will support all algorithm types.  The contents of each specific algorithm mirrors what is in Table 3 of {{TCG-Algos}}.

{: #ref-ietf-tcg-algs}
#### YANG Module
~~~~ YANG
<CODE BEGINS> file "ietf-tcg-algs@2021-05-11.yang"
{::include ietf-tcg-algs@2021-05-11.yang}
<CODE ENDS>
~~~~

Note that not all cryptographic functions are required for use by ietf-tpm-remote-attestation.yang.  However the full definition of Table 3 of {{TCG-Algos}} will allow use by additional YANG specifications.

#  IANA considerations

This document will include requests to IANA:

To be defined yet.  But keeping up with changes to ietf-tcg-algs.yang will be necessary.

#  Security Considerations

The YANG module specified in this document defines a schema for data that is designed to be accessed via network management protocols such as NETCONF {{RFC6241}} or RESTCONF {{RFC8040}}.  The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{RFC6242}}.  The lowest RESTCONF layer is HTTPS, and the mandatory-to-implement secure transport is TLS {{RFC8446}}.

There are a number of data nodes defined in this YANG module that are writable/creatable/deletable (i.e., config true, which is the default).  These data nodes may be considered sensitive or vulnerable in some network environments.  Write operations (e.g., edit-config) to these data nodes without proper protection can have a negative effect on network operations.  These are the subtrees and data nodes and their sensitivity/vulnerability:

Container: '/rats-support-structures/attester-supported-algos'

* 'tpm12-asymmetric-signing', 'tpm12-hash', 'tpm20-asymmetric-signing', and 'tpm20-hash' all could be populated with algorithms which are not supported by the underlying physical TPM installed by the equipment vendor.

Container: '/rats-support-structures/tpms'

* 'name' - Although shown as 'rw', it is system generated.  Therefore it should not be possible for an operator to add or remove a TPM from the configuration.

* 'tpm20-pcr-bank' - It is possible to configure PCRs for extraction which are not being extended by system software.  This could unnecessarily use TPM resources.

* 'certificates' - It is possible to provision a certificate which does not correspond to a Attestation Identity Key (AIK) within the TPM.

RPC: 'tpm12-challenge-response-attestation' - Need to verify that the certificate is provided is able to support Attestation on the targeted TPM.

RPC: 'tpm20-challenge-response-attestation' - Need to verify that the certificate is provided is able to support Attestation on the targeted TPM.

RPC: 'log-retrieval' - Pulling lots of logs can chew up system resources.

#  Acknowledgements

Not yet.

#  Change Log

Changes from version 05 to version 06:

* More YANG Dr comments covered

Changes from version 04 to version 05:

* YANG Dr comments covered

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
