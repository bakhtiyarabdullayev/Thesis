# MSc thesis roadmap

Machine Learning-Based Duplicate Detection in Crowdsourced Urban Issue Reports

Review date: 5 November 2026  
Submission deadline in the thesis specification: 15 May 2027

## Current position

At the time of review, the repository contains the thesis specification. It does not yet contain a dataset, implementation, literature review, experiments, or thesis draft. The plan below is a proposed research workflow for discussion with the supervisor.

## Research question and contribution

How much do geographic distance and temporal information improve duplicate urban issue detection compared with text similarity alone?

The intended contribution is a documented dataset and reproducible evaluation of text, location, and time signals, supported by error analysis and a prototype. A novel neural architecture is not necessary for this scope. Literature review should establish how this work relates to existing approaches before any novelty claims are made.

Define duplicates as reports referring to the same underlying incident. Similar descriptions, the same category, or the same location do not by themselves establish duplication. Recurring problems after an earlier issue was resolved may be separate incidents.

## First milestone: proposal and dataset feasibility

Prepare a short proposal covering:

- The main research question, scope, and intended contribution.
- One city, one language, and a manageable set of issue categories.
- Candidate data sources and their access, licensing, privacy, and reproducibility constraints.
- Availability and quality of descriptions, coordinates, timestamps, categories, and duplicate links.
- A clear labelling protocol with positive, negative, and uncertain examples.
- Proposed methods, evaluation splits, and metrics.

Candidate sources to audit:

- [NYC311 service requests](https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2020-to-Present/erm2-nwe9).
- [Helsinki Open311 API documentation](https://www.hel.fi/static/avoindata/kanslia/Open311_API_description_040225.pdf).

These are candidates, not confirmed suitable datasets. Verify meaningful text availability and whether duplicate labels link to an original report. A duplicate status without the related report identifier may be insufficient for pair labels.

Start with a few hundred manually reviewed candidate pairs to establish feasibility. Include difficult negatives, such as distinct incidents nearby with similar descriptions. Independently double-label a subset, measure agreement, and resolve disagreements. Treat uncertain cases explicitly. Decide the final annotation size based on observed class balance and evaluation needs.

Do not label nearby reports as duplicates merely because they satisfy the same spatial and temporal rules that the model will later use.

## Methods

1. Establish simple geographic and temporal rule baselines.
2. Implement TF-IDF text similarity.
3. Evaluate a suitable pretrained sentence embedding model.
4. Train a modest combined classifier using text similarity, geographic distance, and time difference. Category compatibility can be an optional additional feature.
5. Compare text alone, text plus location, text plus time, and all three.

Choose thresholds and hyperparameters using training and validation data only. Document preprocessing, model versions, random seeds, configurations, and computational requirements.

Candidate retrieval should search earlier reports efficiently before classifying or ranking them. Measure candidate recall separately: the final classifier cannot recover true duplicates removed during retrieval.

## Evaluation design

- Keep reports from the same incident together in data splits; do not randomly split overlapping pairs across training and testing.
- Include a chronological evaluation that represents a new report arriving after existing reports. Document how incidents crossing time boundaries are handled.
- Use only information available when a report is submitted. Exclude closure notes and later administrative updates from predictive inputs.
- Report precision, recall, and F1 at a validation-selected threshold. Report PR-AUC where appropriate for class imbalance.
- For ranked suggestions, report recall@5 and describe which queries are eligible for that metric.
- Explain pair sampling and class prevalence. A balanced pair benchmark does not necessarily represent operational precision.
- Report uncertainty where feasible, accounting for dependence among pairs from the same incident.
- Analyse false positives and false negatives, including repeated incidents, nearby separate issues, sparse descriptions, missing coordinates, and misleading timestamps.
- Measure retrieval and inference latency on documented hardware.

## Prototype

Build a small application in which a user enters a description, location, and report time. Display ranked possible duplicate reports with relevant text, location, and time context. Allow the user to inspect suggestions and continue submitting a distinct report.

Demonstrate the workflow using the selected data and trained method. Avoid expanding into a full municipal reporting platform. Model scores should not be described as calibrated probabilities unless calibration has been evaluated.

## Suggested repository structure

- README.md: project purpose, setup, and reproducible commands.
- docs/: proposal, annotation guide, dataset audit, and experiment protocol.
- data/README.md: sources, licensing, schema, and acquisition instructions.
- src/: preprocessing, retrieval, features, training, and inference.
- configs/: versioned experiment configurations.
- notebooks/: exploratory analysis.
- results/: metrics, figures, and experiment summaries.
- app/: prototype.
- thesis/: chapter drafts and bibliography.
- tests/: checks for important data processing and split invariants.

Do not commit restricted source data or personal information. Keep large data and model files outside Git where appropriate, with documented acquisition or reproduction instructions.

## Proposed schedule

| Period | Deliverables |
| --- | --- |
| November to December 2026 | Literature review, proposal, dataset feasibility audit, annotation guide, pilot labels |
| January to February 2027 | Prepared dataset, baseline experiments, combined model |
| March 2027 | Final evaluation, feature ablations, error analysis |
| April 2027 | Prototype, complete thesis draft, supervisor review |
| 1 to 14 May 2027 | Revisions, reproducibility check, formatting and submission preparation |
| 15 May 2027 | Submission deadline stated in the specification |

Write the thesis throughout the project rather than waiting until experiments are finished.

## Proposed thesis outline

1. Introduction, motivation, research questions, and contribution.
2. Related work on duplicate detection and urban reporting.
3. Dataset, annotation, preprocessing, and exploratory analysis.
4. Candidate retrieval, baseline methods, and combined model.
5. Experimental design, results, ablations, and error analysis.
6. Prototype implementation and demonstration.
7. Conclusions, limitations, and future work.

## Immediate next steps

1. Prepare a two-page proposal.
2. Audit candidate datasets and inspect actual records.
3. Present labelled examples showing duplicates, difficult negatives, and uncertain cases.
4. Agree on the dataset, annotation protocol, and evaluation design with the supervisor.
5. Implement the first reproducible baseline and begin the related-work chapter.
