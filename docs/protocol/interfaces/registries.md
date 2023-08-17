# Registries

### Overview

1. Standardising search, subscription, notification capabilities between G2P enabling DPGS/Products/Systems

### References

1. API specification [link](https://g2p-connect.github.io/specs/release/html/registry\_core\_api\_v1.0.0.html)
2. Discussion [thread](https://github.com/G2P-Connect/.github/discussions)

### Interface List

| Interface ID | End Point                    | Description                                                                       |
| ------------ | ---------------------------- | --------------------------------------------------------------------------------- |
| REG-SUB      | POST /registry/subscribe     | Subscribe to a life event with registry                                           |
| REG-NTFY     | POST /registry/notify        | REG to notify a life event to subscrbiers                                         |
| REG-SRCH     | POST /registry/serach        | Search person(s) in registry using key identifiers                                |
| REG-ON-SRCH  | POST /registry/on-search     | Search results through callback                                                   |
| REG-STS      | POST /registry/txn/status    | Status check on any of the crvs actions using transaction\_id or reference\_id(s) |
| REG-ON-STS   | POST /registry/txn/on-status | Status check response through callback                                            |

### Registry Types

G2P Connect indicative list of registries:

{% @github-files/github-code-block url="https://github.com/G2P-Connect/specs/blob/5edb5d8ab179ccb3110769ce975bfe806452e897/src/registry/schema/core/RegistryType.yaml" %}

This specificaion recommends implemenations to extend custom registries as part of country specific requirements. The registry specific payload can be extended in the G2P connect registry sericce end points.

### Regisry Sub-Event Types

#### Civil Registry

{% @github-files/github-code-block url="https://github.com/G2P-Connect/specs/blob/5edb5d8ab179ccb3110769ce975bfe806452e897/src/registry/schema/civil/EventType.yaml" %}

#### Functional Registry

{% @github-files/github-code-block url="https://github.com/G2P-Connect/specs/blob/5edb5d8ab179ccb3110769ce975bfe806452e897/src/registry/schema/core/EventType.yaml" %}

### Integration Schematics

