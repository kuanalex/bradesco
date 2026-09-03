# Executive summary

| Overall status | Operational risk | Immediate action required |
|---|---|---|
| Yellow | Moderate | Prune node /var disk usage and inspect single-replica PDBs |

**Status: Yellow — the cluster and core platform are operational, with attention required on node disk warnings, single-replica PodDisruptionBudgets, and historical operator states.**

* All 10 cluster and node infrastructure health checks passed.
* Red Hat cert-manager is active and certificate renewals are valid.
* All 22 detected service CRs Completed.
* Ceph storage cluster is in Ready state with raw capacity utilization at 35.55%.

The OpenShift infrastructure, foundational database services, and Software Hub services are running and healthy. Attention is required to monitor and prune disk usage on nodes master-0, master-2, and datastage-worker-brazilsouth3-xtpsj (OCP-001), account for 17 single-replica PodDisruptionBudgets blocking voluntary evictions (OCP-002), address ephemeral local storage limits for the search service deployment (SWH-002), and clean up historical failed CSV artifacts in OLM (OCP-004).

**Service impact (cpd-cli monitor events):** no service or service-instance critical events this collection.

## Top risks

| Priority | Risk | Impact | Required action | Finding |
|---|---|---|---|---|
| 1 | 17 single-replica PodDisruptionBudgets have allowedDisruptions=0 | Voluntary node drains or evictions during maintenance will be blocked | Plan node maintenance around single-replica workloads | OCP-002 |

# Environment profile

## Cluster overview

| Item | Value |
|---|---|
| Cluster name | aroprplatfudi21-9w577 |
| OpenShift version | 4.18.45 |
| IBM Software Hub release | 5.3.1 |
| cpd_platform (ibmcpd) | v5.3 |
| Operators namespace | gov-operators |
| Operands namespace | gov-operands |
| FIPS mode | Disabled |
| Network mode | Connected |
| Software Hub installed | Yes |
| Storage provider | OpenShift Data Foundation (Ceph 19.2.1-375) |
| Storage classes | ocs-storagecluster-ceph-rbd, ocs-storagecluster-cephfs |
| IAM integration | Enabled |
| CPD route | cpd-gov-operands.apps.aroprplatfudi21.arocorp.bradesco.com.br |
| Image mirror ICSP | image-policy-0, image-policy-1, image-policy-2 |
| PodDisruptionBudgets | 46 |
| Helm releases | 76 (76 deployed) |

## Version

| Component | Observed version | Interpretation |
|---|---|---|
| OpenShift Container Platform | 4.18.45 | Release 4.18.45 deployed on Azure Red Hat OpenShift. |
| IBM Software Hub | 5.3.1 | Release 5.3.1 deployed and operational. |

## Node inventory

