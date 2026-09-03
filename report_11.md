# Executive summary

| Overall status | Operational risk | Immediate action required |
|---|---|---|
| Yellow | Moderate | Review single-replica PDB headroom and maintenance flags |

**Status: Yellow — the cluster and core platform are operational, with attention required on single-replica PodDisruptionBudgets, maintenance flags, and pending sync job secrets.**

* All 10 cluster and node infrastructure health checks passed.
* All 17 operator CSVs Succeeded.
* Red Hat cert-manager is active and certificate renewals are valid.
* 13 of 22 detected service CRs Completed, with 11 custom resources currently in maintenance mode.

The underlying OpenShift infrastructure and foundational database services are running stably. Attention is required to account for 17 single-replica PodDisruptionBudgets blocking voluntary evictions (OCP-001), verify custom resources flagged for maintenance (SWH-001), configure image pull secrets for pending synchronization jobs (SWH-005), and address local ephemeral storage limits for the FoundationDB controller (SWH-004).

**Service impact (cpd-cli monitor events):** no service or service-instance critical events this collection.

## Top risks

| Priority | Risk | Impact | Required action | Finding |
|---|---|---|---|---|
| 1 | 17 single-replica PodDisruptionBudgets have allowedDisruptions=0 | Voluntary node drains or evictions during maintenance will be blocked | Plan node maintenance around single-replica workloads | OCP-001 |

# Environment profile

## Cluster overview

| Item | Value |
|---|---|
| Cluster name | arodvplatfudi11-9l5wz |
| OpenShift version | 4.18.19 |
| IBM Software Hub release | 5.3.1 |
| cpd_platform (ibmcpd) | v5.3 |
| Operators namespace | gov-operators |
| Operands namespace | gov-operands |
| FIPS mode | Disabled |
| Network mode | Connected |
| Software Hub installed | Yes |
| Storage provider | OpenShift Data Foundation (Ceph 18.2.1-340) |
| Storage classes | azure-file, ocs-storagecluster-ceph-rbd, ocs-storagecluster-cephfs |
| IAM integration | Enabled |
| CPD route | cpd-gov-operands.apps.arodvplatfudi11.arocorpp.bradesco.com.br |
| Image mirror ICSP | image-policy-0, image-policy-1, image-policy-2, quay-as-proxy |
| PodDisruptionBudgets | 47 |
| Helm releases | 64 (62 deployed) |

## Version

| Component | Observed version | Interpretation |
|---|---|---|
| OpenShift Container Platform | 4.18.19 | Release 4.18.19 deployed on Azure Red Hat OpenShift. |
| IBM Software Hub | 5.3.1 | Release 5.3.1 deployed and operational. |

## Node inventory

