# Registries

### Overview

In any social protection use case, access to beneficiary data from various department systems is key to determine beneficiary eligibility to a social assitance program/scheme.&#x20;

In social protection platform delivery chain, criteria like below are common to determine an eligible beneficiary:

1. Be part of bottom x% of beneficiaries against set of social indicators;
2. Income levels;
3. Assets - house, land, vehicle ownership, etc.,&#x20;
4. Expenditure - Electricity units consumed;
5. Not be part of other social programs - e.g Unemployment pension, Farmer Support Income, etc.,
6. etc.,

One traditional approach is to create centralised database or data lake using one time pull/push of data sets from all relevant dept systems. This approach ends up creating a centralised data store where data quickly gets out of sync with source systems and social program administrators can't target the right beneficiaries.

Complex scenarios like eligibility a) criteria #1 (above) to determine bottom x% requires pooling beneficairy data into one store that act as a cache, b) criteria #5 (above) where proving beneficiary is NOT part of a registry, etc., forces implementations of social protection platforms to build centralised data stores and soon the data goes out of sync with source systems.

G2P Connect [blueprint](../../g2p-connect/solution-blueprint.md) recommends federated data access over centralised data stores using below design principles:

1. <mark style="color:blue;">**Source of truth**</mark> to all functional attributes (like above eligible criteria info) MUST reside <mark style="color:blue;">**with legal custodian**</mark> department systems. Social Protection Platforms should only act as data consumer and be in sync with source systems.&#x20;
2. Social Protection Platforms MUST fetch ONLY the <mark style="color:blue;">**minimal or aggregated**</mark> data sets. for e.g., Year of Birth instead of Date of Birth, Count by vehicle types owned instead of individual vehicle details, Farmer land ownership total acerage info instead of individual land parcel identifiers, etc.,&#x20;
3. Design/Implementation MUST allow unified <mark style="color:blue;">**read only**</mark> view of data instead of centralised  local copies allowing updates!

To enable above design principles technically feasibile, G2P Connect recommends all relevant systems to enable below core end points as interoperable APIs to exchange data:

1. **search** - System in want of data shall pull from source system
2. **subscribe** - Systems in want of data shall register with source system&#x20;
3. **notify** - Source system push the data (at agreed frequency) to systems registered for this service using susbscribe option (point no 2)

G2P Connect specifications is an attempt to enable interoperability both at Technical and Domain level.

Technology interoperability of the APIs is achievied thorugh these [design principles](broken-reference) i.e., transport layer agnostic support using APIs, file exchange or message queues, work both in sync/async modes, support message delivery in a secure, reliable and non-repudiable way.

Additionally, G2P Connect Registry APIs are designed to accomodate various process flows, data/message structures for data exchange that are country/department/use case context specific to address Social Protection and other domains like agriculture.

### References

1. API specification [link](https://g2p-connect.github.io/specs/release/html/registry\_core\_api\_v1.0.0.html)
2. Discussion [thread](https://github.com/G2P-Connect/.github/discussions)

### Interface List

| Interface ID      | End Point                         | Description                                                                          |
| ----------------- | --------------------------------- | ------------------------------------------------------------------------------------ |
| REG-SUB           | POST /registry/subscribe          | Subscribe to a life event with registry                                              |
| REG-NTFY          | POST /registry/notify             | REG to notify a life event to subscrbiers                                            |
| REG-SRCH          | POST /registry/search             | Search person(s) in registry using key identifiers or simple queries                 |
| REG-ON-SRCH       | POST /registry/on-search          | <p>Search results through </p><p>callback</p>                                        |
| REG-STS           | POST /registry/txn/status         | Status check on any of the crvs actions using transaction\_id or reference\_id(s)    |
| REG-ON-STS        | POST /registry/txn/on-status      | Status check response through callback                                               |
| REG-SYNC-SRCH     | POST /registry/sync/search        | Sync based search. The response is received on the same requesting thread            |
| REG-SUBSCRIPTIONS | POST /registry/sync/subscriptions | Fetch subscriptions based on registry events                                         |
| REG-UNSUBSCRIBE   | POST /registry/sync/unsubscribe   | Unsubscribe subscription requests to stop receiving notification events              |
| REG-SYNC-STS      | POST /registry/sync/txn/status    | Sync based txn status check. The response is received on the same requesting thread. |

### Registry Types

G2P Connect indicative list of registries. The Implementating systems are free to define registry type values.&#x20;

Specificaion allows implemenations to extend custom registries as part of country specific requirements. The registry specific payload can be extended in the G2P connect registry search/subscribe end points.



{% @github-files/github-code-block url="https://github.com/G2P-Connect/specs/blob/5edb5d8ab179ccb3110769ce975bfe806452e897/src/registry/schema/core/RegistryType.yaml" %}

### Regisry Sub-Event Types

#### Civil Registry

Indicative reference list of civil registry events. Implementing systems can override&#x20;

{% @github-files/github-code-block url="https://github.com/G2P-Connect/specs/blob/5edb5d8ab179ccb3110769ce975bfe806452e897/src/registry/schema/civil/EventType.yaml" %}

#### Functional Registry

{% @github-files/github-code-block url="https://github.com/G2P-Connect/specs/blob/5edb5d8ab179ccb3110769ce975bfe806452e897/src/registry/schema/core/EventType.yaml" %}

### Integration Schematics

