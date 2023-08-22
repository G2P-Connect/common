# Registries

## Overview

Success of any G2P program delivery depends on access to beneficiary information across various foundational and functional registries.&#x20;

{% code overflow="wrap" %}
```
An electronic registry is a structured & live identification system that gathers, saves, and maintains uniformed updated data or information on an entity, such as a patient, person, employee, student, or facility, and is constantly updated to serve as the entity's "Single Source of Truth" which is also verifiable. 
[ref: Sunbird-RC](https://docs.sunbirdrc.dev/learn/electronic-registries)
```
{% endcode %}

The scope of G2P Connect Registry [interfaces](./#interface-list) is to enable [federated](./#federated-data-access) minimal read-only data access between platforms using [consented](./#consented-data-sharing), [interoperable ](./#interoperability)specifications.&#x20;

## Federated Data Access

G2P Connect [blueprint](../../../g2p-connect/solution-blueprint.md) recommends federated data access using electronic registries over centralised data stores using below design principles:

1. Social Protection Platforms MUST only have a <mark style="color:blue;">**cache copy**</mark> of data
2. Social Protection Platforms (SPP) MUST fetch ONLY the <mark style="color:blue;">**minimal or aggregated**</mark> data. for e.g.,&#x20;
   * Year of Birth or Age band instead of Date of Birth
   * Count by vehicle types instead of each vehicle info
   * Farmer land total acreage info instead of each identifiable land parcel info, etc.,&#x20;
3. Design/Implementation MUST allow minimal unified <mark style="color:blue;">**read only**</mark> view of data as a cache. Implementations should **avoid**&#x20;
   * Creating centralised data store(s)
   * Enabling capabilities to managing data attributes where legal mandate (i.e source of truth) is with another system(s)
   * Siloed data stores and with no capability to be in automated sync with source system(s)

{% hint style="info" %}
Above principles are also applicable to other domains like Agriculture, Health, Education, etc, where system to system data access is required for service delivery.&#x20;

G2P Connect recommends all systems involved in data exchange to enable below core features for interoperability using G2P connect [Registry APIs](./#interface-list):

1. **search** - System in want of data shall **pull** from source system using search `query`
2. **subscribe** - System in want of data shall **register** to data subscription service(s) using `event(s`) and additional filters (optional) with the source system.
3. **notify** - Source system shall **push** data (on event or agreed frequency) to systems
{% endhint %}

## Consented Data Sharing

User consent is a core tenant of any digital process or digital infrastructure integrations. Registry data access API design accomdates the concept of the concept to enable access to data / services.

Consent for data access is broadly classified in one of below operational modes with design aspects embedded into the core Registry API:

<details>

<summary>Implicit Consent</summary>

Entity or system that is in need of user's data to provide services to the user shall obtain the consent directly from the user to initiate the data access process.

For service requests initiated by beneficairy, Registry APIs allows to send the implicit **consent** in **search** query requests.

_In Social Protection use case, this consent may be obtained during registration of the beneficiary into a social program and the consent may be very specific or broad enough to access required data from various systems, frequency or duration._

**Note:** For benficiary services initiated by entity/system through emergency and/or by legal process or intervention may use "**`authorise`**" attribute to access data.&#x20;



</details>

<details>

<summary>Explicit Consent</summary>

Entity or system that is in need of (or providing) user's data may obtain explicit (i.e informed) consent from a common trusted entity (e.g., consent manager).

**In search flow**, entity/system in want of data shall obtain explicit consent. Entity/system providing access to data shall verify the consent shared in search request was indeed obtained from the common trusted entity (e.g., consent manager) before release the data as part of search query response.

**In subscribe/notify flow**, entity/system providing data shall directly obtain explicit consent with the user and acts as a consent manager.

</details>

<details>

<summary>Implied Consent</summary>

If user has access to verifiable credential(s) through that can be directly shared with entity/system providing the service user intends to avail then this is considered implied consent. If verifiable credential has the required data then no futher action is required to seek additional data, If verifiable credential is an auth token then entity/system providing data can use this as implied consent to release the data through search response.

</details>

### Consent Flows

<table><thead><tr><th width="188.33333333333331">Consent Type</th><th width="263">Data Consumer</th><th>Data Provider</th></tr></thead><tbody><tr><td>Implicit </td><td>search: </td><td>on-search:</td></tr><tr><td>Explicit </td><td>search: </td><td>on-search:</td></tr><tr><td>Implied </td><td>User shares VC to directly avail service</td><td>N/A</td></tr></tbody></table>

{% hint style="info" %}
G2P Connect recommends a digitally signed machine readable consent artefact for trusted data exchange between entities. In the absence of this, the existing paper based, techno-legal approach may work for entities to trust each other to exchange data using Registry APIs.

Use of _`"`**`consent`**`"`_ attribute in APIs is recommend to implement this feature.
{% endhint %}

## Authorised Data Sharing

In emergency scenarios where local laws allow intervention access to critical data on time is critical to reach out to beneficiaries in need to provide immediate relief. In these scenarios, obtaining regular consent may not practically possible.&#x20;

Authorise attribute in search/subsribe requests enable data providers to share data to requesting entity. Authorise attribute may contain document reference that enable access to user data for specific purpose. Systems may audit this information for future references. &#x20;

## Interoperability

G2P Connect specifications is an attempt to enable interoperability both at Technology and Domain layers.

**Technology** interoperability of the APIs are based on [design](broken-reference) principles to enable communcation/messaging protocol between systems in a [trusted](../../security/) manner. for e.g.,

* Transport layer agnostic support using REST, file exchange or message queues
* Sync/Async modes
* Reliable message delivery
* End to end payload security, non-repudiable capabilities&#x20;

{% hint style="info" %}
Auditability of data exchange requests is not in scope of these interfaces. As a best practice, registries that are providing data access services and systems consuming data should have good auditing mechanism built-in.

G2P Connect does recommend to implement **consent** and authorised data artefacts to request and service &#x20;
{% endhint %}

Additionally, G2P Connect Registry APIs are designed to accomodate various **Domain** process flows, data/message structures for data exchange that are country/department/use case context specific.

## References

1. API specifications - [html](https://g2p-connect.github.io/specs/release/html/registry\_core\_api\_v1.0.0.html) | [yaml](https://g2p-connect.github.io/specs/release/yaml/registry\_core\_api\_v1.0.0.yaml)
2. Discussion [thread](https://github.com/G2P-Connect/.github/discussions)

***

## Additional Information

{% tabs %}
{% tab title="Interface List" %}
**`Async`**

1. /registry/subscribe - Subscribe for an event with registry
2. /registry/notify - Notify with data upon event or requested frequency
3. /registry/search - Search request using key identifiers or simple queries
4. /registry/on-search - Search results through callback
5. /registry/txn/status - Status check request for Async API using txn id or ref id
6. /registry/txn/on-status - Status check response through callback

**`Sync`**&#x20;

1. /registry/sync/search - Search request/response on same thread
2. /registry/sync/subscriptions -  Fetch registered subscriptions
3. /registry/sync/unsubscribe - Unsubscribe to stop receiving data on notify API
4. /registry/sync/txn/status - Async APIs status check invoked synchronously
{% endtab %}

{% tab title="Registry Types" %}
The Implementating systems are free to define registry type values using /[.well-known](https://en.wikipedia.org/wiki/Well-known\_URI) folder as meta data for integration.&#x20;

{% hint style="info" %}
Registry type is an optional value to indicate registry to query against and notify using event subscription. This is useful in case of system hosting multiple registries under an entity id!
{% endhint %}

{% @github-files/github-code-block url="https://github.com/G2P-Connect/specs/blob/5edb5d8ab179ccb3110769ce975bfe806452e897/src/registry/schema/core/RegistryType.yaml" %}
{% endtab %}

{% tab title="Event Types" %}
The Implementating systems are free to define event type values using /[.well-known](https://en.wikipedia.org/wiki/Well-known\_URI) folder as meta data for integration.&#x20;

{% hint style="info" %}
Event type is a mandatory value to indicate subscription service offered by a registry to notify data upon occurrence of an event. Optionally subscribers may opt for aggregated data push at requested frequency!

&#x20;

Events can be defined at domain level registries. e.g., civil, farmer, student, disability, social, etc.,&#x20;
{% endhint %}

***

**Civil Registry**

{% @github-files/github-code-block url="https://github.com/G2P-Connect/specs/blob/5edb5d8ab179ccb3110769ce975bfe806452e897/src/registry/schema/civil/EventType.yaml" %}

#### Functional Registry

{% @github-files/github-code-block url="https://github.com/G2P-Connect/specs/blob/5edb5d8ab179ccb3110769ce975bfe806452e897/src/registry/schema/core/EventType.yaml" fullWidth="true" %}

&#x20;
{% endtab %}

{% tab title="Consent" %}


{% @github-files/github-code-block url="https://github.com/G2P-Connect/specs/blob/7a04e8c01910af7cb1256d5734221d5d25db6f74/src/common/schema/Consent.yaml" %}


{% endtab %}

{% tab title="Authorise" %}


{% @github-files/github-code-block url="https://github.com/G2P-Connect/specs/blob/7a04e8c01910af7cb1256d5734221d5d25db6f74/src/common/schema/Consent.yaml" %}


{% endtab %}
{% endtabs %}