| Node | Role | vCPU | Memory | Age |
|---|---|---|---|---|
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth1-7xgjh | worker | 32 | 125.8 GiB | 127d |
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth2-cztz6 | worker | 32 | 125.8 GiB | 127d |
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth3-l6w8s | worker | 32 | 125.8 GiB | 127d |
| arodvplatfudi11-9l5wz-infra-brazilsouth1-bmd9q | worker | 8 | 62.8 GiB | 571d |
| arodvplatfudi11-9l5wz-infra-brazilsouth2-zb9dp | worker | 8 | 62.8 GiB | 483d |
| arodvplatfudi11-9l5wz-infra-brazilsouth3-p5tsc | worker | 8 | 62.8 GiB | 571d |
| arodvplatfudi11-9l5wz-master-0 | master | 16 | 62.8 GiB | 2y41d |
| arodvplatfudi11-9l5wz-master-1 | master | 16 | 62.8 GiB | 2y41d |
| arodvplatfudi11-9l5wz-master-2 | master | 16 | 62.8 GiB | 2y41d |
| arodvplatfudi11-9l5wz-odf-worker-brazilsouth1-v9msl | worker | 16 | 62.8 GiB | 678d |
| arodvplatfudi11-9l5wz-odf-worker-brazilsouth2-wk88m | worker | 16 | 62.8 GiB | 678d |
| arodvplatfudi11-9l5wz-odf-worker-brazilsouth3-vf8qk | worker | 16 | 62.8 GiB | 672d |
| arodvplatfudi11-9l5wz-platf-worker-brazilsouth1-4fdr8 | worker | 32 | 125.8 GiB | 127d |
| arodvplatfudi11-9l5wz-platf-worker-brazilsouth2-2nmt5 | worker | 32 | 125.8 GiB | 127d |
| arodvplatfudi11-9l5wz-platf-worker-brazilsouth3-k29xf | worker | 32 | 125.8 GiB | 127d |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-8c7l6 | worker | 16 | 62.8 GiB | 120d |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-fsk6g | worker | 16 | 62.8 GiB | 431d |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-g6fbz | worker | 16 | 62.8 GiB | 109d |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-q5xhz | worker | 16 | 62.8 GiB | 387d |
| arodvplatfudi11-9l5wz-worker-brazilsouth2-2qvcw | worker | 16 | 62.8 GiB | 398d |
| arodvplatfudi11-9l5wz-worker-brazilsouth2-5gvqk | worker | 16 | 62.8 GiB | 9d |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-9sskw | worker | 16 | 62.8 GiB | 110d |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-gsjk8 | worker | 16 | 62.8 GiB | 431d |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-mpcbp | worker | 16 | 62.8 GiB | 96d |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-tgcm4 | worker | 16 | 62.8 GiB | 431d |

_Totals: 25 nodes, 472 vCPU, 1948 GiB memory._

## Installed services and instances

| Instance | Type | Version | Status | Notes |
|---|---|---|---|---|
| ProfHbIntrnl | spark | 5.3.4 | PROVISIONED | Internal Apache Spark instance supporting profiling workloads. |
| ds-px-default | datastage | 5.3.5 | PROVISIONED | Default DataStage parallel execution engine instance. |

_5 internal zen-secrets/volumes instances (PROVISIONED) omitted._

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

Status: Yellow — cluster infrastructure and core operators are healthy, with disruption risks on single-replica PodDisruptionBudgets.

All OpenShift control plane components, machine config pools, and cluster operators are available and functional. Node resources and disk capacities remain within operational bounds across all nodes, while 17 PodDisruptionBudgets currently allow zero disruptions.

## Cluster health

_All 10 checks passed._

## Node resource usage

