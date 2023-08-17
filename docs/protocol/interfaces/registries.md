# Registries

### Overview

In any social protection use case, access to beneficiary data from various department systems is key to determine beneficiary eligibility to a social assitance program/scheme.&#x20;

In social protection platform delivery chain, criteria like below are common to determine an eligible beneficiary:

1. Be part of bottom x% of beneficiaries against set of social indicators;
2. Income levels;
3. Assets - house, land, vehicle ownership, etc.,&#x20;
4. Expenditure - Electricity units consumed;
5. **Not** be part of other social programs - e.g Unemployment pension, Farmer Support Income, etc.,
6. etc.,

One traditional approach is to create centralised database or data lake using one time pull/push of data sets from all relevant dept systems. This approach ends up creating a centralised data store where data quickly gets out of sync with source systems and social program administrators can't target the right beneficiaries.

While the G2P Connect [blueprint](../../g2p-connect/solution-blueprint.md) ideally recommends federated data access over centralised data stores there are complex scenarios like eligibility criteria no 1 (above) to determine bottom x% requires pooling beneficairy data into one store!

G2P Connect recommends source of truth to all functional attributes (like above eligible criteria info) must **reside** with legal custodian departments/entities while G2P program administering systems should only act as data consumer and be in sync with source systems.&#x20;

To make this technically feasibile, G2P Connect recommends all relevant systems to enable interoperable APIs to exchange data:

1. **search** - System in want of data shall pull from source system
2. **subscribe** - Systems in want of data shall register with source system&#x20;
3. **notify** - Source system push the data (at agreed frequency) to systems registered for this service using susbscribe option (point no 2)

G2P Connect specifications is an attempt to enable interoperability both at Technical and Domain level.

Technology interoperability of the APIs is achievied thorugh these [design principles](broken-reference) - transport layer agnostic support using APIs, file exchange or message queues, work both in sync/async modes, support message delivery in a secure, reliable and non-repudiable way.

Additionally, G2P Connect Registry APIs are designed to accomodate various process flows, data/message structures for data exchange that country/department/context specific.

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

