# Cloud Infrastructure

The common cloud-infrastructure vocabulary -- the instances that compute, the volumes and snapshots that store, the networks, firewalls and load balancers that connect, the SSH keys that let people in and the regions it all runs in -- distilled from the APIs that already exist. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Cloud, Cloud Computing, IaaS, Compute, Infrastructure, GPU, Data Center, Hosting, VPS, Bare Metal in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| instance | 88 | 83 | id 77%, name 76%, status 75%, created_at 55% | instances 49, servers 22, vms 10, vm 8 |
| volume | 38 | 36 | id 83%, name 81%, status 78%, created_at 64%, size 58%, type 50% | volumes 24, disks 13, volume 3, disk 3 |
| snapshot | 31 | 29 | id 90%, name 79%, status 72%, created_at 62% | snapshots 25, snapshot 9, backups 6, images 3 |
| network | 36 | 34 | name 82%, id 76% | networks 17, subnets 12, vpcs 10, network 6 |
| firewall | 21 | 20 | id 85%, name 75%, status 65%, created_at 50% | firewalls 12, security-groups 4, firewall 2, security_groups 2 |
| load_balancer | 14 | 13 | name 92%, id 85%, metadata 62%, region 62%, status 62%, created_at 54% | load-balancers 5, load_balancers 4, loadbalancers 3, nodebalancers 1 |
| ssh_key | 24 | 24 | name 88%, public_key 88%, id 83%, created_at 58%, fingerprint 54% | ssh-keys 14, sshkeys 4, keypairs 4, keys 2 |
| region | 29 | 29 | name 79%, id 52% | regions 26, datacenters 6, region 1 |

Cohort: 384 providers with harvested OpenAPI, 88 crosswalks. Of the providers exposing a instance: 62% create, 93% list, 82% fetch, 48% update, 67% delete. Generated 2026-11-10 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- Cloud infrastructure agrees more than any other technical context: an SSH key (name, public key, fingerprint), a volume (size, type, status) and a load balancer (region, status) are shared almost completely across clouds, VPS hosts and GPU clouds.
- An instance agrees only on name, status, id and created_at: size, image and IP address are each named by every provider in its own way (size, plan, flavor, machine_type, shape).
- Azure maps poorly however it is measured: Azure Resource Manager nests every field of every resource under `properties`, so the common names sit one level down.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- An instance covers servers, droplets, virtual machines, Linodes and GPU pods; singular `/server` is a load-balancer backend or syslog target and does not count.
- A snapshot covers backups only beneath an instance, server or volume: database backups belong with the database.
- A network covers VPCs, private networks and subnets; a firewall covers security groups.
- Duplicates merged: Packet into Equinix, M0, TiDB into PingCAP, Platform.sh into Upsun, Sail.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
