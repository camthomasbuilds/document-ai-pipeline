# Document AI Pipeline

## The Business Problem

A Fortune 200 energy company maintained 650,000+ engineering documents (drawings, maintenance records, equipment manuals) in an enterprise content management system. Decades of accumulation left 40%+ of documents with incomplete or incorrect metadata, making them effectively unfindable. Engineers averaged 15-30 minutes searching for a single document, costing an estimated $3.4M/year in lost productivity.

**The strategic question:** When should AI automate, and when should humans decide?

## My Role

Senior Product Manager. Owned product vision, architecture decisions, delivery, and stakeholder alignment across ML engineers, platform developers, and operational SMEs.

## The Key Decisions

### 1. Three specialized AI agents, not one general model

Each document type (drawings, records, manuals) has fundamentally different structures. I chose three specialized extraction agents over a single general-purpose model. Result: 90-94% F1 accuracy vs. ~75% for the generalized approach. A 15%+ improvement that made the difference between a useful product and an expensive experiment.

### 2. Confidence-gated automation

Not every prediction should be trusted equally. I designed a graduated system: high-confidence classifications auto-file (no human touch), medium-confidence routes to review, low-confidence flags for expert review. The thresholds are product decisions I own, adjustable without code deploys, and monitored at the boundaries where errors concentrate.

This pattern was adopted by two additional product lines across the organization.

### 3. Transition from project to product

The initial framing was a one-time backfill of 650K documents. I reframed it as a three-phase product: backfill (process existing corpus), forward-fill (auto-process new documents), and continuous improvement (human corrections feed retraining). This changed the investment profile from a one-time cost to a compounding asset.

### 4. Start with the hardest document type

We started with drawings (38 subtypes, complex title blocks) rather than manuals (simplest category). This front-loaded risk. When we got to records and manuals, the architecture was already battle-tested.

## Results

| Metric | Outcome |
|--------|---------|
| **Accuracy** | 90% F1 (drawings), 93% F1 (manuals), 94% F1 (records) |
| **Scale** | 650K+ documents processed, 10K+ human-reviewed |
| **ROI** | $3.4M annual productivity savings, 6.6x return |
| **Automation** | Pipeline runs fully unattended (EventBridge-triggered) |
| **Traceability** | Every document linked to exact model version that processed it |
| **Governance** | ROT rules block ~90% of redundant/obsolete content pre-extraction |

## What This Demonstrates

- **AI product judgment** (when to automate vs. when to keep humans in the loop)
- **Architecture decisions with organizational leverage** (confidence-gating adopted by 2+ product lines)
- **Business model framing** (project to product transition changed the investment case)
- **Stakeholder alignment** on "good enough" thresholds across finance, operations, and engineering
