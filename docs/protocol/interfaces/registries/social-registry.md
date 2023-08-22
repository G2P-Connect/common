# Social Registry

## Overview

In any social protection use case, access to beneficiary data from various department systems is key to determine beneficiary eligibility to a social assitance program/scheme.&#x20;

In social protection platform, criteria like below are common to determine an eligible beneficiary:

1. Be part of bottom x% of beneficiaries against set of social indicators;
2. Income at household level;
3. Assets - house, land, vehicle ownership, etc;
4. Expenditure - Electricity units consumed; last x months, avg of last x months, etc;
5. Not be part of other social programs - e.g Unemployment pension, Farmer Support Income, etc;

One traditional approach is to create centralised database or data lake using one time pull/push of data sets from all relevant dept systems. This approach ends up creating a centralised data store where data quickly gets out of sync with source systems and social program administrators can't target the right beneficiaries.

Complex scenarios (like below) forces implementations of social protection platforms to build **centralised data stores** and soon the data goes **out of sync** with source systems.

1. Criteria #1 (above) to determine bottom x% requires pooling all beneficiary data into one single data store&#x20;
2. Criteria #5 (above) where proving beneficiary is **NOT** part of a registry

G2P Connect recommends creating cache data complying to federated data access [principles](./).
