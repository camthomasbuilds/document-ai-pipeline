# Case Study: AI-Powered Document Intelligence Platform

## Context

A large energy company maintained 650,000+ engineering documents (drawings, maintenance records, equipment manuals) in an enterprise content management system. Over decades, these documents accumulated with incomplete, incorrect, or missing metadata — making them effectively unfindable.

Knowledge workers spent significant time manually searching for documents, costing an estimated $3.4M/year in lost productivity across the engineering organization.

## My Role

Senior Product Manager — responsible for product vision, roadmap, stakeholder alignment, and delivery across a cross-functional team of ML engineers, platform developers, and operational SMEs.

## Discovery & Problem Framing

**User research findings:**
- Engineers averaged 15–30 minutes searching for a single document
- 40%+ of documents had incomplete classification metadata
- Manual metadata entry was error-prone and inconsistent across sites
- No feedback mechanism existed to improve data quality over time

**Key insight:** This wasn't just a search problem — it was a data quality problem at the source. If we could accurately classify and extract metadata at ingest time, we could eliminate the search problem entirely.

## Product Strategy

I framed the product around three operating modes:

1. **Backfill** — Process the existing 650K document corpus to fill in missing metadata
2. **Forward-fill** — Automatically process new documents as they arrive (no human intervention for high-confidence results)
3. **Human-in-the-loop** — Surface uncertain cases for review, use corrections to improve the model

This framing was critical because it set the expectation that the product would *transition* from a one-time project to a live production system.

## Architecture Decisions I Drove

### Multi-agent vs. single-model approach
- **Decision:** Three specialized LLM extraction agents (one per document type) rather than a single general-purpose model
- **Rationale:** Each document type has fundamentally different field structures. A drawing has title blocks; a manual has cover pages; a record has header stamps. Specialized agents achieved 90–94% F1 vs. ~75% for a generalized approach.

### Confidence-threshold automation
- **Decision:** Documents above a confidence threshold write to the content management system automatically; below-threshold documents queue for human review
- **Rationale:** This creates a self-improving system — as accuracy improves, more documents bypass review, reducing operational cost without sacrificing quality.

### Classification as a two-level taxonomy
- **Decision:** L1 classification (4 disciplines) feeds into L2 classification (39 subtypes) in a hierarchical model
- **Rationale:** L1 accuracy was 97–98% across all categories, so errors at L1 were rare. L2 had more variance (77–100% across subtypes), making it the right place to invest retraining effort.

## Delivery & Results

### Phase 1 (6 months)
- Shipped extraction pipeline processing all three document types
- Launched validation portal with 10,000+ documents reviewed
- Achieved 90% F1 (drawings), 93% F1 (manuals), 94% F1 (records)
- Deployed ML classifiers with MLflow tracking for every model version

### Phase 2 (in progress)
- Automated retraining loops triggered by reviewer corrections
- ROT governance rules blocking ~90% of redundant/obsolete documents pre-extraction
- Self-service document upload portal
- Transition to live forward-fill processing

### Business impact
- $3.4M estimated annual productivity savings
- Processing runs fully unattended (EventBridge-triggered, no manual intervention)
- Every document traceable to the exact model version that processed it

## Lessons Learned

1. **Confidence scores are the product** — The extraction itself is table stakes. The real value is knowing *how confident* the system is, so you can automate the easy cases and focus human attention on the hard ones.

2. **Retraining needs a product design, not just a pipeline** — It's not enough to have a retraining script. You need to design: What triggers retraining? Who approves the new model? How do you roll back? What's the SLA?

3. **Stakeholder alignment on "good enough"** — Different stakeholders had different accuracy thresholds. Finance wanted 99%+ to avoid rework. Operations wanted 85%+ to get speed. Getting alignment on per-field thresholds early saved months of rework.

4. **Start with the hardest document type** — We started with drawings (the most complex) rather than manuals (the simplest). This front-loaded risk and meant that when we got to records and manuals, the architecture was already battle-tested.
