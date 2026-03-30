# Clark Venture Workspace

This workspace is structured to develop CLARK as a research-driven venture project that can later be run in a fuller OpenClaw environment.

## What This Repo Holds

- `docs/`: source-of-truth planning, research, decisions, product specs, pilot plans, and fundraising materials
- `agents/`: reusable agent briefs and project-specific task packets
- `prompts/`: reusable prompts for running CLARK workstreams
- `templates/`: structured research and analysis templates
- `workflows/`: repeatable runbooks for how work should move through the repo

## Docs Layout

- `docs/` root: folder hub only; core materials now live in purpose-specific subfolders
- `docs/process/`: living control documents that get updated as the venture advances
- `docs/venture/`: top-level venture narrative and strategy documents
- `docs/research/supporting/`: supporting research memos, market scans, and comparative materials
- `docs/products/`: product-family documentation, organized by revenue-bearing line
- `docs/pilots/`: Niagara pilot planning, checklist, staffing, tracker, and demo materials
- `docs/financials/`: startup-capital, launch-cost, and venture-finance planning documents
- `docs/support-dev/`: partner, investor, and strategic-promotion support materials beyond the main `docs/venture/pitch.md`

## Source Of Truth

- `docs/venture/venture-overview.md`: current company thesis
- `docs/process/research.md`: evidence base and unresolved questions
- `docs/process/research-board.md`: prioritized research execution queue
- `docs/venture/business-plan.md`: strategy and business model decisions
- `docs/financials/startup-costs-first-pass.md`: first-pass review of CLARK startup capital needs and raise / borrow scenarios
- `docs/financials/canadian-fin-programs.md`: detailed Canadian regional, provincial, and federal financing-program research for CLARK's early-stage capital stack
- `docs/financials/investor-fin-strategy.md`: investor-capital strategy mapped to CLARK's proof ladder, first-scan targets, and cross-border financing lanes
- `docs/financials/investors-and-investment-firms.md`: pure markdown master list of investor names, firms, angel networks, and ecosystem patrons to keep expanding over time
- `docs/financials/use-of-funds-12-to-18-months.md`: 12 to 18 month capital-use plan with burn, staffing, and raise-range guidance
- `docs/financials/use-of-funds-and-milestones.md`: investor-facing summary of capital deployment, Hamilton co-location activation, and milestone unlocks
- `docs/financials/hamilton-colocation-term-sheet-outline.md`: structured negotiation outline for the CLARK and Niagara Assembly co-location at 24 Clark Ave
- `docs/venture/pitch.md`: investor-facing narrative
- `docs/process/decision-log.md`: decisions that have been made
- `docs/process/claims-register.md`: which claims are safe, weak, or rejected
- `docs/process/beachhead-customer-decision.md`: explicit selection of the first customer segment, buyer, and program profile
- `docs/process/corridor-comparison-memo.md`: investor-safe comparison of Hamilton-Buffalo against realistic alternatives
- `docs/process/incentive-validation-sheet.md`: narrowed incentive language showing what is safe to say now
- `docs/process/dunkirk-scope-memo.md`: scope classification for Dunkirk as a later logistics option rather than active footprint
- `docs/process/workforce-housing-scope-memo.md`: scope boundary keeping housing out of the current venture plan
- `AGENTS.md`: contributor guide for repository contributors
- `docs/process/documentation-status.md`: current documentation audit showing what is complete and what remains outstanding
- `docs/program-status-memo.md`: current program-level status, completed work, active work, and next decisions
- `docs/process/investor-selection-criteria.md`: scorecard for deciding which investors or capital actors belong in CLARK's active target set
- `docs/process/investor-discovery-criteria.md`: discovery framework for finding distinct sets of investors, strategics, family offices, and public-capital actors
- `docs/products/README.md`: current product-family map with confirmed revenue lines and sequencing notes
- `docs/products/product-roadmap.md`: cross-product roadmap prioritizing training, Clarkware development and testing, and later network service lines
- `docs/products/training-services/hamilton-training-model.md`: operating model for the first CLARK-run training facility inside Niagara Assembly
- `docs/products/training-services/training-financial-model.md`: working economics for IPC delivery, train-the-trainer activity, and Clark Courses
- `docs/products/training-services/clark-certified-records-implementation-brief.md`: staged implementation path for verifiable Clark Certified records by the end of year 1
- `docs/courses/clark-courses.md`: Clark Courses service profile, delivery model, and positioning
- `docs/courses/README.md`: Clark Courses curriculum map across Foundations, Professional Practice, and Advanced Topics
- `docs/products/assembly-services/assembly-services.md`: partner-facility assembly-service framing for the CLARK network
- `docs/products/clarkware/clarkware-financial-plan.md`: staged commercialization and pricing architecture for Clarkware
- `docs/products/firmware-optimization/firmware-optimization.md`: partner-specialist firmware-service framing for the CLARK network
- `docs/products/partner-facility-development/partner-facility-development-financial-plan.md`: qualification, onboarding, and recurring-value model for node development
- `docs/products/logistics-and-facility-coordination/logistics-and-facility-coordination.md`: later-phase logistics and facility-coordination service framing
- `docs/process/workflow.md`: high-level document flow from research to plan to pitch
- `docs/research/supporting/competitor-map.md`: consolidated competitor, analog, benchmark, and substitute map
- `docs/research/supporting/canadian-investor-scan.md`: Canada-only investor research memo covering fit, stage, contact paths, and strategic takeaways for CLARK
- `docs/products/clarkware/clarkware.md`: software-family concept, IPE definition, and version 1 boundary
- `docs/products/clarkware/clarkware-architecture.md`: version 1 technical architecture, data model, messaging, and deployment approach
- `docs/products/clarkware/clark-ipe-architecture.md`: IPE-specific architecture draft and companion source export
- `docs/products/clarkware/clarkware-data-model.md`: canonical version 1 objects, event taxonomy, and sample payloads
- `docs/products/clarkware/clarkware-messaging-model.md`: XMPP-oriented conversation, presence, and AI-participation model
- `docs/products/clarkware/network-operating-model.md`: how the network and software layers fit together operationally
- `docs/pilots/niagara-clarkware-pilot.md`: first pilot scope, success criteria, and rollout plan
- `docs/support-dev/clarkware-technical-diligence.md`: investor-facing technical diligence framing and risk analysis
- `docs/products/clarkware/clarkware-roadmap.md`: phased milestones from workstation proof to node standardization
- `docs/pilots/niagara-clarkware-demo-script.md`: 5 to 10 minute investor demo walkthrough for the Niagara pilot
- `docs/support-dev/clarkware-investor-one-pager.md`: concise investor-facing product summary for Clarkware
- `docs/support-dev/clarkware-slide-outline.md`: slide-by-slide Clarkware narrative for deck integration
- `docs/pilots/niagara-pilot-requirements-checklist.md`: operational checklist for standing up the first Niagara pilot
- `docs/support-dev/clarkware-deck-draft.md`: markdown deck draft for Clarkware investor narrative
- `docs/pilots/niagara-pilot-task-tracker.md`: task tracker with owners, statuses, and immediate next actions
- `docs/support-dev/clarkware-faq.md`: investor and technical objection-handling FAQ
- `docs/support-dev/clarkware-deck-short.md`: compressed investor-ready Clarkware deck version
- `docs/pilots/niagara-pilot-staffing-and-station-sheet.md`: concrete staffing, station, instrument, and access sheet for the first Niagara pilot
- `docs/financials/hamilton-colocation-draft-terms.md`: working draft positions for the first CLARK and Niagara Assembly co-location agreement
- `docs/financials/node-economics-and-fee-stack.md`: explicit first-pass fee stack, node economics, and proof-case revenue logic

## Recommended Working Order

1. Start with `docs/process/research-board.md`
2. Gather evidence in `docs/process/research.md`
3. Record decisions in `docs/process/decision-log.md`
4. Update `docs/process/claims-register.md` when a claim becomes usable or unusable
5. Promote validated conclusions into `docs/venture/business-plan.md` and `docs/venture/venture-overview.md`
6. Refresh `docs/venture/pitch.md` only after the strategy is stable

## Reusable Assets

- Use `templates/` for consistent research outputs
- Use `workflows/` for recurring execution patterns
- Use `agents/tasks/` for bounded project-specific assignments
- Use `prompts/` when running multi-agent CLARK work in sequence
- Use `agents/clark-courses-instructor.md` when developing or extending Clark Courses curriculum materials

## Current Status

The repo now contains a coherent CLARK operating structure: venture thesis, research system, business plan draft, pitch draft, agent prompts, task packets, templates, workflows, and decision controls. The next high-value work is validating the beachhead customer, competitor analogs, corridor advantage, and incentive stack.