| Node | Role | vCPU | Memory | Age |
|---|---|---|---|---|
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-57klq | worker | 32 | 125.8 GiB | 131d |
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-5vhph | worker | 32 | 125.8 GiB | 88d |
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-zkm9t | worker | 32 | 125.8 GiB | 93d |
| aroprplatfudi21-9w577-avim-worker-brazilsouth2-k2dmt | worker | 32 | 125.8 GiB | 93d |
| aroprplatfudi21-9w577-avim-worker-brazilsouth2-vjfcl | worker | 32 | 125.8 GiB | 88d |
| aroprplatfudi21-9w577-avim-worker-brazilsouth3-jdmqx | worker | 32 | 125.8 GiB | 88d |
| aroprplatfudi21-9w577-avim-worker-brazilsouth3-lkkgb | worker | 32 | 125.8 GiB | 93d |
| aroprplatfudi21-9w577-datastage-worker-brazilsouth1-nhplr | worker | 32 | 125.8 GiB | 435d |
| aroprplatfudi21-9w577-datastage-worker-brazilsouth2-lwd8d | worker | 32 | 125.8 GiB | 339d |
| aroprplatfudi21-9w577-datastage-worker-brazilsouth3-xtpsj | worker | 32 | 125.8 GiB | 162d |
| aroprplatfudi21-9w577-infra-brazilsouth1-b7wzm | worker | 16 | 125.8 GiB | 418d |
| aroprplatfudi21-9w577-infra-brazilsouth2-nbz9p | worker | 16 | 125.8 GiB | 418d |
| aroprplatfudi21-9w577-infra-brazilsouth3-9gz4k | worker | 16 | 125.8 GiB | 418d |
| aroprplatfudi21-9w577-master-0 | master | 16 | 62.8 GiB | 715d |
| aroprplatfudi21-9w577-master-1 | master | 16 | 62.8 GiB | 715d |
| aroprplatfudi21-9w577-master-2 | master | 16 | 62.8 GiB | 715d |
| aroprplatfudi21-9w577-odf-worker-brazilsouth1-6lmbf | worker | 16 | 62.8 GiB | 714d |
| aroprplatfudi21-9w577-odf-worker-brazilsouth2-tc89s | worker | 16 | 62.8 GiB | 714d |
| aroprplatfudi21-9w577-odf-worker-brazilsouth3-9vrqk | worker | 16 | 62.8 GiB | 714d |
| aroprplatfudi21-9w577-platf-worker-brazilsouth1-rbs8b | worker | 32 | 125.8 GiB | 111d |
| aroprplatfudi21-9w577-platf-worker-brazilsouth2-zhdc7 | worker | 32 | 125.8 GiB | 12d |
| aroprplatfudi21-9w577-platf-worker-brazilsouth3-flk9d | worker | 32 | 125.8 GiB | 111d |
| aroprplatfudi21-9w577-worker-brazilsouth1-hgx4r | worker | 32 | 125.8 GiB | 382d |
| aroprplatfudi21-9w577-worker-brazilsouth1-jln4q | worker | 32 | 125.8 GiB | 57d |
| aroprplatfudi21-9w577-worker-brazilsouth1-rjp99 | worker | 32 | 125.8 GiB | 449d |
| aroprplatfudi21-9w577-worker-brazilsouth2-6qwkl | worker | 32 | 125.8 GiB | 382d |
| aroprplatfudi21-9w577-worker-brazilsouth2-brldn | worker | 32 | 125.8 GiB | 61d |
| aroprplatfudi21-9w577-worker-brazilsouth2-d2h5z | worker | 32 | 125.8 GiB | 57d |
| aroprplatfudi21-9w577-worker-brazilsouth3-6jvhs | worker | 32 | 125.8 GiB | 56d |
| aroprplatfudi21-9w577-worker-brazilsouth3-7t9vc | worker | 32 | 125.8 GiB | 28d |
| aroprplatfudi21-9w577-worker-brazilsouth3-hp56d | worker | 32 | 125.8 GiB | 57d |

_Totals: 31 nodes, 848 vCPU, 3521.8 GiB memory._

## Installed services and instances

| Instance | Type | Version | Status | Notes |
|---|---|---|---|---|
| ds-px-default | datastage | 5.3.5 | PROVISIONED | Default DataStage parallel execution engine instance. |
| ProfHbIntrnl | spark | 5.3.4 | PROVISIONED | Internal Apache Spark instance supporting profiling workloads. |

_4 internal zen-secrets/volumes instances (PROVISIONED) omitted._

## Assessment scope

| Area | Coverage | Notes |
|---|---|---|
| OpenShift cluster health | Full | Full cluster and node health checks assessed. |
| IBM Software Hub platform | Partial | Workload-state coverage reduced; Catalog Source, Install Plan, Subscriptions, StatefulSet, and Custom Resource checks skipped. |
| Storage | Partial | ODF backend inspected; storage performance tests omitted. |
| Network | Partial | Bandwidth tested across selected worker nodes. |
| Security and certificates | Full | Red Hat cert-manager active; renewal and expiry verified. |
| Upgrade considerations | Not assessed | Routine day-2 health check; no upgrade target assessed. |

# Infrastructure and OpenShift assessment

