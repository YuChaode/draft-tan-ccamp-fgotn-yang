---
title: "YANG Data Models for fine grain Optical Transport Network"
abbrev: "Fine grain OTN YANG"
category: std

docname: draft-tan-ccamp-fgotn-yang-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Routing"
workgroup: "Common Control and Measurement Plane"
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: "Common Control and Measurement Plane"
  type: "Working Group"
  mail: "ccamp@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ccamp/"
  github: "YuChaode/draft-tan-ccamp-fgotn-yang"
  latest: "https://YuChaode.github.io/draft-tan-ccamp-fgotn-yang/draft-tan-ccamp-fgotn-yang.html"

author:
 -
  ins: Y. Tan
  name: Yanxia Tan
  organization: China Unicom
  email: tanyx11@chinaunicom.cn
  city: Beijing
  country: China
 -
  ins: Y. Zheng
  name: Yanlei Zheng
  organization: China Unicom
  email: zhengyanlei@chinaunicom.cn
  city: Beijing
  country: China
 -
  ins: I. Busi
  name: Italo Busi
  organization: Huawei Technologies
  email: italo.busi@huawei.com
 -
  ins: C. Yu
  name: Chaode Yu
  organization: Huawei Technologies
  email: yuchaode@huawei.com
  country: China
 -
  ins: X. Zhao
  name: Xing Zhao
  organization: CAICT
  email: zhaoxing@caict.ac.cn
  country: China

contributor:
  -
    name: Chen Li
    org: Fiberhome Telecommunication Technologies Co.,LTD
    email: lich@fiberhome.com

normative:
  ITU-T_G.709:
    title: Interfaces for the optical transport network
    author:
      org: ITU-T Recommendation G.709
    date: March 2024
    seriesinfo: ITU-T Recommendation G.709, Amendment 3
    target: https://www.itu.int/rec/T-REC-G.709/
  ITU-T_G.709.20:
    title: Overview of fine grain OTN
    author:
      org: ITU-T Recommendation G.709.20
    date: April 2024
    seriesinfo: ITU-T Recommendation G.709.20
    target: https://www.itu.int/rec/T-REC-G.709.20/
  IANA_YANG:
    title: YANG Parameters
    author:
      org: IANA
    target: https://www.iana.org/assignments/yang-parameters

informative:

--- abstract
This document defines YANG data models to describe the topology and tunnel information of a fine grain Optical Transport Network. The YANG data models defined in this document are designed to meet the requirements for efficient transmission of sub-1Gbit/s client signals in transport network.

--- middle

# Introduction {#sec-intro}

Optical Transport Networks (OTN) is a mainstream layer 1 technology for the transport network. Over the years, it has continued to evolve, to improve its transport functions for the emerging requirements. The topology and tunnel information in the OTN has already been defined by generic traffic-engineering models and technology-specific models, including {{?I-D.ietf-ccamp-otn-topo-yang}} and {{?I-D.ietf-ccamp-otn-tunnel-model}}.

In the latest version of OTN, ITU-T G.709/Y.1331 Edition 6.5 {{ITU-T_G.709}}, the fine grain OTN (fgOTN) is introduced for the efficient transmission of low rate client signals (e.g., sub-1G).

This document presents the control interface requirements of fgOTN, and defines two YANG data models for fgOTN topology and fgOTN tunnel. The topology model can capture topological and resource-related information pertaining to fgOTN. The fgOTN tunnel YANG data model defined in this document is used for the provisioning and management of fgOTN Traffic Engineering (TE) tunnels and Label Switched Paths (LSPs).

Furthermore, this document also imports the generic Layer 1 types defined in {{?I-D.ietf-ccamp-layer1-types}}.

The YANG data models defined in this document conform to the Network Management Datastore Architecture (NMDA) defined in {{!RFC8342}}.

## Terminology and Notations

Some of the key terms used in this document are listed as follow.

  *  fgTS: fine grain Tributary Slot.

  *  fgODUflex: fine grain Optical channel Data Unit flex.

The following terms are defined in {{!RFC7950}} and are not redefined here:

  *  client

  *  server

  *  augment

  *  data model

  *  data node

The following terms are defined in {{!RFC6241}} and are not redefined here:

  *  configuration data

  *  state data

The terminology for describing YANG data models is found in {{!RFC7950}}.

## Requirements Notation

{::boilerplate bcp14}

## Tree Diagram

A simplified graphical representation of the data model is used in {{fgotn-tree}} of this document. The meaning of the symbols in this diagram is defined in {{!RFC8340}}.