| Node | CPU usage | CPU % | Memory used | Memory % |
|---|---|---|---|---|
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth1-7xgjh | 1.6 CPU | 4.9% | 44.4 GiB | 35.3% |
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth2-cztz6 | 1.6 CPU | 5.1% | 50.7 GiB | 40.3% |
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth3-l6w8s | 1 CPU | 3% | 45.9 GiB | 36.5% |
| arodvplatfudi11-9l5wz-infra-brazilsouth1-bmd9q | 1.3 CPU | 15.9% | 27 GiB | 43% |
| arodvplatfudi11-9l5wz-infra-brazilsouth2-zb9dp | 1.2 CPU | 14.4% | 26.7 GiB | 42.5% |
| arodvplatfudi11-9l5wz-infra-brazilsouth3-p5tsc | 0.5 CPU | 6.1% | 16.9 GiB | 26.9% |
| arodvplatfudi11-9l5wz-master-0 | 3.5 CPU | 21.7% | 32.9 GiB | 52.4% |
| arodvplatfudi11-9l5wz-master-1 | 2.4 CPU | 14.9% | 27.3 GiB | 43.5% |
| arodvplatfudi11-9l5wz-master-2 | 1.9 CPU | 11.7% | 43 GiB | 68.4% |
| arodvplatfudi11-9l5wz-odf-worker-brazilsouth1-v9msl | 0.4 CPU | 2.4% | 13.1 GiB | 20.8% |
| arodvplatfudi11-9l5wz-odf-worker-brazilsouth2-wk88m | 0.4 CPU | 2.2% | 10.6 GiB | 16.9% |
| arodvplatfudi11-9l5wz-odf-worker-brazilsouth3-vf8qk | 0.4 CPU | 2.8% | 14.9 GiB | 23.7% |
| arodvplatfudi11-9l5wz-platf-worker-brazilsouth1-4fdr8 | 1.1 CPU | 3.5% | 30.2 GiB | 24% |
| arodvplatfudi11-9l5wz-platf-worker-brazilsouth2-2nmt5 | 1.5 CPU | 4.7% | 47 GiB | 37.4% |
| arodvplatfudi11-9l5wz-platf-worker-brazilsouth3-k29xf | 1.1 CPU | 3.3% | 41.9 GiB | 33.3% |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-8c7l6 | 0.8 CPU | 5% | 22.5 GiB | 35.8% |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-fsk6g | 1.1 CPU | 7.1% | 22.3 GiB | 35.5% |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-g6fbz | 0.6 CPU | 3.5% | 20.3 GiB | 32.3% |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-q5xhz | 0.7 CPU | 4.1% | 24.6 GiB | 39.1% |
| arodvplatfudi11-9l5wz-worker-brazilsouth2-2qvcw | 0.9 CPU | 5.9% | 26 GiB | 41.4% |
| arodvplatfudi11-9l5wz-worker-brazilsouth2-5gvqk | 0.4 CPU | 2.6% | 14 GiB | 22.3% |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-9sskw | 0.7 CPU | 4.3% | 27.3 GiB | 43.4% |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-gsjk8 | 0.7 CPU | 4.5% | 25.1 GiB | 40% |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-mpcbp | 0.5 CPU | 3.4% | 23.8 GiB | 37.9% |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-tgcm4 | 0.6 CPU | 4% | 14.8 GiB | 23.6% |

## Disk usage

| Node | Size | Used | Available | Use % | Status |
|---|---|---|---|---|---|
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth1-7xgjh | 512G | 320G | 192G | 63% | OK |
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth2-cztz6 | 512G | 304G | 208G | 60% | OK |
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth3-l6w8s | 512G | 324G | 189G | 64% | OK |
| arodvplatfudi11-9l5wz-infra-brazilsouth1-bmd9q | 256G | 196G | 61G | 77% | OK |
| arodvplatfudi11-9l5wz-infra-brazilsouth2-zb9dp | 256G | 174G | 82G | 68% | OK |
| arodvplatfudi11-9l5wz-infra-brazilsouth3-p5tsc | 256G | 120G | 137G | 47% | OK |
| arodvplatfudi11-9l5wz-master-0 | 1.0T | 855G | 170G | 84% | Warn |
| arodvplatfudi11-9l5wz-master-1 | 1.0T | 845G | 180G | 83% | Warn |
| arodvplatfudi11-9l5wz-master-2 | 1.0T | 858G | 166G | 84% | Warn |
| arodvplatfudi11-9l5wz-odf-worker-brazilsouth1-v9msl | 300G | 234G | 66G | 78% | OK |
| arodvplatfudi11-9l5wz-odf-worker-brazilsouth2-wk88m | 300G | 201G | 100G | 67% | OK |
| arodvplatfudi11-9l5wz-odf-worker-brazilsouth3-vf8qk | 300G | 204G | 97G | 68% | OK |
| arodvplatfudi11-9l5wz-platf-worker-brazilsouth1-4fdr8 | 512G | 266G | 246G | 52% | OK |
| arodvplatfudi11-9l5wz-platf-worker-brazilsouth2-2nmt5 | 512G | 286G | 226G | 56% | OK |
| arodvplatfudi11-9l5wz-platf-worker-brazilsouth3-k29xf | 512G | 276G | 237G | 54% | OK |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-8c7l6 | 512G | 312G | 201G | 61% | OK |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-fsk6g | 512G | 323G | 190G | 63% | OK |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-g6fbz | 512G | 317G | 195G | 62% | OK |
| arodvplatfudi11-9l5wz-worker-brazilsouth1-q5xhz | 512G | 308G | 205G | 61% | OK |
| arodvplatfudi11-9l5wz-worker-brazilsouth2-2qvcw | 512G | 325G | 187G | 64% | OK |
| arodvplatfudi11-9l5wz-worker-brazilsouth2-5gvqk | 512G | 113G | 400G | 22% | OK |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-9sskw | 512G | 301G | 211G | 59% | OK |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-gsjk8 | 512G | 325G | 187G | 64% | OK |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-mpcbp | 512G | 302G | 211G | 59% | OK |
| arodvplatfudi11-9l5wz-worker-brazilsouth3-tgcm4 | 512G | 309G | 204G | 61% | OK |

