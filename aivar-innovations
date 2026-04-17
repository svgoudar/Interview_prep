## Questions 

- A banking client must explain every credit decision to regulators. Compare SHAP and LIME. Which would you recommend here and why
- How do SHAP’s global insights help regulators understand systemic patterns, not just individual decisions?
- What would happen if a model had complex non-linear interactions? How might LIME or SHAP handle those differently for regulatory review?

### Transcribed Problem (from the image)


You are asked to implement the following function:

```python
import re
from typing import List, Tuple


def tokenize(text: str) -> List[str]:
    return text.lower().split()


def lcs_length(x: List[str], y: List[str]) -> int:
    m, n = len(x), len(y)
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(m):
        for j in range(n):
            if x[i] == y[j]:
                dp[i + 1][j + 1] = dp[i][j] + 1
            else:
                dp[i + 1][j + 1] = max(dp[i][j + 1], dp[i + 1][j])

    return dp[m][n]


def rouge_l_f1(reference: str, generated: str) -> Tuple[float, dict]:
    ref_tokens = tokenize(reference)
    gen_tokens = tokenize(generated)

    lcs = lcs_length(ref_tokens, gen_tokens)

    if not ref_tokens or not gen_tokens:
        return 0.0, {}

    precision = lcs / len(gen_tokens)
    recall = lcs / len(ref_tokens)

    if precision + recall == 0:
        f1 = 0.0
    else:
        f1 = 2 * precision * recall / (precision + recall)

    details = {
        "precision": precision,
        "recall": recall,
        "lcs_length": lcs,
        "ref_len": len(ref_tokens),
        "gen_len": len(gen_tokens)
    }

    return f1, details


def extract_entities(text: str) -> List[str]:
    # Match:
    # - Capitalized sequences (New York, John Doe)
    # - Acronyms (FDA, HIPAA)
    pattern = r'\b(?:[A-Z][a-z]+(?:\s+[A-Z][a-z]+)*|[A-Z]{2,})\b'
    return list(set(re.findall(pattern, text)))


def compute_entity_coverage(ref_entities: List[str], generated: str) -> Tuple[float, List[str]]:
    gen_lower = generated.lower()

    matched = []
    missing = []

    for ent in ref_entities:
        if ent.lower() in gen_lower:
            matched.append(ent)
        else:
            missing.append(ent)

    coverage = len(matched) / len(ref_entities) if ref_entities else 1.0
    return coverage, missing


def evaluate_summary(reference: str,
                     generated: str,
                     rouge_threshold: float = 0.4,
                     entity_threshold: float = 0.8) -> dict:

    rouge_score, rouge_details = rouge_l_f1(reference, generated)

    ref_entities = extract_entities(reference)
    entity_coverage, missing_entities = compute_entity_coverage(ref_entities, generated)

    result = {
        "rouge_l": round(rouge_score, 4),
        "entity_coverage": round(entity_coverage, 4),
        "missing_entities": missing_entities,
        "pass": (rouge_score >= rouge_threshold) and (entity_coverage >= entity_threshold),
        "details": {
            **rouge_details,
            "ref_entities": ref_entities,
            "num_missing_entities": len(missing_entities)
        }
    }

    return result
    ...
```

---

### The function should compute:

#### 1. ROUGE-L F1 Score

* Compute **ROUGE-L F1** between the `reference` and `generated` summaries.
* Use **LCS (Longest Common Subsequence)**.
* Tokenization:

  * Split on **whitespace**
  * Convert to **lowercase**

---

#### 2. Factual Consistency Check (Entity Coverage)

* Extract **“named entities”** from the **reference text**
* Do **NOT** use spaCy/NLTK (no heavy NLP libraries)

Use lightweight heuristics such as:

* Sequences of **capitalized words**

  * Example: `"New York"`, `"John Doe"`
* Acronyms:

  * Example: `"FDA"`, `"HIPAA"`

---

#### Compute Entity Coverage:

```text
entity_coverage = fraction of extracted entities that appear in the generated summary
```