## Requirements Language

{::boilerplate bcp14-tagged}

## Prefixes in Model Names

In this documents, names of data nodes and other data model objects are prefixed using the standard prefix associated with the corresponding YANG imported modules, as shown in the following table.

| Prefix     | Yang Module                     | Reference     |
| ---------- | ------------------------------- | ------------- |
| l1-types   | ietf-layer1-types               | \[RFC YYYY]   |
| otnt       | ietf-otn-topology               | \[RFC ZZZZ]   |
| te         | ietf-te                         | \[RFC KKKK]   |
| otn-tnl    | ietf-otn-tunnel                 | \[RFC JJJJ]   |
| fgotnt     | ietf-fgotn-topology             | RFC XXXX      |
| fgotn-tnl  | ietf-fgotn-tunnel               | RFC XXXX      |
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note:
Please replace XXXX with the number assigned to the RFC once this draft becomes an RFC.
Please replace YYYY with the RFC numbers assigned to {{!I-D.ietf-ccamp-layer1-types}}.
Please replace ZZZZ with the RFC numbers assigned to {{!I-D.ietf-ccamp-otn-topo-yang}}.
Please replace KKKK with the RFC numbers assigned to {{!I-D.ietf-teas-yang-te}}.
Please replace JJJJ with the RFC numbers assigned to {{!I-D.ietf-ccamp-otn-tunnel-model}}.
Please remove this note.

## Model Tree Diagrams

The tree diagrams extracted from the module(s) defined in this document are given in subsequent sections as per the syntax defined in {{!RFC8340}}.

# Fine grain Optical Transport Network Scenarios Overview

OTN network will cover a larger scope of networks, it may include the backbone network, metro core, metro aggregation, metro access, and even the OTN CPE in the customers' networks {{ITU-T_G.709.20}}. In general, the metro OTN networks support both fgODUflex and ODUk switching.  At the boundary nodes (e.g., metro-core nodes) of the metro OTN networks, the fgODUflexes to other metro OTN networks are multiplexed into ODUk of backbone networks. Therefore, the backbone OTN network could only support ODUk switching.

The typical scenarios for fgOTN is to provide low bit rate private line or private network services for customers. The interface function requirements of fgOTN mainly include topology resource reporting and service provisioning. Three scenarios that require special consideration are listed based on the characteristics of fgOTN.

## Retrieve Server Tunnels Scenario of fgOTN

{{fig-multiplexing}} below shows an example of scenario to retrieve server tunnels for multi-domain fgOTN service.
In this example, some small bandwidth fgOTN service are aggregated by the access ring (10G), and then aggregated into a bigger bandwidth in metro ring (100G).
The allocation of TS to support fgOTN switching maybe different in access ring and metro ring.
All link bandwidth information that supports fgOTN should be reported to MDSC by the PNC controller. E.g. there could be three ODU0 allocated in the access ring while there could be two ODU2 are allocated in the metro ring to support fgOTN switching. In this example, the server layer ODUk tunnel for fgOTN tunnel from node A to node E is ODU0, and the server layer tunnel from node E to node G is ODU2. The server layer tunnel for fgOTN tunnel will include one ODU0 tunnel and one ODU2 tunnel.

~~~~ ascii-art

      +-----+
      |  A  | \                                 |
      +-----+  \            Domain 1            |      Domain 2
         |      \                               |
         |  10G  \                              |
         |        \                             |
      +-----+       +-----+         +-----+     |     +-----+
      |  B  | \     |  E  |---------|  G  |-----------|  I  |---------
      +-----+  \  / +-----+         +-----+           +-----+
                \/    |      100G      |                 |    100G
                /\    |                |                 |
      +-----+  /  \ +-----+         +-----+           +-----+
      |  C  | /     |  F  |---------|  H  |-----------|  J  |---------
      +-----+       +-----+         +-----+           +-----+
         |         /
         |  10G   /
         |       /
      +-----+   /
      |  D  |  /
      +-----+