_Filesystem: node `/var` (where container ephemeral storage lives)._

## OLM and operator state

| Check | Result |
|---|---|
| cert-manager-operator.v1.19.1 | Succeeded |
| cluster-logging.v6.4.6 | Succeeded |
| conjur-follower-operator.v2.6.2 | Succeeded |
| custom-metrics-autoscaler.v2.19.0-2 | Succeeded |
| devworkspace-operator.v0.35.1 | Succeeded |
| elasticsearch-operator.v5.8.21 | Succeeded |
| kiali-operator.v2.4.7 | Succeeded |
| openshift-gitops-operator.v1.17.0 | Succeeded |
| rhacs-operator.v4.11.3 | Succeeded |
| rhods-operator.2.25.10 | Succeeded |
| servicemeshoperator.v2.6.8 | Succeeded |
| web-terminal.v1.9.0-0.1732652688.p | Succeeded |
| Manual subscriptions | 7 found |

_All 12 operator CSVs Succeeded._

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

_17 of 47 PodDisruptionBudgets currently allow zero disruptions, so a voluntary eviction such as a node drain is blocked or will violate the budget; the rest have disruption headroom and are not listed. Single replica means the budget guards a workload with no second instance._

## Certificates

| Check | Result | Interpretation |
|---|---|---|
| Cert-manager controller | redhat active (determination: redhat) | Red Hat cert-manager active and managing cluster certificates. |
| Certificates Healthcheck | PASS | All evaluated certificates valid with renewal and expiry within expected ranges. |

# IBM Software Hub assessment

Status: Yellow — core platform services are operational, with attention required on custom resources in maintenance mode, operator resource thresholds, and pending job secrets.

Foundational services and databases are healthy and running. Several custom resources have reconciliation paused under maintenance flags, and specific operator pods experienced resource limits or pending image secrets.

## Service CR status

| Kind | Name | Version | Status | Has hotfix | Notes |
|---|---|---|---|---|---|
| wkcgovui | wkc-gov-ui-cr | 5.3.15 | InMaintenance | No | Maintenance mode enabled; reconciliation paused (SWH-001). |
| glossary | glossary-cr | 5.3.15 | InMaintenance | No | Maintenance mode enabled; reconciliation paused (SWH-001). |
| workflow | workflow-cr | 5.3.15 | InMaintenance | No | Maintenance mode enabled; reconciliation paused (SWH-001). |
| wkc | wkc-cr | 5.3.15 | InMaintenance | No | Maintenance mode enabled; reconciliation paused (SWH-001). |
| dataquality | data-quality-cr | 5.3.15 | InMaintenance | No | Maintenance mode enabled; reconciliation paused (SWH-001). |
| profiling | profiling-cr | 5.3.15 | InMaintenance | No | Maintenance mode enabled; reconciliation paused (SWH-001). |
| policy | policy-cr | 5.3.15 | InMaintenance | No | Maintenance mode enabled; reconciliation paused (SWH-001). |
| metadataimports | metadataimports-cr | 5.3.15 | InMaintenance | No | Maintenance mode enabled; reconciliation paused (SWH-001). |
| enrichment | enrichment-cr | 5.3.15 | InMaintenance | No | Maintenance mode enabled; reconciliation paused (SWH-001). |
| ccs | ccs-cr | 12.1.6 | Completed | Yes (3 digest overrides) | Carries 3 image-digest overrides (SWH-002). |

