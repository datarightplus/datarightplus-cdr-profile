%%%
title = "DataRight+: Australian CDR Profile"
area = "Internet"
workgroup = "datarightplus"
submissionType = "independent"

[seriesInfo]
name = "Internet-Draft"
value = "draft-authors-datarightplus-cdr-profile-latest"
stream = "independent"
status = "experimental"

date = 2024-04-03T00:00:00Z

[[author]]
initials="S."
surname="Low"
fullname="Stuart Low"
organization="Biza.io"
[author.address]
email = "stuart@biza.io"

%%%

.# Abstract

This is the ecosystem profile for the Australian CDR describing the composite components to form the technical infrastructure operating to form the Australian Consumer Data Right. This specification is intended to result in a [@!CDS] compatible implementation.

.# Notational Conventions

The keywords "**MUST**", "**MUST NOT**", "**REQUIRED**", "**SHALL**", "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**",  "**MAY**", and "**OPTIONAL**" in this document are to be interpreted as described in [@!RFC2119].

{mainmatter}

# Scope

The scope of this document is intended to be the combinatorial outcome of a variety of specifications to achieve a compliant outcome within the Australian Consumer Data Right. Because this document relates to a specific ecosystem deployment it contains static configuration information.

This document **does not** seek to navigate the complexities of [@!CDR-RULES] but rather to establish a technical baseline to consider these in each implementors context.

# Terminology

This specification utilises the various terms outlined within [@!DATARIGHTPLUS-ROSETTA].

Banking Sector
: Relates to the Holders, designated under the [@!CDR-RULES] in the Banking industry

Energy Sector
: Relates to the Designated Holders, designated under the [@!CDR-RULES] in the Energy industry

Designated Holders
: Designated Holders are organisations which belong to a designated sector, according to the [@!CDR-RULES] and meet certain eligibility requirements to be required to deliver CDR services within their sector.

# Providers

Providers are required to deliver authorisation and resource requirements. They are also required to integrate with the Ecosystem Authority in the prescribed way.

## Information Security

Providers **MUST**:

1. comply with the Provider provisions described in [@!DATARIGHTPLUS-INFOSEC-BASELINE-00];
1. comply with the provisions outlined in [@!DATARIGHTPLUS-ADMISSION-CONTROL-00];
1. comply with the provisions outlined in [@!DATARIGHTPLUS-SHARING-ARRANGEMENT-V1-00];
1. support the following `acr` claim and validate the Consumer with the following values:
    1. `urn:cds.au:cdr:2` where the authentication achieved matches the Credential Level `CL1` from [@!TDIF] or;
    2. `urn:cds.au:cdr:3` where the authentication achieved matches the Credential Level `CL2` from [@!TDIF]
1. incorporate One-Time Passwords as part of the requirement to achieve the minimum acceptable value for the `acr` claim and:
   1. **MUST** be delivered using existing and preferred channels
   1. **MUST** be numeric digits and be between 4 and 6 digits in length
   1. **MUST** only be valid for the purposes of establishing authorisations between Provider and Initiators
   1. **MUST** be invalidated after a reasonable period of time
1. **MUST** authenticate a confidential client using `private_key_jwt` as described in section 9 of [@!OIDC-Core] with a client identifier of `cdr-register`
1. **MUST** supply a `scope` value of `admin:metrics.basic:read` for all successful authentications of the `cdr-register` client identifier

_Note:_ The CDR currently mandates, essentially exclusively, the use of One-Time Passwords while restricting the introduction of additional "friction" via other factors. It is understood this is currently being reconsidered.

## Resource Server

Providers operating within the Banking Sector **MUST** comply with the provisions outlined in [@!DATARIGHTPLUS-RESOURCE-SET-COMMON-00] and [@!DATARIGHTPLUS-RESOURCE-SET-BANKING-00].

Providers operating within the Energy Sector **MUST** comply with the provisions outlined in [@!DATARIGHTPLUS-RESOURCE-SET-COMMON-00] and [@!DATARIGHTPLUS-RESOURCE-SET-ENERGY-00].