* Matching can be **case-insensitive substring match**
* Example:

  * Reference entities: `["FDA", "New York"]`
  * Generated contains `"FDA"` → counted as match

---

### 3. Return a Structured Dictionary

The function should return at least:

```python
{
    "rouge_l": float (0 to 1),
    "entity_coverage": float (0 to 1),
    "missing_entities": List[str],
    "pass": bool,
    "details": {
        "precision": float,
        "recall": float,
        "lcs_length": int,
        "token_counts": ...
    }
}
```

---

### Pass Criteria

```python
pass = (rouge_l >= rouge_threshold) AND (entity_coverage >= entity_threshold)
```

---

### Additional Notes

* `missing_entities` → entities from reference NOT found in generated
* `details` should include intermediate metrics such as:

  * precision
  * recall
  * LCS length
  * token counts

---


**Senior AI Engineer - Agentic AI & LLM Reasoning**
**Question 3 of 3**

**Design a HIPAA-compliant CI/CD and MLOps pipeline for a patient risk stratification model. Review the details, use the notepad for your ideas, and ask any clarifying questions.**

#### Scenario

Aivar Innovations is building an AI-first delivery platform for a healthcare client. The client uses EHR data to run a **patient risk stratification model** in production (used by care teams and downstream workflows). The system must comply with **HIPAA**, support **monthly retraining** on fresh EHR data, and provide **end-to-end auditability**.

#### What you need to design

Design the full **CI/CD + MLOps pipeline** from **data ingestion** to **model deployment and monitoring**.

#### Key requirements & constraints

**HIPAA compliance**

* PHI must be protected at rest and in transit.
* Strict access control, least privilege, and auditable access logs.
* Define how you handle **de-identification vs limited datasets** (if applicable) and where PHI is permitted to exist.

**Auditability / governance**

* Be able to answer: **“Which data, code, features, parameters and approvals produced model version X?”**
* Maintain lineage across **datasets → features → training runs → model artifacts → deployments**.
* Keep an immutable audit trail for training and for per-prediction events (as appropriate).

**Monthly retraining**

* New EHR data arrives continuously; model is retrained monthly.
* Training should be reproducible and automated, but gated by validation and approvals.

**Production deployment**

* Must support safe rollout strategies (**shadow/canary**) and quick rollback.
* Integrates with enterprise systems (e.g. document repositories/knowledge bases, EHR APIs, internal email/alerts) where relevant.

**Monitoring & drift**

* Monitor data quality, drift, and model performance.
* Define alert thresholds, investigation workflow, and rollback criteria.

#### Assumptions you may make (state them clearly)

* Model type can be tabular (e.g. gradient boosting) or LLM-assisted features, but the pipeline must handle regulated healthcare constraints.
* The service may operate at hospital or multi-hospital scale.
* You can pick a cloud or hybrid environment; describe how your design changes if training and serving environments differ.

#### Deliverable

A clear **architecture and operating model** covering:

* components
* data flow
* control points (validation/approval, security/audit, failure/rollback handling)

---

## Clarifying Questions You Can Ask

1. Is the primary model a **tabular risk model** or does it also use **LLM-generated clinical features/summaries**?
2. Is inference **real-time**, **batch**, or both?
3. Are training and inference allowed in the **same cloud/VPC**, or do we need a **hybrid split** because of PHI constraints?
4. Do we need **per-hospital tenant isolation**?
5. Is model explainability required at prediction time for clinicians and auditors?

---

# Interview-Ready Answer / “Code” for the Notepad

You can paste this directly in the notepad.