_13 of 22 detected service CRs Completed. 10 are shown because each is not Completed or carries a hotfix signal; 12 Completed CRs with no hotfix signal are omitted. The Version column is each CR's own operand version, distinct from the 5.3.1 platform release._

## Control plane health

| Check | Result | Interpretation |
|---|---|---|
| CPFS overall status |  | Foundational services healthy and reconciled. |
| Catalog Source Healthcheck | Skipped | Catalog source readiness check skipped during collection. |
| Install Plan Healthcheck | Skipped | Install plan approval policy check skipped during collection. |
| Subscriptions Healthcheck | Skipped | Subscription CSV status check skipped during collection. |
| Monitor Events — pvc (4) | FAIL | Critical PVC events reported for unassociated storage volumes (SWH-003). |
| Pod Healthcheck (ibm-fdb-controller-manager-78c5ff86bf-2jwdg) (2) | FAIL | Ephemeral storage limit exceeded resulting in pod termination (SWH-004). |
| Pod Healthcheck (ibm-fdb-controller-manager-78c5ff86bf-5p6jk) (2) | FAIL | Ephemeral storage limit exceeded resulting in pod termination (SWH-004). |
| Pod Healthcheck (ibm-fdb-controller-manager-78c5ff86bf-vbcmp) (2) | FAIL | Ephemeral storage limit exceeded resulting in pod termination (SWH-004). |
| Pod Healthcheck - Pending (env-spec-sync-job-29805945-f767f) | FAIL | Pending job pod waiting for image pull secret (SWH-005). |
| Pod Healthcheck - Pending (jdbc-driver-sync-job-29805945-xbrw5) | FAIL | Pending job pod waiting for image pull secret (SWH-005). |
| Stateful Set Healthcheck | Skipped | StatefulSet replica check skipped during collection. |
| Custom Resource Healthcheck | Skipped | Operator custom resource state check skipped during collection. |
| Custom Resource Healthcheck | FAIL | Operand custom resource in maintenance mode (SWH-001). |

_12 of 30 checks passed and are not listed individually. The 18 failed or skipped checks above are grouped by root cause into 12 rows. CPFS overall status above is the foundational-services rollup, reported separately and not one of these checks._

## Operator resource thresholds

| Pod | Issue | Value | Limit | Usage |
|---|---|---|---|---|
| ibm-cpd-datastage-operator-647bc9f4f8-xkqj5 | CPU usage exceeded 90% threshold | N/A | N/A | N/A |
| ibm-iam-operator-5f4c4d9d87-8etvn | CPU usage exceeded 90% threshold | N/A | N/A | N/A |
| zen-minio-1 | Memory usage exceeded 90% threshold | N/A | N/A | N/A |

_Value/Limit/Usage: cpd-cli reports a 90% threshold breach, not millicores (see data gaps)._

# Storage and network assessment

Status: Green — storage backend and network throughput meet requirements.

The OpenShift Data Foundation storage cluster is in a Ready state with Ceph healthy. Network throughput between tested worker nodes exceeds the minimum configured bandwidth threshold.

## Storage validation and backend

| Check | Result | Follow-up |
|---|---|---|
| ODF / Ceph backend | storagecluster Ready, ceph None, raw used None% | StorageCluster in Ready phase; backend storage operational. |
| Volume snapshot classes | 5 present | VolumeSnapshotClasses present and configured. |

## Network performance

| Node | Min throughput | Threshold | Status |
|---|---|---|---|
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth1-7xgjh | 1124 MB/s | 350 MB/s | PASS |
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth2-cztz6 | 1124 MB/s | 350 MB/s | PASS |
| arodvplatfudi11-9l5wz-avim-worker-brazilsouth3-l6w8s | 1159 MB/s | 350 MB/s | PASS |

_Tested 3 of 25 nodes (the configured perf-node subset); min throughput is each node's slowest pair, either direction._

# Upgrade considerations

This assessment is a routine operational health check for release 5.3.1. No upgrade target was evaluated during this collection.