Status: Yellow — cluster infrastructure and core operators are healthy, with warnings noted on disk utilization and single-replica PodDisruptionBudgets.

All OpenShift control plane components, machine config pools, and cluster operators are available and functional. Master nodes master-0 and master-2 and worker node datastage-worker-brazilsouth3-xtpsj have crossed the 80% disk threshold on /var, and 17 single-replica PodDisruptionBudgets currently allow zero disruptions.

## Cluster health

_All 10 checks passed._

## Node resource usage

| Node | CPU usage | CPU % | Memory used | Memory % |
|---|---|---|---|---|
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-57klq | 1.1 CPU | 3.5% | 23.7 GiB | 18.8% |
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-5vhph | 1 CPU | 3.1% | 24.9 GiB | 19.8% |
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-zkm9t | 1 CPU | 3.2% | 37 GiB | 29.4% |
| aroprplatfudi21-9w577-avim-worker-brazilsouth2-k2dmt | 1.2 CPU | 3.9% | 28.6 GiB | 22.7% |
| aroprplatfudi21-9w577-avim-worker-brazilsouth2-vjfcl | 0.7 CPU | 2.1% | 23.9 GiB | 19% |
| aroprplatfudi21-9w577-avim-worker-brazilsouth3-jdmqx | 0.9 CPU | 2.8% | 11.2 GiB | 8.9% |
| aroprplatfudi21-9w577-avim-worker-brazilsouth3-lkkgb | 0.5 CPU | 1.7% | 12.8 GiB | 10.2% |
| aroprplatfudi21-9w577-datastage-worker-brazilsouth1-nhplr | 6.3 CPU | 19.8% | 26.9 GiB | 21.4% |
| aroprplatfudi21-9w577-datastage-worker-brazilsouth2-lwd8d | 1.1 CPU | 3.3% | 31.3 GiB | 24.9% |
| aroprplatfudi21-9w577-datastage-worker-brazilsouth3-xtpsj | 0.8 CPU | 2.6% | 28.1 GiB | 22.3% |
| aroprplatfudi21-9w577-infra-brazilsouth1-b7wzm | 2.1 CPU | 13% | 25.8 GiB | 20.5% |
| aroprplatfudi21-9w577-infra-brazilsouth2-nbz9p | 1 CPU | 6.1% | 13.2 GiB | 10.5% |
| aroprplatfudi21-9w577-infra-brazilsouth3-9gz4k | 1.7 CPU | 10.9% | 17.4 GiB | 13.8% |
| aroprplatfudi21-9w577-master-0 | 2.7 CPU | 17.1% | 30.9 GiB | 49.2% |
| aroprplatfudi21-9w577-master-1 | 2.6 CPU | 16.1% | 28.4 GiB | 45.3% |
| aroprplatfudi21-9w577-master-2 | 1.8 CPU | 11.3% | 28.4 GiB | 45.2% |
| aroprplatfudi21-9w577-odf-worker-brazilsouth1-6lmbf | 0.5 CPU | 3.1% | 15.2 GiB | 24.2% |
| aroprplatfudi21-9w577-odf-worker-brazilsouth2-tc89s | 0.4 CPU | 2.2% | 16.1 GiB | 25.6% |
| aroprplatfudi21-9w577-odf-worker-brazilsouth3-9vrqk | 0.4 CPU | 2.4% | 7.9 GiB | 12.6% |
| aroprplatfudi21-9w577-platf-worker-brazilsouth1-rbs8b | 1 CPU | 3.1% | 25.3 GiB | 20.1% |
| aroprplatfudi21-9w577-platf-worker-brazilsouth2-zhdc7 | 0.7 CPU | 2.3% | 24.2 GiB | 19.2% |
| aroprplatfudi21-9w577-platf-worker-brazilsouth3-flk9d | 1 CPU | 3% | 13.3 GiB | 10.6% |
| aroprplatfudi21-9w577-worker-brazilsouth1-hgx4r | 1.6 CPU | 5% | 35.6 GiB | 28.3% |
| aroprplatfudi21-9w577-worker-brazilsouth1-jln4q | 0.5 CPU | 1.6% | 9.2 GiB | 7.3% |
| aroprplatfudi21-9w577-worker-brazilsouth1-rjp99 | 2.5 CPU | 7.7% | 43.5 GiB | 34.6% |
| aroprplatfudi21-9w577-worker-brazilsouth2-6qwkl | 1 CPU | 3.1% | 30.9 GiB | 24.6% |
| aroprplatfudi21-9w577-worker-brazilsouth2-brldn | 2.1 CPU | 6.5% | 31.2 GiB | 24.8% |
| aroprplatfudi21-9w577-worker-brazilsouth2-d2h5z | 3.7 CPU | 11.6% | 26.8 GiB | 21.3% |
| aroprplatfudi21-9w577-worker-brazilsouth3-6jvhs | 5.1 CPU | 15.8% | 24.7 GiB | 19.6% |
| aroprplatfudi21-9w577-worker-brazilsouth3-7t9vc | 0.4 CPU | 1.1% | 8.2 GiB | 6.5% |
| aroprplatfudi21-9w577-worker-brazilsouth3-hp56d | 0.9 CPU | 2.9% | 13.5 GiB | 10.7% |