```text
Assumptions
- Model is primarily tabular (e.g. XGBoost/LightGBM) using EHR-derived features.
- Inference is near-real-time for care workflows and batch scoring for downstream population health jobs.
- PHI is allowed only inside a restricted HIPAA boundary.
- Training occurs monthly on curated snapshots; serving is containerized and deployed separately.
- Explainability is required for audit and clinician review.

1. High-level architecture

EHR / FHIR / HL7 feeds
    -> secure ingestion
    -> raw PHI landing zone
    -> validation + de-identification / limited dataset processing
    -> curated feature pipelines
    -> offline feature store + training datasets
    -> monthly training pipeline
    -> evaluation + bias + drift baseline + explainability artifacts
    -> model registry + approval workflow
    -> deployment pipeline
    -> online serving / batch scoring
    -> monitoring, audit, lineage, rollback

2. Security and HIPAA boundary

PHI zones
- Raw PHI zone: encrypted object store / database, private subnet only.
- Curated restricted zone: minimal necessary PHI, tightly controlled.
- De-identified / limited dataset zone: used for most training and analytics where possible.

Controls
- Encryption at rest via KMS-managed keys.
- TLS in transit everywhere.
- IAM/RBAC with least privilege.
- Separate roles for data engineers, ML engineers, reviewers, and runtime services.
- Secrets in Secrets Manager / Vault.
- Private networking, VPC endpoints, no public data-plane exposure.
- Immutable audit logs for access, pipeline runs, model approvals, and predictions.

3. Data ingestion and data quality

Ingestion
- EHR data enters through FHIR/HL7 connectors or batch drops.
- Land raw data in immutable object storage partitioned by source/date.
- Register metadata in catalog.
- Trigger validation pipeline via events.

Validation
- Schema checks
- freshness checks
- null/range checks
- code-set validation (ICD/CPT/LOINC where relevant)
- duplicate checks
- patient ID consistency checks

Bad data handling
- quarantine bucket/table
- alert data steward
- no downstream promotion until validated

4. De-identification / limited dataset policy

Default principle: use minimum necessary data.

For training
- Prefer de-identified or limited dataset form.
- Replace direct identifiers with surrogate patient keys.
- Keep re-identification map in a separate highly restricted service if required by policy.
- Feature engineering occurs inside HIPAA boundary; only approved derived features move forward.

For inference
- Runtime may access PHI because predictions are tied to a patient workflow.
- Prediction service stores only minimal required fields in logs; sensitive payloads are redacted/tokenized where possible.

5. Feature store and lineage

Offline feature store
- Versioned feature tables tied to dataset snapshot IDs.
- Includes feature definitions, owners, lineage, and quality checks.

Online feature store
- Only features required for low-latency inference.
- Point-in-time correctness enforced.

Lineage to capture
dataset version -> feature version -> code commit -> training run -> parameters -> model artifact -> approval ticket -> deployment version

6. Monthly retraining pipeline

Schedule
- Monthly orchestration using Airflow / Step Functions / Kubeflow.

Steps
1. Select approved monthly dataset snapshot.
2. Rebuild features reproducibly.
3. Run data validation and drift comparison against prior month.
4. Train candidate model.
5. Evaluate on holdout / temporal validation set.
6. Generate explainability and fairness reports.
7. Compare against current production champion.
8. Register candidate model if thresholds pass.
9. Require manual approvals from model owner + compliance/risk reviewer.
10. Promote to staging, then controlled production rollout.

Reproducibility
- Pin container image, package versions, seeds, feature definitions, and dataset snapshot.
- Every run linked to Git SHA and config version.

7. CI pipeline (code + ML validation)

On every merge
- linting / unit tests
- integration tests
- IaC validation
- security scanning (SAST, dependency scan, container scan)
- feature pipeline tests
- training pipeline smoke tests on synthetic/de-identified sample data
- policy-as-code checks for HIPAA guardrails

Artifacts produced
- signed container images
- pipeline package
- model training spec
- infrastructure manifests

8. CD / model promotion pipeline

Environments
dev -> validation/staging -> prod

Promotion gates
- technical validation passed
- data quality passed
- model metrics above thresholds
- fairness / calibration reviewed
- explainability artifact generated
- security/compliance approval
- business owner approval if required

Deployment strategy
- shadow deployment first
- then canary rollout (5% -> 25% -> 50% -> 100%)
- automatic rollback if latency/errors/performance degrade

9. Model registry and governance

Registry stores
- model artifact URI
- training dataset snapshot
- feature version
- metrics
- hyperparameters
- explainability report
- approval records
- deployment history
- expiry / review date

This allows answering:
“Which data, code, features, params and approvals produced model version X?”

10. Serving architecture

Real-time serving
- Containerized inference API behind internal load balancer.
- Pull approved model from registry.
- Fetch online features.
- Return risk score + reason codes / SHAP explanation if needed.

Batch serving
- Scheduled scoring jobs for population-level workflows.
- Outputs written to secure downstream stores / care management systems.

Operational controls
- idempotent request handling
- request tracing
- model version included in every response/log
- circuit breaker / fallback to previous stable model

11. Monitoring and drift

System monitoring
- latency, throughput, error rate, CPU/memory, queue depth

Data monitoring
- schema drift
- missingness
- distribution drift (PSI / KS)
- source freshness SLA

Model monitoring
- score distribution drift
- calibration drift
- performance drift when labels become available
- subgroup/fairness monitoring across hospital/site/demographic slices

Alerts
- Sev1: inference failures / PHI policy violation / audit log failure
- Sev2: major drift / degraded latency / large missingness spike
- Sev3: mild drift / retraining reminder / delayed labels

Investigation workflow
- alert -> ticket -> on-call triage
- inspect data snapshot, feature stats, model version, deployment diff
- if production harm risk: freeze rollout or rollback immediately
- document incident and CAPA

12. Rollback strategy

Rollback triggers
- canary metrics degrade beyond threshold
- error rate spike
- unexpected prediction distribution shift
- compliance control failure
- severe drift after deployment

Rollback actions
- traffic back to prior champion model
- mark candidate as rejected in registry
- preserve incident trail and artifacts
- open retraining/investigation task

13. Recommended stack example (AWS, but cloud-agnostic pattern)

- Storage: S3 + KMS
- Catalog: Glue Data Catalog
- Ingestion: API Gateway / Lambda / Kafka / Kinesis
- Orchestration: Step Functions or Airflow
- Processing: EMR / Spark / ECS / EKS
- Feature store: SageMaker Feature Store / Feast
- Experiment tracking + registry: MLflow
- Training: SageMaker / Kubernetes jobs
- Serving: ECS/EKS/SageMaker endpoint
- Monitoring: CloudWatch + Prometheus + Grafana
- Audit: CloudTrail + immutable log archive
- Secrets: Secrets Manager
- IaC: Terraform

14. If training and serving are split environments

Case: training in central cloud, serving inside hospital VPC/on-prem
- Keep PHI-local feature generation near hospital boundary.
- Transfer only approved model artifacts and non-sensitive metadata across boundary.
- Use signed artifacts and attestation.
- Sync registry metadata centrally, but keep patient-level inference logs local or replicated via approved secure pipeline.
- Separate KMS domains and trust boundaries.

15. Final summary

This design uses a secure HIPAA boundary, minimum-necessary PHI handling, reproducible monthly retraining, strong lineage from data to deployment, gated model promotion, safe canary rollout, continuous monitoring, and immutable audit trails. It supports both compliance and operational reliability for a patient risk stratification system.
```

---

## Very Short Spoken Version

“ I would design three zones: raw PHI, curated restricted, and de-identified training data. EHR data lands in an encrypted private ingestion layer, passes data quality and de-identification controls, and feeds a versioned feature store. Monthly retraining is orchestrated reproducibly with dataset snapshots, feature versions, Git SHA, parameters, and metrics tracked in MLflow or a registry. Promotion is gated by validation, fairness, explainability, compliance, and approval workflows. Production rollout uses shadow or canary deployment with fast rollback. Monitoring covers data quality, drift, latency, and post-deployment performance, while immutable audit logs and lineage answer exactly which data, code, features, and approvals produced any model version.”

If you want, I can also turn this into a **diagram-style architecture answer** or a **2-minute interview script**.