### Metrics

In addition to the aforementioned requirements Providers **MUST** deliver protected resource(s), in accordance with [@!DATARIGHTPLUS-REDOCLY-ID1], as follows:

| Resource Server Endpoint | Required Scope             | Valid `x-v` |
|--------------------------|----------------------------|-------------|
| `GET /admin/metrics`     | `admin:metrics.basic:read` | `5`         |

### Forced Metadata Refresh

In addition to the aforementioned requirements Providers **MUST** deliver protected resource(s), in accordance with [@!DATARIGHTPLUS-REDOCLY-ID1], as follows:

| Resource Server Endpoint | Required Scope          | Valid `x-v` |
|--------------------------|-------------------------|-------------|
| `GET /admin/metrics`     | `admin:metadata:update` | `1`         |

On requesting this endpoint the Provider **MUST** trigger a refresh of the information obtained from the Ecosystem Directory.

# Initiators

Initiators are required to comply with Ecosystem Authority requirements and integrate with Providers in prescribed ways.

Within the Australian CDR Initiators are commonly referred to as Software Products.

## Information Security

Initiators **MUST**:

1. comply with the Initiator provisions described in [@!DATARIGHTPLUS-INFOSEC-BASELINE-00];
1. comply with the provisions outlined in [@!DATARIGHTPLUS-ADMISSION-CONTROL-00];
1. comply with the provisions outlined in [@!DATARIGHTPLUS-SHARING-ARRANGEMENT-V1-00];

## Resource Server Client

Initiators **MUST** access Provider resource server infrastructure in accordance with:

1. [@!DATARIGHTPLUS-INFOSEC-BASELINE-00] and;
2. [@!DATARIGHTPLUS-RESOURCE-SET-COMMON-00] and;
3. [@!DATARIGHTPLUS-RESOURCE-SET-BANKING-00] and;
4. [@!DATARIGHTPLUS-RESOURCE-SET-ENERGY-00]

# Ecosystem Authority

The Ecosystem Authority **MUST** comply with the requirements outlined within [@!DATARIGHTPLUS-ADMISSION-CONTROL-00].