## Disk usage

| Node | Size | Used | Available | Use % | Status |
|---|---|---|---|---|---|
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-57klq | 512G | 373G | 140G | 73% | OK |
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-5vhph | 512G | 159G | 353G | 31% | OK |
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-zkm9t | 512G | 230G | 283G | 45% | OK |
| aroprplatfudi21-9w577-avim-worker-brazilsouth2-k2dmt | 512G | 205G | 308G | 40% | OK |
| aroprplatfudi21-9w577-avim-worker-brazilsouth2-vjfcl | 512G | 178G | 335G | 35% | OK |
| aroprplatfudi21-9w577-avim-worker-brazilsouth3-jdmqx | 512G | 157G | 356G | 31% | OK |
| aroprplatfudi21-9w577-avim-worker-brazilsouth3-lkkgb | 512G | 190G | 323G | 37% | OK |
| aroprplatfudi21-9w577-datastage-worker-brazilsouth1-nhplr | 512G | 314G | 198G | 62% | OK |
| aroprplatfudi21-9w577-datastage-worker-brazilsouth2-lwd8d | 512G | 392G | 120G | 77% | OK |
| aroprplatfudi21-9w577-datastage-worker-brazilsouth3-xtpsj | 512G | 420G | 93G | 82% | Warn |
| aroprplatfudi21-9w577-infra-brazilsouth1-b7wzm | 512G | 188G | 324G | 37% | OK |
| aroprplatfudi21-9w577-infra-brazilsouth2-nbz9p | 512G | 261G | 252G | 51% | OK |
| aroprplatfudi21-9w577-infra-brazilsouth3-9gz4k | 512G | 88G | 425G | 18% | OK |
| aroprplatfudi21-9w577-master-0 | 1.0T | 847G | 177G | 83% | Warn |
| aroprplatfudi21-9w577-master-1 | 1.0T | 143G | 882G | 14% | OK |
| aroprplatfudi21-9w577-master-2 | 1.0T | 852G | 172G | 84% | Warn |
| aroprplatfudi21-9w577-odf-worker-brazilsouth1-6lmbf | 300G | 144G | 156G | 49% | OK |
| aroprplatfudi21-9w577-odf-worker-brazilsouth2-tc89s | 300G | 152G | 149G | 51% | OK |
| aroprplatfudi21-9w577-odf-worker-brazilsouth3-9vrqk | 300G | 158G | 143G | 53% | OK |
| aroprplatfudi21-9w577-platf-worker-brazilsouth1-rbs8b | 512G | 217G | 296G | 43% | OK |
| aroprplatfudi21-9w577-platf-worker-brazilsouth2-zhdc7 | 512G | 82G | 430G | 16% | OK |
| aroprplatfudi21-9w577-platf-worker-brazilsouth3-flk9d | 512G | 212G | 301G | 42% | OK |
| aroprplatfudi21-9w577-worker-brazilsouth1-hgx4r | 512G | 316G | 197G | 62% | OK |
| aroprplatfudi21-9w577-worker-brazilsouth1-jln4q | 512G | 62G | 450G | 13% | OK |
| aroprplatfudi21-9w577-worker-brazilsouth1-rjp99 | 512G | 303G | 209G | 60% | OK |
| aroprplatfudi21-9w577-worker-brazilsouth2-6qwkl | 512G | 321G | 192G | 63% | OK |
| aroprplatfudi21-9w577-worker-brazilsouth2-brldn | 512G | 195G | 318G | 38% | OK |
| aroprplatfudi21-9w577-worker-brazilsouth2-d2h5z | 512G | 186G | 326G | 37% | OK |
| aroprplatfudi21-9w577-worker-brazilsouth3-6jvhs | 512G | 185G | 328G | 37% | OK |
| aroprplatfudi21-9w577-worker-brazilsouth3-7t9vc | 512G | 57G | 456G | 11% | OK |
| aroprplatfudi21-9w577-worker-brazilsouth3-hp56d | 512G | 185G | 328G | 37% | OK |