~~~~
{: #fig-multiplexing scenario title="The Scenario to Retrieve Server Tunnels"}

## Multi-layer Path Splicing Scenario of fgOTN

Some operators that would like to provide the paths when there could be different switching capabilities of nodes in their LSP, so that the MDSC coordinator can clearly display multi-layer paths and the relationship between primary-path and secondary-path. In the current network, not all nodes in the operator network support fgOTN, as shown in figure 2, node f1, f2, f3 and f4 support fgOTN, node N-f5 and node N-f6 do not support fgOTN. To present the end-to-end multi-layer primary-path and secondary-path of the services on the client side, it is necessary to complete the end-to-end path splicing based on the the ODU tunnel information associated with the fgotn tunnel.

In {{fig-service}}, assuming that the server layer ODUk tunnel for the fgOTN primary tunnel from node f1 to node f2 is ODU0, the server layer tunnel from node f2 to node f3 is ODU2, and the server layer tunnel from node f3 to node f4 is ODU1. Assuming the server layer ODUk tunnel for the fgOTN secondary tunnel from node f1 to node f2 is ODU2. We need to setup four server layer ODUk tunnels before setting up an fgODUflex tunnel with a primary path and a secondary path to provide protection. To support multi-layer path splicing, we should make some extension on the dependency tunnel structure or on the path element, such as extending the working roles and index of the tunnels.

~~~~ ascii-art
                   +-----+            +-----+
               ----|  f2 |------------|  f3 |----
              /    +-----+            +-----+    \
             / ----------primary-path------------ \
            / /                                  \ \
         +-----+                                +-----+
         |  f1 |                                |  f4 |
         +-----+                                +-----+
            \ \                                  / /
             \ ---------secondary-path----------- /
              \    +------+          +------+    /
               ----| N-f5 |----------| N-f6 |----
                   +------+          +------+
~~~~
{: #fig-service multi-layer path splicing scenario title="Multi-layer Path Splicing Scenario of fgOTN"}

## Hitless Bandwidth Adjustment Scenario of fgOTN

{{ITU-T_G.709}} defines the data plane procedure to support fgODUflex hitless resizing. The support of management of hitless resizing of fgODUflex needs to be carefully considered.

The range of fgOTN service's Bandwidth on Demand (BoD) cannot exceed its server layer's bandwidth.

The client needs to know how many bandwidth of a link is allocated for fgOTN. When performs hitless resizing, the client sends the fgODUflex identifier and the target bandwidth to the source node controller. After receiving the network management configuration information, the source node triggers the bandwidth adjustment. During the hitless bandwidth adjustment process, it is necessary to reserve or mark the corresponding bandwidth resources first, and then trigger the the bandwidth adjustment actions.

Another point to note is that when performing bidirectional hitless resizing for fgODUflex service, the adjustment should be initiated by the client side to a single network management system. Specifically, the adjustment is first performed in the Node 1 to Node 6 direction, and then the reverse direction (Node 6 to Node 1) is automatically triggered for adjustment.

Both single domain and multi-domain hitless resizing should be supported. For multi-domain hitless resizing scenario, both the source controller and the destination controller should report the bandwidth adjustment status to the MDSC coordinator upon completion.

~~~~ ascii-art

                                        +----------+
                  ----------------------|   MDSC   |---------------------
                 /                      |          |                     \
                /                       +----------+                      \
               /                             |                             \
              /                              |                              \
        +------------+                 +------------+               +------------+
        | Controller |                 | Controller |               | Controller |
        |     1      |                 |     2      |               |     3      |
        +------------+                 +------------+               +------------+

                                    End-to-end fgOTN service
   <--------------------------------------------------------------------------------->
   +------+       +------+       +------+       +------+       +------+       +------+
   | node |-------| node |-------| node |-------| node |-------| node |-------| node |
   |  1   |-------|  2   |-------|  3   |-------|  4   |-------|  5   |-------|  6   |
   +------+       +------+   |   +------+       +------+   |   +------+       +------+
    source                   |                             |                 destination
          Domain 1           |          Domain 2           |           Domain 3
                             |                             |
~~~~
{: #fig-hitless resizing scenario title="Hitless Resizing Scenario of fgOTN"}

# YANG Data Model for fine grain Optical Transport Network Overview

In order to provide fgOTN capabilities, this document defines two extension YANG data models augmenting to OTN topology and OTN tunnel YANG model. The attributes related to fgOTN are augments from OTN topology data model, and fgOTN topology is not treated as a separate hierarchy. The fgOTN tunnel is defined as a separate tunnel hierarchy, and the fgOTN tunnels need to be pre-set and created before the service provisioning process.

# YANG Data Model for fgOTN Topology

## Fine Grain OTN Topology Data Model Overview

This document aims to describe the data model for fine grain OTN topology. The YANG module presented in this document augments from OTN topology data model, i.e., the ietf-otn-topology, as specified in {{?I-D.ietf-ccamp-otn-topo-yang}}. In section 6 of {{?I-D.ietf-ccamp-otn-topo-yang}}, the guideline for augmenting OTN topology model was provided, and in this draft, we augment the OTN topology model to describe the topology characteristics of fgOTN.

Common types, identities and groupings defined in {{?I-D.ietf-ccamp-layer1-types}} is reused in this document.

{{?RFC8345}} defines an abstract (generic, or base) YANG data model for network/service topologies and inventories, and provides the fundamental model for {{?RFC8795}}. OTN topology module in {{?I-D.ietf-ccamp-otn-topo-yang}} augments from the TE topology YANG model defined in {{?RFC8795}}. {{fig-fgotn-topology-relationship}} shows the augmentation relationship.

~~~~ ascii-art
    +--------------+      +-----------------------+
    | ietf-network |      | ietf-network-topology |
    +--------------+      +-----------------------+
                ^             ^
                |_____   _____|
                      | |
                      | | Augments
             +-------------------+
             | ietf-te-topology  |
             +-------------------+
                       ^
                       | Augments
                       |
             +-------------------+
             | ietf-otn-topology |
             +-------------------+
                       ^
                       | Augments
                       |
            +----------+----------+
            | ietf-fgotn-topology |
            +---------------------+
~~~~
{: #fig-fgotn-topology-relationship title="Relationship between fgOTN topology and OTN topology model"}

The entities, TE attributes and OTN attributes, such as nodes, termination points and links, are still applicable for describing an fgOTN topology and the model presented in this document only specifies technology-specific attributes/information. The fgOTN-specific attributes including the fgTS, can be used to represent the bandwidth and label information. At the same time, it is necessary to extend the encoding and switching-capability enumeration values in {{?I-D.ietf-teas-rfc8776-update}} to identify that the current Tunnel Termination Point (TTP) is a termination point of an fgOTN tunnel.

## Bandwidth Augmentation

Based on the OTN topology model, we augment the bandwidth information of fgOTN, including the max-link-bandwidth and unreserved-bandwidth. The augmented parameter fgotn-bandwidth is used to indicate how much of the bandwidth has been allocated for the usage of fgOTN. For example, if 2 ODU0s are allocated to support fgOTN switching switching, the fgotn-bandwidth is 2500, and the unit is Mbps.

~~~~ ascii-art
augment /nw:networks/nw:network/nt:link/tet:te/tet:te-link-attributes
          /tet:max-link-bandwidth/tet:te-bandwidth/otnt:otn-bandwidth
          /otnt:odulist:
   +--rw fgotn-bandwidth?   uint16
~~~~

The augmented fgotnlist structure is used to describe the unreserved TE bandwidth of fgOTN in the server ODUk. The odu-ts-number is used to indicate the index of server ODUk channel.

~~~~ ascii-art
augment /nw:networks/nw:network/nt:link/tet:te/tet:te-link-attributes
          /tet:unreserved-bandwidth/tet:te-bandwidth
          /otnt:otn-bandwidth:
   +--rw fgotnlist* [odu-type odu-ts-number]
      +--rw odu-type           identityref
      +--rw odu-ts-number?     uint16
      +--rw fgotn-bandwidth?   uint16
~~~~

## Label Augmentation
The model augments the label-restriction list with fgOTN technology-specific label information using the otn-label-range-info grouping defined in {{?I-D.ietf-ccamp-layer1-types}}.

~~~~ ascii-art
augment /nw:networks/tet:te/tet:templates/tet:link-template
        /tet:te-link-attributes/tet:label-restrictions
        /tet:label-restriction/otnt:otn-label-range:
   +--rw fgts-range* [odu-type odu-ts-number]
      +--rw odu-type           identityref
      +--rw odu-ts-number?     uint16
      +--rw fgts-reserved?     string
      +--rw fgts-unreserved?   string
~~~~

The fgts-range list is used to describe the availability of fgOTN timeslot in the server ODUk, including the fgts-reserved and fgts-unreserved. The odu-ts-number is used to indicate the index of server ODUk channel.

# YANG Data Model for fgOTN Tunnel

## Fine Grain OTN Tunnel Data Model Overview

This document aims to describe the data model for fgOTN tunnel. The fgOTN tunnel model augments to OTN tunnel {{?I-D.ietf-ccamp-otn-tunnel-model}} with fgOTN-specific parameters, including the bandwidth information and label information. {{fig-fgotn-tunnel-relationship}} shows the augmentation relationship.

~~~~ ascii-art
                +------------------+
                |      ietf-te     |
                +------------------+
                          ^
                          | Augments
                          |
                +-----------------+
                | ietf-otn-tunnel |
                +-----------------+
                          ^
                          | Augments
                          |
               +----------+--------+
               | ietf-fgotn-tunnel |
               +-------------------+
~~~~
{: #fig-fgotn-tunnel-relationship title="Relationship between fgOTN and OTN tunnel model"}

It's also worth noting that the fgOTN tunnel provisioning is usually based on the fgOTN topology. Therefore the fgOTN tunnel model is usually used together with fgOTN topology model specified in this document. The OTN tunnel model also imports a few type modules, including ietf-layer1-types and ietf-te-types.

A new identity based on odu-type should be defined for fgODUflex in an updated version of {{?I-D.ietf-ccamp-layer1-types}} to indicate the bandwidth of fgotn tunnel.

## Bandwidth Augmentation

The model augment TE bandwidth information of fgOTN tunnel.

~~~~ ascii-art
augment /te:te/te:tunnels/te:tunnel/te:te-bandwidth/te:technology
        /otn-tnl:otn:
   +--rw fgoduflex-bandwidth?   string
~~~~

The string value fgoduflex-bandwidth is used to indicate the bandwidth of this fgOTN tunnel.

## Label Augmentation

The module augments TE label-hop for the explicit route objects included or excluded by the path computation of the primary-paths and secondary-paths using the fgts-numbers. The fgts-numbers is used to specify fgTS information on inter-domain ports of the routing path. When specifying the fgotn time slot in the routing constraint information, the ODU time slot must also be specified. We also augment the TE label-hop for the record route of the LSP using the fgts-numbers.

{:#fgotn-tree}

# YANG Tree for fgOTN topology

{{fig-fgotn-topo-tree}} below shows the tree diagram of the YANG data model defined in module "ietf-fgotn-topology" ({{fgotn-topology-yang}}).

~~~~ ascii-art
{::include ./yang/ietf-fgotn-topology.tree}
~~~~
{: #fig-fgotn-topo-tree title=fgOTN topology YANG tree diagram"
artwork-name="ietf-fgotn-topology.tree"}

# YANG Data Model for fgOTN topology

~~~~ yang
{::include ./yang/ietf-fgotn-topology.yang}
~~~~
{: #fgotn-topology-yang title="fgOTN topology YANG module"
sourcecode-markers="true" sourcecode-name="ietf-fgotn-topology@2025-06-18.yang"}


# YANG Tree for fgOTN tunnel

{{fig-fgotn-tunnel-tree}} below shows the tree diagram of the YANG data model defined in module "ietf-fgotn-tunnel" ({{fgotn-tunnel-yang}}).

~~~~ ascii-art
{::include ./yang/ietf-fgotn-tunnel.tree}
~~~~
{: #fig-fgotn-tunnel-tree title=fgOTN tunnel YANG tree diagram"
artwork-name="ietf-fgotn-tunnel.tree"}

# YANG Data Model for fgOTN tunnel

~~~~ yang
{::include ./yang/ietf-fgotn-tunnel.yang}
~~~~
{: #fgotn-tunnel-yang title="fgOTN tunnel YANG module"
sourcecode-markers="true" sourcecode-name="ietf-fgotn-tunnel@2025-06-18.yang"}

# Manageability Considerations

  \<Add any manageability considerations>

# Security Considerations
  \<Add any security considerations>

# IANA Considerations

  \<Add any IANA considerations>

--- back

# Acknowledgments

# Appendix A. Multi-domain fgOTN Hitless Resizing Process

The process of cross domain hitless resizing include six steps. Both the source and destination controllers should report the hitless bandwidth adjustment status to the MDSC coordinator.

Step 1: The MDSC coordinator sends an resizing command to the source node (Node1) via Controller 1. The command includes the target bandwidth and the fgODUflex service identifier.

Step 2: Controller 1 reports the bandwidth adjustment status ietf-te-types:lsp-path-modifying to the MDSC via notification.

Step 3: After the resizing in the direction from Node1 to Node6 was completed, Controller 1 reported ietf-te-types:lsp-path-modified to MDSC. This automatically triggered a reverse-direction resizing (Node6 to Node1).

Step 4: Once the reverse resizing begins, Controller 3 reports the bandwidth adjustment status to the MDSC via notification.

Step 5: After the reverse direction  (Node 6 to Node 1) resizing is completed, Controller 3 reports ietf-te-types:lsp-path-modified to MDSC.

Step 6: All domain controllers, including the intermediate domain Controller 2, report notifications of topology and tunnel resource changes to MDSC coordinator.