Before planning any future upgrade, review the findings register regarding custom resources currently in maintenance mode (SWH-001), single-replica PodDisruptionBudgets (OCP-001), image-digest overrides on custom resources (SWH-002), and operator subscription approval settings (OCP-002).

# Findings register and recommendations

Finding IDs: OCP = OpenShift infrastructure; SWH = Software Hub platform. Severity: Critical / High / Medium / Low.

| ID | Sev. | Finding | Recommendation | Owner | Timeframe | Validation |
|---|---|---|---|---|---|---|
| OCP-001 | High | 17 single-replica PodDisruptionBudgets currently allow zero disruptions, blocking voluntary node drains or evictions. | Review single-replica PDBs during node maintenance and ensure high-availability replicas where required. | Cluster Admin | Within one week | Verify allowed disruptions headroom across PDB configurations. |
| OCP-002 | Low | Seven operator subscriptions have installPlanApproval configured as Manual. | Review subscription approval policies according to organizational change management standards. | Cluster Admin | Next maintenance window | Check subscription spec installPlanApproval settings. |
| SWH-001 | Medium | Eleven custom resources have maintenance mode enabled (ignoreForMaintenance=true), placing their status in InMaintenance and pausing operator reconciliation. | Review maintenance flags on affected custom resources and resume reconciliation when maintenance activities conclude. | Software Hub Admin | Within two weeks | Verify custom resource statuses transition to Completed. |
| SWH-002 | Medium | Custom resource ccs-cr contains 3 image-digest overrides, indicating hotfix or specific patch image pins. | Confirm with IBM Support whether the overrides are still required and track them for future lifecycle operations. | Software Hub Admin | Within two weeks | Review ccs-cr spec for image_digests overrides. |
| SWH-003 | Medium | Monitor events report 4 critical volume status events where PersistentVolumeClaims lack associated storage volumes. | Inspect affected PVCs and recreate or bind storage resources as required by the workload. | Software Hub Admin | Within two weeks | Confirm PVCs are Bound and monitor events resolve. |
| SWH-004 | Medium | Controller manager pods for FoundationDB exceeded their ephemeral local storage limits, resulting in pod evictions. | Increase ephemeral storage limits for the FoundationDB controller deployment. | Software Hub Admin | Within two weeks | Ensure new controller pods run without storage eviction. |
| SWH-005 | Medium | Synchronization jobs env-spec-sync-job and jdbc-driver-sync-job remain in Pending status due to missing image pull secret ibm-entitlement-key. | Ensure secret ibm-entitlement-key exists in the operands namespace and is linked to the job service accounts. | Software Hub Admin | Within one week | Confirm sync jobs complete successfully. |
| SWH-006 | Medium | Datastage and IAM operator pods exceeded the 90% CPU threshold, while MinIO exceeded the 90% memory threshold. | Review operator and component resource limits and scale allocations if sustained high utilization is observed. | Software Hub Admin | Within two weeks | Monitor operator pod CPU and memory consumption. |

# Technical remediation notes

### OCP-001: Zero disruption headroom on single-replica PodDisruptionBudgets

Inspect PodDisruptionBudgets reporting zero allowed disruptions:
```bash
oc get pdb -A -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,ALLOWED:.status.disruptionsAllowed,DESIRED:.status.desiredHealthy,CURRENT:.status.currentHealthy
```

### OCP-002: Manual install plan approvals on subscriptions

List subscriptions configured with manual approval policies:
```bash
oc get subscriptions -A -o jsonpath='{range .items[?(@.spec.installPlanApproval=="Manual")]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
```

### SWH-001: Custom resources in maintenance mode

Investigate custom resource status and maintenance annotations:
```bash
oc get ccs,wkc,glossary,workflow,dataquality,profiling,policy,metadataimports,enrichment -n $PROJECT_CPD_INST_OPERANDS
oc get wkc wkc-cr -n $PROJECT_CPD_INST_OPERANDS -o yaml
```

### SWH-002: Custom resource image-digest overrides