_Filesystem: node `/var` (where container ephemeral storage lives)._

## OLM and operator state

| Check | Result |
|---|---|
| cert-manager-operator.v1.19.1 | Succeeded |
| cluster-logging.v6.4.6 | Succeeded |
| elasticsearch-operator.v5.8.21 | Succeeded |
| openshift-gitops-operator.v1.17.1 | Succeeded |
| rhacs-operator.v4.11.3 | Succeeded |
| rhods-operator.2.25.10 | Succeeded |
| Manual subscriptions | 2 found |
| Failed/pending CSVs | 23 found |

_All 6 operator CSVs Succeeded._

## PodDisruptionBudgets at zero disruptions

| PDB | Namespace | Allowed disruptions | Single replica | Severity |
|---|---|---|---|---|
| ccs-cams-postgres-primary | avim-operands | 0 | yes | High |
| ccs-cams-postgres-primary | gov-operands | 0 | yes | High |
| ccs-jobs-postgres-primary | avim-operands | 0 | yes | High |
| ccs-jobs-postgres-primary | gov-operands | 0 | yes | High |
| common-service-db-primary | avim-operands | 0 | yes | High |
| common-service-db-primary | gov-operands | 0 | yes | High |
| common-service-db-primary | platf-operands | 0 | yes | High |
| ibm-lh-postgres-edb-primary | platf-operands | 0 | yes | High |
| ikc-activity-lineage-postgres-primary | gov-operands | 0 | yes | High |
| ikc-dp-dps-bidata-mde-mdi-postgres-primary | gov-operands | 0 | yes | High |
| ikc-glossary-workflow-postgres-primary | gov-operands | 0 | yes | High |
| spark-hb-cloud-native-postgresql-primary | gov-operands | 0 | yes | High |
| spark-hb-cloud-native-postgresql-primary | platf-operands | 0 | yes | High |
| wdp-profiling-cloud-native-postgresql-primary | gov-operands | 0 | yes | High |
| zen-metastore-edb-primary | avim-operands | 0 | yes | High |
| zen-metastore-edb-primary | gov-operands | 0 | yes | High |
| zen-metastore-edb-primary | platf-operands | 0 | yes | High |

_17 of 46 PodDisruptionBudgets currently allow zero disruptions, so a voluntary eviction such as a node drain is blocked or will violate the budget; the rest have disruption headroom and are not listed. Single replica means the budget guards a workload with no second instance._

## Certificates

| Check | Result | Interpretation |
|---|---|---|
| Cert-manager controller | redhat active (determination: redhat) | Red Hat cert-manager active and managing cluster certificates. |
| Certificates Healthcheck | PASS | All evaluated certificates valid with renewal and expiry within expected ranges. |