The Ecosystem Authority for the Australian CDR is the [Australian Competition and Consumer Commission (ACCC)](https://www.accc.gov.au/).

## Accreditation

The Ecosystem Authority performs external validation of participant capabilities, particularly of the Initiator Entity.

This occurs by way of a combination of Ecosystem Authority verification, external technology audit standards (notably ASAE3150) and legal assertions by a Initiator Entity carrying higher accreditation as to the suitability of new subordinate Initiator Entities.

As a result of this validation Initiator Entities are granted an accreditation status which in turn influences the authorisation scopes that are made available to thee relevant Initiator. Further detail on this process in the context of the CDR can be found within the [@!CDR-RULES] and within guidelines published on the [CDR website](https://cdr.gov.au).

The subject of accreditation is not intended to be covered by this specification. As a consequence this document focuses primarily on the relationship between Provider and Initiator with the Ecosystem Authority providing third party assurance with respect to technical admission control.

# Electricity Authority

The Electricity Authority **MUST** comply with the relevant provisions outlined within [@!DATARIGHTPLUS-RESOURCE-SET-ENERGY-00].

The Electricity Authority for the Australian CDR is the [Australian Energy Market Operator (AEMO)](https://aemo.com.au/en).

# Electricity Plan Website

The Electricity Plan Website **MUST** comply with the relevant provisions outlined within [@!DATARIGHTPLUS-RESOURCE-SET-ENERGY-00].

The Electricity Plan Website for the Australian CDR is [Energy Made Easy](https://energymadeeasy.gov.au/) operated by the [Australian Energy Regulator](https://www.aer.gov.au/).

# Current Ecosystem Configuration

The following outlines the currently understood endpoint configuration for the Australian CDR ecosystem:

# Implementation Considerations

Where One-Time Password OTP are in use the generation method **SHOULD** incorporate controls, such as retry limits, to minimise the risk of enumeration attacks.

# Acknowledgement

The following people contributed to this document:

- Stuart Low (Biza.io) - Editor

We acknowledge the contribution to the [@!CDS] of the following individuals:
- James Bligh (Data Standards Body) - Lead Architect for the Consumer Data Right
- Mark Verstege (Data Standards Body) - Lead Architect, Banking & Information Security for the Consumer Data Right
- Ivan Hosgood (formerly Data Standards Body & ACCC) - Solutions Architect

{backmatter}


<reference anchor="DATARIGHTPLUS-ROSETTA" target="https://datarightplus.github.io/datarightplus-rosetta/draft-authors-datarightplus-rosetta.html"> <front><title>DataRight+ Rosetta Stone</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-REDOCLY-ID1" target="https://datarightplus.github.io/datarightplus-redocly/?v=ID1"> <front><title>DataRight+: Redocly (ID1)</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author><author initials="B." surname="Kolera" fullname="Ben Kolera"><organization>Biza.io</organization></author>
<author initials="W." surname="Cai" fullname="Wei Cai"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="CDS" target="https://consumerdatastandardsaustralia.github.io/standards"><front><title>Consumer Data Standards (CDS)</title><author><organization>Data Standards Body (Treasury)</organization></author></front> </reference>

<reference anchor="CDR-RULES" target="https://www.legislation.gov.au/F2020L00094/2023-07-22/text"> <front><title>Competition and Consumer (Consumer Data Right) Rules 2020</title><author><organization>Department of the Treasury</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-SHARING-ARRANGEMENT-V1-00" target="https://datarightplus.github.io/datarightplus-sharing-arrangement-v1/draft-authors-datarightplus-sharing-arrangement-v1-00/draft-authors-datarightplus-sharing-arrangement-v1.html"> <front><title>DataRight+: Sharing Arrangement V1</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author><author initials="B." surname="Kolera" fullname="Ben Kolera"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-RESOURCE-SET-BANKING-00" target="https://datarightplus.github.io/datarightplus-resource-set-banking/draft-authors-datarightplus-resource-set-banking-00/draft-authors-datarightplus-resource-set-banking.html"> <front><title>DataRight+: Banking Resource Set</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-RESOURCE-SET-ENERGY-00" target="https://datarightplus.github.io/datarightplus-resource-set-energy/draft-authors-datarightplus-resource-set-energy-00/draft-authors-datarightplus-resource-set-energy.html"> <front><title>DataRight+: Energy Resource Set</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-RESOURCE-SET-COMMON-00" target="https://datarightplus.github.io/datarightplus-resource-set-common/draft-authors-datarightplus-resource-set-common-00/draft-authors-datarightplus-resource-set-common.html"> <front><title>DataRight+: Common Resource Set</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-ADMISSION-CONTROL-00" target="https://datarightplus.github.io/datarightplus-admission-control-baseline/draft-authors-datarightplus-admission-control-00/draft-authors-datarightplus-admission-control.html"> <front><title>DataRight+: Admission Control Baseline</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author><author initials="B." surname="Kolera" fullname="Ben Kolera"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-INFOSEC-BASELINE-00" target="https://datarightplus.github.io/datarightplus-infosec-baseline/draft-authors-datarightplus-infosec-baseline-00/draft-authors-datarightplus-infosec-baseline.html"> <front><title>DataRight+ Security Profile: Baseline</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author><author initials="B." surname="Kolera" fullname="Ben Kolera"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="OIDC-Core" target="http://openid.net/specs/openid-connect-core-1_0.html"> <front> <title>OpenID Connect Core 1.0 incorporating errata set 1</title> <author initials="N." surname="Sakimura" fullname="Nat Sakimura"></author></front></reference>

<reference anchor="TDIF" target="https://www.digitalidentity.gov.au"><front><title>Trusted Digital Identity Framework (
TDIF)</title><author><organization>Commonwealth of
Australia (Digital Transformation Agency)</organization></author></front> </reference>