Review configured image digest overrides on the CCS custom resource:
```bash
oc get ccs ccs-cr -n $PROJECT_CPD_INST_OPERANDS -o jsonpath='{.spec.image_digests}'
```

### SWH-003: Unbound PVC monitor events

Inspect persistent volume claims in the operands namespace:
```bash
oc get pvc -n $PROJECT_CPD_INST_OPERANDS
oc describe pvc volumes-teste-pvc volumes-teste2-pvc volumes-teste3-pvc volumes-xmlscatalogacaotrans-pvc -n $PROJECT_CPD_INST_OPERANDS
```

### SWH-004: Ephemeral storage evictions on FoundationDB controller

Inspect controller manager pod resource requests and limits:
```bash
oc describe deployment/ibm-fdb-controller-manager -n $PROJECT_CPD_INST_OPERATORS
oc get events -n $PROJECT_CPD_INST_OPERATORS --field-selector reason=Evicted
```

### SWH-005: Missing image pull secret on sync jobs

Check for the existence of the entitlement secret in the operands namespace:
```bash
oc get secret ibm-entitlement-key -n $PROJECT_CPD_INST_OPERANDS
oc get cronjobs -n $PROJECT_CPD_INST_OPERANDS
```

### SWH-006: Operator resource threshold breaches

Review operator resource utilization and pod status:
```bash
oc adm top pods -n $PROJECT_CPD_INST_OPERATORS
oc adm top pods -n $PROJECT_CPD_INST_OPERANDS
```

# Appendix: Data collection and limitations

## Collection summary

| Item | Value |
|---|---|
| Collection tool | swh-collect v3.11.0 |
| Collection time | 2026-09-02 10:37:22 |
| Cluster ID | arodvplatfudi11-9l5wz |
| Software Hub installed | Yes |
| Network mode | Connected |
| Tools available | cpd_cli, jq, helm, skopeo, yq |

## Incomplete jobs

| Job | Scheduled | Active pods | Failed pods |
|---|---|---|---|
| catalog-api-import-job-7gjt7 | N/A | 0 | 1 |
| catalog-api-import-job-87hsw | N/A | 0 | 1 |
| catalog-api-import-job-cbg2w | N/A | 0 | 1 |
| catalog-api-import-job-kwwx8 | N/A | 0 | 1 |
| catalog-api-import-job-nc84d | N/A | 0 | 1 |
| catalog-api-import-job-qk5dt | N/A | 0 | 1 |
| catalog-api-import-job-tnpch | N/A | 0 | 1 |
| cpd-im | N/A | 0 | 1 |
| env-spec-sync-job-29738415 | 2026-07-17 | 0 | 1 |
| env-spec-sync-job-29805945 | 2026-09-02 | 1 | 0 |
| jdbc-driver-sync-job-29734095 | 2026-07-14 | 0 | 1 |
| jdbc-driver-sync-job-29805945 | 2026-09-02 | 1 | 0 |
| jobs-retention-check-cronjob-29805945 | 2026-09-02 | 1 | 0 |
| zen-storage-perf | N/A | 1 | 0 |

_14 of 113 Jobs have 0 successful completions; 4 still have active pods — pods are being retried, so those failures are live. Scheduled is decoded from the Job-name suffix (the CronJob schedule time)._

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

- **Image pull secrets:** Environment synchronization jobs are assumed to require the standard `ibm-entitlement-key` secret in the operands namespace; missing secrets prevent automated synchronization tasks from completing.
- **Maintenance flags:** Custom resources reporting `InMaintenance` are assumed to be intentionally placed under maintenance via `ignoreForMaintenance=true`, rather than experiencing reconciliation failure.
- **PodDisruptionBudgets:** Single-replica PDBs with allowed disruptions at zero are assumed to reflect expected single-instance database topologies rather than unexpected pod downtime.
- **Snapshot point-in-time:** Diagnostics represent a single point-in-time snapshot of cluster metrics; transient threshold breaches should be corroborated with historical observability data.