# IBM Software Hub assessment

Status: Yellow — core platform services are operational, with attention required on pod local storage eviction, operator CPU utilization, and historical CSV states.

Foundational services and databases are healthy and running. All 22 detected service custom resources are Completed. One search pod experienced local ephemeral storage eviction, and the DataStage operator pod exceeded the 90% CPU threshold.

## Service CR status

| Kind | Name | Version | Status | Has hotfix | Notes |
|---|---|---|---|---|---|
| ccs | ccs-cr | 12.1.6 | Completed | Yes (3 digest overrides) | Carries 3 image-digest overrides (SWH-001). |

_All 22 detected service CRs Completed. 1 is shown because it carries a hotfix signal; 21 Completed CRs with no hotfix signal are omitted. The Version column is each CR's own operand version, distinct from the 5.3.1 platform release._

## Control plane health

| Check | Result | Interpretation |
|---|---|---|
| CPFS overall status |  | Foundational services healthy and reconciled. |
| Catalog Source Healthcheck | Skipped | Catalog source readiness check skipped during collection. |
| Install Plan Healthcheck | Skipped | Install plan approval policy check skipped during collection. |
| Subscriptions Healthcheck | Skipped | Subscription CSV status check skipped during collection. |
| Pod Healthcheck (wkc-search-59bc9fd476-dr256) | FAIL | Ephemeral storage limit exceeded resulting in pod termination (SWH-002). |
| Stateful Set Healthcheck | Skipped | StatefulSet replica check skipped during collection. |
| Custom Resource Healthcheck | Skipped | Custom resource state check skipped during collection. |

_16 of 22 checks passed and are not listed individually. CPFS overall status above is the foundational-services rollup, reported separately and not one of these checks._

## Operator resource thresholds

| Pod | Issue | Value | Limit | Usage |
|---|---|---|---|---|
| ibm-cpd-datastage-operator-745568c486-ldknq | CPU usage exceeded 90% threshold | N/A | N/A | N/A |

_Value/Limit/Usage: cpd-cli reports a 90% threshold breach, not millicores (see data gaps)._

# Storage and network assessment

Status: Green — storage backend and network throughput meet requirements.

The OpenShift Data Foundation storage cluster is in a Ready state with Ceph healthy and raw capacity utilization at 35.55%. Network throughput between tested worker nodes exceeds the minimum configured bandwidth threshold.

## Storage validation and backend

| Check | Result | Follow-up |
|---|---|---|
| ODF / Ceph backend | storagecluster Ready, ceph HEALTH_OK, raw used 35.55% | StorageCluster in Ready phase with Ceph healthy; raw capacity utilization is 35.55%. |
| Volume snapshot classes | 4 present | VolumeSnapshotClasses present and configured. |

## Network performance

| Node | Min throughput | Threshold | Status |
|---|---|---|---|
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-57klq | 1104 MB/s | 350 MB/s | PASS |
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-5vhph | 1171 MB/s | 350 MB/s | PASS |
| aroprplatfudi21-9w577-avim-worker-brazilsouth1-zkm9t | 1168 MB/s | 350 MB/s | PASS |
| aroprplatfudi21-9w577-avim-worker-brazilsouth2-k2dmt | 1104 MB/s | 350 MB/s | PASS |
| aroprplatfudi21-9w577-avim-worker-brazilsouth2-vjfcl | 1175 MB/s | 350 MB/s | PASS |

_Tested 5 of 31 nodes (the configured perf-node subset); min throughput is each node's slowest pair, either direction._

# Upgrade considerations

This assessment is a routine operational health check for release 5.3.1. No upgrade target was evaluated during this collection.

Before planning any future upgrade, review the findings register regarding node disk utilization (OCP-001), single-replica PodDisruptionBudgets (OCP-002), operator subscription approval policies (OCP-003), historical failed CSVs in OLM (OCP-004), and image-digest overrides on custom resources (SWH-001).

# Findings register and recommendations

Finding IDs: OCP = OpenShift infrastructure; SWH = Software Hub platform. Severity: Critical / High / Medium / Low.

| ID | Sev. | Finding | Recommendation | Owner | Timeframe | Validation |
|---|---|---|---|---|---|---|
| OCP-001 | Medium | Master nodes master-0 (83%) and master-2 (84%), and worker node datastage-worker-brazilsouth3-xtpsj (82%) have crossed the 80% disk utilization threshold on /var. | Clean up unused container images, journal logs, and temporary files on affected nodes. | Cluster Admin | Within two weeks | Confirm node /var disk utilization drops below 80%. |
| OCP-002 | High | 17 single-replica PodDisruptionBudgets currently allow zero disruptions, blocking voluntary node drains or evictions. | Review single-replica PDBs during node maintenance and ensure high-availability replicas where required. | Cluster Admin | Within one week | Verify allowed disruptions headroom across PDB configurations. |
| OCP-003 | Low | Two operator subscriptions (redhat-oadp-operator, openshift-gitops-operator) have installPlanApproval configured as Manual. | Review subscription approval policies according to organizational standards. | Cluster Admin | Next maintenance window | Check subscription spec installPlanApproval settings. |
| OCP-004 | Medium | 23 older operator CSV versions remain in Failed or Pending phase in the ibm-operators namespace. | Clean up obsolete failed CSVs from prior operator upgrades to prevent OLM reconciliation clutter. | Cluster Admin | Within two weeks | Verify failed CSVs are removed from the operator namespace. |
| SWH-001 | Medium | Custom resource ccs-cr contains 3 image-digest overrides, indicating hotfix or specific patch image pins. | Confirm with IBM Support whether the overrides are still required and track them for future lifecycle operations. | Software Hub Admin | Within two weeks | Review ccs-cr spec for image_digests overrides. |
| SWH-002 | Medium | Pod wkc-search-59bc9fd476-dr256 was terminated after exceeding its ephemeral local storage limit. | Adjust ephemeral storage limits or clean up local temporary data for the search service deployment. | Software Hub Admin | Within two weeks | Ensure search pods run without storage eviction. |
| SWH-003 | Medium | DataStage operator pod exceeded the 90% CPU threshold. | Review operator resource limits and evaluate scaling allocations if high CPU usage persists. | Software Hub Admin | Within two weeks | Monitor DataStage operator CPU consumption. |

# Technical remediation notes

### OCP-001: Node /var disk utilization

Inspect node disk usage and cleanup space:
```bash
oc adm top nodes
oc describe node/aroprplatfudi21-9w577-master-0
oc describe node/aroprplatfudi21-9w577-master-2
```

### OCP-002: Zero disruption headroom on single-replica PodDisruptionBudgets

Inspect PodDisruptionBudgets reporting zero allowed disruptions:
```bash
oc get pdb -A -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,ALLOWED:.status.disruptionsAllowed,DESIRED:.status.desiredHealthy,CURRENT:.status.currentHealthy
```

### OCP-003: Manual subscription approvals

Review subscriptions configured with manual approval:
```bash
oc get subscriptions -A -o jsonpath='{range .items[?(@.spec.installPlanApproval=="Manual")]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
```

### OCP-004: Failed historical CSV cleanup in OLM

List failed and pending CSVs in the operators namespace:
```bash
oc get csv -n $PROJECT_CPD_INST_OPERATORS -o custom-columns=NAME:.metadata.name,PHASE:.status.phase | grep -E "Failed|Pending"
```

### SWH-001: Custom resource image-digest overrides

Review configured image digest overrides on the CCS custom resource:
```bash
oc get ccs ccs-cr -n $PROJECT_CPD_INST_OPERANDS -o jsonpath='{.spec.image_digests}'
```

### SWH-002: Search pod ephemeral storage eviction

Inspect deployment configuration and resource limits for the search service:
```bash
oc describe deployment/wkc-search -n $PROJECT_CPD_INST_OPERANDS
oc get events -n $PROJECT_CPD_INST_OPERANDS --field-selector reason=Evicted
```

### SWH-003: Operator CPU threshold breach

Review DataStage operator resource utilization:
```bash
oc adm top pods -n $PROJECT_CPD_INST_OPERATORS
oc describe deployment/ibm-cpd-datastage-operator -n $PROJECT_CPD_INST_OPERATORS
```

# Appendix: Data collection and limitations

## Collection summary

| Item | Value |
|---|---|
| Collection tool | swh-collect v3.11.0 |
| Collection time | 2026-09-02 10:58:41 |
| Cluster ID | aroprplatfudi21-9w577 |
| Software Hub installed | Yes |
| Network mode | Connected |
| Tools available | cpd_cli, jq, helm, skopeo, yq |

## Incomplete jobs

| Job | Scheduled | Active pods | Failed pods |
|---|---|---|---|
| 713092ef5c4193ba-1000331001-1788350419 | N/A | 1 | 0 |
| catalog-api-import-job-64t9b | N/A | 0 | 1 |
| catalog-api-import-job-6btbc | N/A | 0 | 1 |
| catalog-api-import-job-8ghgb | N/A | 0 | 1 |
| catalog-api-import-job-8xdv9 | N/A | 0 | 1 |
| catalog-api-import-job-l2zsw | N/A | 0 | 1 |
| catalog-api-import-job-xjq66 | N/A | 0 | 1 |
| cpd-im | N/A | 0 | 1 |
| env-spec-sync-job-29763595 | 2026-08-04 | 0 | 1 |
| jdbc-driver-sync-job-29788270 | 2026-08-21 | 0 | 1 |
| policy-api-import-job-ckpkw | N/A | 0 | 1 |
| policy-api-import-job-d5kqd | N/A | 0 | 1 |
| policy-api-import-job-wgzmq | N/A | 0 | 1 |
| policy-api-import-job-z4gnc | N/A | 0 | 1 |
| zen-remote-svc-inst-status-cron-job-29805966 | 2026-09-02 | 1 | 0 |
| zen-storage-perf | N/A | 1 | 0 |

_16 of 130 Jobs have 0 successful completions; 3 still have active pods — pods are being retried, so those failures are live. Scheduled is decoded from the Job-name suffix (the CronJob schedule time)._

## Collection limitations

| Limitation | Impact | Follow-up |
|---|---|---|
| cpd-cli login | Authentication failure during cpd-cli login | Validate platform login credentials and route reachability |
| entitled registry login | Entitled registry authentication failure | Confirm IBM entitlement key validity and registry secret configuration |
| OCP token for storage tests | OpenShift authentication token unavailable for storage testing | Provide valid cluster-admin token when running storage performance benchmarks |
| cpd cli version — failed (query failed) | cpd-cli command-line binary execution failure | Investigate cpd-cli binary installation and environment on bastion |
| RSI patch info — failed (query failed) | RSI patch information retrieval failure | Verify RSI operator status and API availability |
| EDB cnp plugin status — not assessed (tool missing) | EnterpriseDB cnp plugin binary not available on collection host | Install EnterpriseDB kubectl cnp plugin for enhanced database diagnostics |
| cr status — failed (query failed); component CR reconciliation status unavailable | Custom resource status retrieval failure | Check cluster API responsiveness during CR status queries |

## Assumptions

- **Disk utilization trends:** Master node and worker node disk usage figures represent point-in-time measurements; regular journal pruning and container image garbage collection are assumed to keep utilization below critical levels.
- **Historical CSVs:** The 23 failed CSV versions in OLM are assumed to be inactive leftovers from earlier operator version reconciliations rather than impacting current active CSV functionality.
- **PodDisruptionBudgets:** Single-replica PDBs with allowed disruptions at zero are assumed to reflect expected single-instance database topologies rather than unexpected pod downtime.
- **Snapshot point-in-time:** Diagnostics represent a single point-in-time snapshot of cluster metrics; transient threshold breaches should be corroborated with historical observability data.
