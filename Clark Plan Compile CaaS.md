# Clarkware Architecture v1

## Purpose

This document translates the Clarkware concept into an initial technical architecture for version 1. It is intended to be specific enough to guide early product design, technical diligence, and first implementation decisions without pretending that every engineering choice is already settled.

This architecture is built around a simple principle: Clarkware should preserve and relay trustworthy operational memory inside real industrial environments.

## Version 1 Goal

Clarkware version 1 should prove that CLARK can deploy workstation-level Integrated Production Environments inside a real facility, connect them to meaningful production activity, capture human and AI actions in a reviewable record, and produce facility-level visibility from that underlying record.

The first architecture should therefore optimize for:

- workstation usability
- evidence capture
- auditable messaging and coordination
- node-resident deployment
- controlled remote participation
- extensibility for later integrations

It should not optimize first for broad ERP replacement, full plant automation, or complex multi-site orchestration.

## Architectural Summary

Clarkware version 1 should be structured as six cooperating layers:

1. IPE client layer
2. Local integration and event capture layer
3. Messaging and collaboration layer
4. Operational record and query layer
5. Reporting and workflow services layer
6. Node infrastructure and trust layer

Each layer should be replaceable or extensible without forcing the whole product family into one rigid application.

## Layer 1: IPE Client Layer

### Role

The IPE client is the primary workstation-level environment where operators, technicians, quality personnel, supervisors, and experts interact with Clarkware.

### Implementation Direction

Theia should be the main shell for version 1 IPEs where mixed tooling and extension-based workspaces are needed. Theia is a strong fit because it already supports modular panels, commands, views, logs, editors, extensions, and multi-runtime deployment patterns.

Version 1 IPE clients should be able to run in one or both of these forms:

- browser-based client served from a node-resident Clarkware server
- packaged desktop client for stations that need stronger local-first behavior or tighter integration with local tools

### Core IPE Surfaces

Each IPE should be able to host these first-class surfaces:

- job context view
- workstation context view
- note composer and note history
- conversation and presence pane
- event stream pane
- artifact and attachment pane
- machine or tool integration pane where applicable
- AI assistant pane with reviewable outputs
- task and checklist pane

### UI Principles

The UI should prefer:

- fast access to current job context
- persistent visibility of workstation identity
- one-click note capture
- explicit distinction between observed facts, proposed actions, and AI-generated content
- strong keyboard support where practical
- low-friction image, file, and test-output attachment

## Layer 2: Local Integration And Event Capture Layer

### Role

This layer connects the workstation environment to the local operational reality: tools, instruments, files, sessions, and operator actions.

### Version 1 Responsibilities

- collect workstation session events
- normalize local tool and test events into Clarkware event objects
- bind events to workstation, job, and actor context where possible
- support attachment capture from local file systems, screenshots, exports, and instrument outputs
- buffer events during temporary network disruption

### Integration Model

Version 1 should prefer adapters over deep monolithic integrations. An adapter should do four things:

- identify the source system or device
- capture raw events or outputs
- map them into canonical Clarkware objects
- preserve raw evidence when useful for later audit or debugging

### Niagara-First Integration Priorities

The first Niagara Assembly integration set should focus on testing and note-linked evidence capture. Initial targets should include:

- bench test instruments that export results as files, CSV, JSON, XML, or serial output
- calibration records and measurement exports
- image capture tied to workstation and job context
- firmware build or test artifacts relevant to a production or diagnostics workflow
- operator note capture linked to attached evidence and current task state

Version 1 should not require full direct command-and-control integration with every device. It is enough to prove trustworthy event capture and contextual linkage from a small number of high-value tool paths.

## Layer 3: Messaging And Collaboration Layer

### Role

This layer supports the conversations, alerts, and presence information that allow distributed industrial work to move quickly without losing accountability.

### Core Direction

An XMPP-based or strongly XMPP-aligned server architecture should be treated as a first-order requirement, not an optional add-on. Clarkware's collaboration model depends on durable identity-aware communication that can include humans, AI agents, and automation services.

### Why XMPP Fits

XMPP is a strong fit because it supports:

- identity-aware messaging
- presence
- room-based and direct communication patterns
- extensibility through standardized and custom extensions
- federation options for later multi-node topologies
- inspectable standards-based behavior

### Version 1 Messaging Objects

The messaging layer should support at least these entities:

- direct conversation
- workspace channel
- job-linked thread
- issue-linked thread
- AI agent thread or AI-participant identity
- system alert message
- workflow notification

### Message Design Requirements

Messages should be able to carry:

- sender identity
- sender type: human, AI, or automation
- timestamp
- facility and workstation context when relevant
- linked job or issue context
- attachments or artifact references
- message classification such as note, question, alert, recommendation, handoff, or resolution
- review state where an AI-generated message requires explicit acknowledgment

### Presence Model

Presence should not only mean online or offline. Clarkware should support operational presence states such as:

- available
- heads down
- on station
- in review
- awaiting response
- unavailable
- automation active

These states should remain simple in version 1 and not become heavy workflow bureaucracy.

## Layer 4: Operational Record And Query Layer

### Role

This layer is the core memory of Clarkware. It stores the canonical record of what happened.

### Design Principle

The system should preserve both narrative continuity and structured queryability. This means Clarkware needs both linked operational objects and durable event history.

### Canonical First-Class Objects

Clarkware version 1 should define at least the following first-class objects:

- Facility
- Zone
- Workstation
- Job
- Task
- Issue
- Conversation
- Message
- Note
- Event
- Tool
- Machine Session
- Artifact
- Person
- Agent
- Role
- Permission Grant

### Core Relationship Rules

The most important relationships are:

- every workstation belongs to a facility
- every event should belong to a workstation, job, issue, or conversation context where possible
- every note should identify author, time, and context
- every AI action should identify the responsible agent identity and linked review context
- every artifact should be linkable to source event, job, and workstation
- every conversation should be linkable to a job, issue, workspace, or facility scope

### Event Model

The event model should be append-first. In practice this means new events are written as new records, while higher-level objects may maintain current state derived from those events.

Example event types:

- workstation.session.started
- workstation.session.ended
- job.started
- job.paused
- task.completed
- note.created
- note.revised
- artifact.attached
- test.run.imported
- calibration.result.imported
- message.sent
- ai.summary.generated
- ai.recommendation.accepted
- ai.recommendation.rejected
- issue.opened
- issue.resolved

### Storage Direction

Version 1 should use a pragmatic mixed storage model:

- relational storage for canonical objects and permissions
- append-oriented event storage for event history and replayable audit trails
- object storage for files, images, exports, logs, and evidence artifacts
- search indexing for notes, messages, and key event fields

The exact database choices can vary, but the model should preserve structured integrity, durable history, and fast retrieval.

## Layer 5: Reporting And Workflow Services Layer

### Role

This layer turns raw operational records into useful views, summaries, escalations, and reports.

### Version 1 Responsibilities

- build role-based dashboards from canonical data
- generate issue and bottleneck summaries
- support shift, workstation, job, and facility reporting
- surface test and quality exceptions
- provide recent-history and handoff views
- generate AI-assisted summaries that remain reviewable and attributable

### Reporting Principle

Version 1 reports should be derived from recorded events, notes, messages, and artifacts. Clarkware should avoid dashboards that look polished but are disconnected from the underlying operational record.

### Workflow Principle

Clarkware should support workflow evolution rather than rigid deterministic sequencing. Version 1 workflows should therefore be light-touch:

- assign
- notify
- acknowledge
- escalate
- resolve
- hand off

This is enough to support real operations without forcing every site into an over-designed workflow engine.

## Layer 6: Node Infrastructure And Trust Layer

### Role

This layer makes Clarkware deployable in real facilities with privacy, inspection, ownership, and resilience properties that support CLARK's business model.

### Deployment Model

Each major node should run Clarkware server infrastructure under CLARK's operating model, with data governance and access rules appropriate to the facility and customer context.

A typical node deployment should include:

- Clarkware application services
- XMPP or XMPP-aligned messaging services
- operational database services
- search and indexing services
- object storage for artifacts and backups
- integration adapter services
- identity and access control services
- observability and audit services

### Trust Requirements

Version 1 infrastructure should support:

- owner-accessible exports
- clear backup and restore procedures
- inspectable service architecture
- environment-level audit logging
- role-based remote access controls
- documented data residency and retention rules

### Remote Access Model

Remote access should be treated as controlled participation, not unrestricted platform access. A remote expert should be able to view or contribute to the contexts they are authorized for without gaining unnecessary visibility into unrelated facility activity.

## Identity And Permission Model

### Identity Types

Clarkware should recognize three broad actor types:

- human user
- AI agent
- automation service

Each actor should have a distinct identity and attributable actions.

### Human Roles

Version 1 should support roles such as:

- owner
- facility administrator
- supervisor
- operator
- quality reviewer
- remote expert
- observer or auditor

### Permission Model

Permissions should be granted at multiple scopes:

- facility scope
- zone or department scope
- workstation scope
- job scope
- issue scope
- conversation scope

### Permission Categories

Version 1 permission categories should include:

- view
- comment
- create note
- attach artifact
- initiate conversation
- participate remotely
- review AI output
- approve disposition
- administer integrations
- export records
- manage retention and backup settings

### AI Permission Rules

AI agents should not inherit broad human permissions by default. They should operate through explicit bounded grants tied to defined contexts and action classes.

An AI agent should be able to:

- summarize activity in authorized contexts
- propose next actions
- draft notes or reports
- route messages or alerts

An AI agent should not be able to silently finalize critical operational actions without explicit human review unless a later, separate automation policy allows that in a tightly bounded case.

## Local-First And Offline Behavior

### Principle

Clarkware version 1 should assume that workstation reliability matters more than permanent connectivity.

### Minimum Local-First Guarantees

At minimum, a workstation IPE should continue to support during temporary disconnection:

- viewing the current assigned job context and recent local history
- capturing notes
- attaching locally available artifacts
- recording local events with reliable timestamps
- queueing messages and sync operations for later delivery
- preserving explicit sync status so users know what has and has not propagated

### Sync Rules

When connectivity returns, the system should:

- sync queued notes, events, and attachments
- preserve original local timestamps and creation context
- mark conflicts explicitly rather than silently overwriting
- keep append-only event history intact

### Conflict Strategy

Version 1 should prefer explicit conflict visibility over hidden automatic reconciliation in high-accountability records.

## AI Agent Architecture

### Role Of AI In Version 1

AI in Clarkware version 1 should be assistive, not sovereign.

Primary AI functions should include:

- summarization of recent activity
- issue triage suggestions
- note drafting assistance
- retrieval of related historical context
- alert routing suggestions
- handoff summary generation

### AI Execution Pattern

AI services should operate as explicitly identified participants attached to recorded contexts rather than as invisible background magic.

Every meaningful AI output should carry:

- agent identity
- generation time
- source context references where practical
- review state
- acceptance, edit, or rejection outcome where applicable

### Agent Communication Pattern

AI agents should participate through the same messaging and event framework as other actors wherever feasible. This keeps the audit model coherent.

## Suggested Service Topology

A pragmatic version 1 topology could include:

- web gateway and API service
- IPE workspace service
- XMPP messaging service
- identity and access service
- event ingestion service
- integration adapter service
- reporting and query service
- AI orchestration service
- relational database
- event store
- object storage
- search index

This can start as a modular monolith with clearly separated service boundaries, then split further only where scale or operational concerns justify it.

## Security And Compliance Posture

Version 1 should emphasize:

- strong authentication
- role-scoped authorization
- transport encryption
- audit logging for sensitive actions
- explicit remote-session controls
- documented retention behavior
- exportability for owner review
- clear separation between operational data and broader cross-node intelligence layers

Clarkware should be able to support compliance-related workflows through better records and traceability, but it should avoid marketing itself as a complete compliance platform until those claims are proven in operation.

## Niagara Assembly Pilot Blueprint

The first credible deployment should prove five things at Niagara Assembly:

1. A workstation-level IPE can be used in real daily work.
2. At least one meaningful test-tool path can feed evidence into the record.
3. Operators and remote experts can collaborate in job-linked threads.
4. AI agents can generate useful summaries or routing suggestions with full auditability.
5. Facility leadership can review a trustworthy activity narrative derived from workstation records.

### Suggested First Pilot Workstation

The first pilot station should be one where these conditions hold:

- test activity is common enough to generate useful evidence
- notes and handoffs already matter operationally
- remote expert input is plausibly valuable
- a small number of users can create dense learning quickly

A diagnostics, test, or calibration-linked station is likely a better first candidate than a broadly generalized production environment.

## Build Sequence

### Phase 1

- define canonical object and event schemas
- define role and permission model
- stand up XMPP server and identity model
- build first IPE shell with notes, job context, and messaging

### Phase 2

- build first test-tool integration adapter
- implement artifact capture and attachment flows
- implement append-first event ingestion and audit storage
- build first manager and quality reporting views

### Phase 3

- add AI summary and routing services
- refine local-first sync behavior
- add facility-level exception views and handoff summaries
- prepare node-resident export and inspection workflows

## Open Technical Questions

- Which exact protocols and file formats dominate the first Niagara test-tool integrations?
- Should the first event store be implemented inside the primary relational system or as a separate append-oriented service?
- Which identity provider model best fits node-resident deployments with controlled remote access?
- What retention classes should exist for notes, messages, machine events, and attached artifacts?
- How much federation should the initial XMPP architecture support before there are multiple active nodes?
- Which events are important enough to require immutable retention from day one?


# Clarkware Data Model v1

## Purpose

This document defines the initial canonical data model for Clarkware version 1. It is intended to make the Clarkware concept and architecture concrete enough for product design, backend implementation, interface design, and technical diligence.

The data model is designed around one principle: Clarkware should preserve operational narrative continuity without giving up structured queryability.

## Modeling Principles

### 1. Context Before Abstraction

Most Clarkware records should be meaningful in place. A note, message, event, or artifact should usually be tied to a workstation, job, issue, conversation, or facility context.

### 2. Append-First History

High-accountability actions should generally produce new events rather than destructive overwrites. Current state can be derived, but history should remain reviewable.

### 3. Human, AI, And Automation Are Distinct Actor Types

All three can act in the system, but the model should distinguish them clearly.

### 4. Operational Objects Need Stable IDs

Facilities, workstations, jobs, issues, conversations, artifacts, and actors should all have stable identifiers so records can remain linked over time.

### 5. Evidence Must Be Linkable

Machine outputs, files, images, and logs should be treated as first-class evidence artifacts, not as incidental attachments with weak traceability.

## Canonical Object Set

Version 1 should include these first-class objects:

- Facility
- Zone
- Workstation
- Job
- Task
- Issue
- Conversation
- Message
- Note
- Event
- Tool
- MachineSession
- Artifact
- Person
- Agent
- AutomationService
- Role
- PermissionGrant

## Object Definitions

### Facility

Represents a physical operating site such as Niagara Assembly.

Core fields:

- `facility_id`
- `name`
- `code`
- `jurisdiction`
- `timezone`
- `status`
- `owner_org`
- `metadata`

Example:

```json
{
  "facility_id": "fac_niagara_001",
  "name": "Niagara Assembly",
  "code": "NIAGARA",
  "jurisdiction": "ON-CA",
  "timezone": "America/Toronto",
  "status": "active"
}
```

### Zone

Represents a sub-area within a facility such as diagnostics, test, calibration, assembly, or receiving.

Core fields:

- `zone_id`
- `facility_id`
- `name`
- `zone_type`
- `status`

### Workstation

Represents a bench, station, line position, test rig, or other local operating context where work is performed.

Core fields:

- `workstation_id`
- `facility_id`
- `zone_id`
- `name`
- `station_type`
- `status`
- `device_profile`
- `integration_profile`

Example:

```json
{
  "workstation_id": "ws_testbench_a01",
  "facility_id": "fac_niagara_001",
  "zone_id": "zone_test_lab",
  "name": "Test Bench A01",
  "station_type": "diagnostics_test",
  "status": "active",
  "integration_profile": ["scope_export", "camera_capture", "firmware_artifacts"]
}
```

### Job

Represents a unit of tracked operational work. A job may correspond to a production batch, test activity, diagnostics task, calibration task, or other bounded piece of work.

Core fields:

- `job_id`
- `facility_id`
- `workstation_id` or `primary_workstation_id`
- `job_type`
- `title`
- `status`
- `priority`
- `customer_ref`
- `product_ref`
- `opened_at`
- `closed_at`
- `current_owner_actor_id`

Example:

```json
{
  "job_id": "job_2026_niagara_tb_a01_0042",
  "facility_id": "fac_niagara_001",
  "primary_workstation_id": "ws_testbench_a01",
  "job_type": "board_diagnostics",
  "title": "Power board validation run",
  "status": "in_progress",
  "priority": "high",
  "customer_ref": "cust_internal_proto",
  "product_ref": "pwr_board_rev_c"
}
```

### Task

Represents a smaller assigned or tracked unit of work inside a job.

Core fields:

- `task_id`
- `job_id`
- `title`
- `status`
- `assigned_actor_id`
- `due_at`
- `sequence_hint`
- `task_type`

### Issue

Represents an operational problem, anomaly, exception, or decision point that requires attention.

Core fields:

- `issue_id`
- `facility_id`
- `job_id`
- `workstation_id`
- `severity`
- `status`
- `issue_type`
- `opened_by_actor_id`
- `opened_at`
- `resolved_at`

### Conversation

Represents a contextual communication space. A conversation can be direct, workspace-linked, job-linked, or issue-linked.

Core fields:

- `conversation_id`
- `conversation_type`
- `facility_id`
- `zone_id`
- `workstation_id`
- `job_id`
- `issue_id`
- `title`
- `status`

Allowed `conversation_type` values in v1:

- `direct`
- `workspace`
- `job`
- `issue`
- `system`
- `ai_assist`

### Message

Represents an individual message inside a conversation.

Core fields:

- `message_id`
- `conversation_id`
- `sender_actor_id`
- `sender_type`
- `message_type`
- `body`
- `created_at`
- `review_state`
- `artifact_refs`
- `job_id`
- `issue_id`
- `workstation_id`

Allowed `message_type` values in v1:

- `note_like`
- `question`
- `alert`
- `handoff`
- `recommendation`
- `resolution`
- `system`

Allowed `review_state` values in v1:

- `not_required`
- `pending_review`
- `accepted`
- `rejected`
- `edited`

### Note

Represents a durable operational note. Notes are distinct from messages because they are part of the long-lived operating record.

Core fields:

- `note_id`
- `author_actor_id`
- `author_type`
- `facility_id`
- `workstation_id`
- `job_id`
- `issue_id`
- `note_type`
- `body`
- `visibility_scope`
- `created_at`
- `revised_at`
- `supersedes_note_id`

Allowed `note_type` values in v1:

- `observation`
- `operator_log`
- `quality_note`
- `test_note`
- `shift_handoff`
- `ai_draft`
- `resolution_note`

### Event

Represents an append-only recorded occurrence.

Core fields:

- `event_id`
- `event_type`
- `facility_id`
- `workstation_id`
- `job_id`
- `issue_id`
- `conversation_id`
- `actor_id`
- `actor_type`
- `source_type`
- `occurred_at`
- `recorded_at`
- `payload`
- `artifact_refs`

Allowed `source_type` values in v1:

- `human_ui`
- `ai_agent`
- `automation_service`
- `tool_adapter`
- `system`

Example:

```json
{
  "event_id": "evt_01jws8k5x0y3",
  "event_type": "test.run.imported",
  "facility_id": "fac_niagara_001",
  "workstation_id": "ws_testbench_a01",
  "job_id": "job_2026_niagara_tb_a01_0042",
  "actor_id": "svc_adapter_scope_01",
  "actor_type": "automation_service",
  "source_type": "tool_adapter",
  "occurred_at": "2026-03-23T13:22:10Z",
  "recorded_at": "2026-03-23T13:22:12Z",
  "payload": {
    "tool_type": "oscilloscope",
    "import_format": "csv",
    "run_label": "validation_pass_1"
  }
}
```

### Tool

Represents an instrument, machine, application, or local utility that can generate evidence or participate in workflow.

Core fields:

- `tool_id`
- `facility_id`
- `workstation_id`
- `tool_type`
- `name`
- `vendor`
- `model`
- `integration_mode`
- `status`

Allowed `integration_mode` values in v1:

- `manual_attach`
- `file_import`
- `watched_folder`
- `api_adapter`
- `serial_adapter`
- `network_adapter`

### MachineSession

Represents a bounded interaction period between a workstation user or job and a tool or machine.

Core fields:

- `machine_session_id`
- `tool_id`
- `workstation_id`
- `job_id`
- `started_at`
- `ended_at`
- `operator_actor_id`
- `session_label`

### Artifact

Represents durable evidence such as files, exports, images, logs, or generated reports.

Core fields:

- `artifact_id`
- `artifact_type`
- `storage_uri`
- `checksum`
- `created_at`
- `created_by_actor_id`
- `facility_id`
- `workstation_id`
- `job_id`
- `issue_id`
- `source_event_id`
- `metadata`

Allowed `artifact_type` values in v1:

- `image`
- `file`
- `test_output`
- `calibration_output`
- `firmware_build`
- `log_export`
- `report`

### Person

Represents a human actor.

Core fields:

- `person_id`
- `display_name`
- `employment_type`
- `primary_role`
- `facility_affiliation`
- `status`

### Agent

Represents an identifiable AI participant.

Core fields:

- `agent_id`
- `display_name`
- `agent_type`
- `operator_org`
- `status`
- `allowed_action_classes`

Allowed `agent_type` values in v1:

- `summarizer`
- `router`
- `retrieval_assistant`
- `drafting_assistant`
- `triage_assistant`

### AutomationService

Represents a non-human non-AI system identity such as an integration adapter or background worker.

Core fields:

- `service_id`
- `display_name`
- `service_type`
- `status`
- `facility_scope`

### Role

Represents a reusable privilege role.

Allowed role examples in v1:

- `owner`
- `facility_admin`
- `supervisor`
- `operator`
- `quality_reviewer`
- `remote_expert`
- `observer`
- `agent_bounded`
- `service_adapter`

### PermissionGrant

Represents a scoped permission assignment.

Core fields:

- `permission_grant_id`
- `actor_id`
- `actor_type`
- `role_id`
- `scope_type`
- `scope_id`
- `permission_list`
- `granted_at`
- `granted_by_actor_id`
- `expires_at`

Allowed `scope_type` values in v1:

- `facility`
- `zone`
- `workstation`
- `job`
- `issue`
- `conversation`

## Relationship Summary

Minimum required relationships:

- `Facility 1 -> many Zones`
- `Facility 1 -> many Workstations`
- `Facility 1 -> many Jobs`
- `Workstation 1 -> many Jobs`
- `Job 1 -> many Tasks`
- `Job 1 -> many Notes`
- `Job 1 -> many Events`
- `Job 1 -> many Artifacts`
- `Issue 1 -> many Notes`
- `Conversation 1 -> many Messages`
- `Tool 1 -> many MachineSessions`
- `MachineSession 1 -> many Events`
- `Actor 1 -> many Notes / Messages / Events`

## Event Taxonomy

Version 1 event families should include:

- workstation events
- job lifecycle events
- task workflow events
- note lifecycle events
- artifact lifecycle events
- conversation and message events
- issue lifecycle events
- tool integration events
- AI action events
- permission and access-control events

Suggested event names:

- `workstation.session.started`
- `workstation.session.ended`
- `job.created`
- `job.started`
- `job.paused`
- `job.closed`
- `task.assigned`
- `task.completed`
- `note.created`
- `note.revised`
- `artifact.attached`
- `conversation.created`
- `message.sent`
- `issue.opened`
- `issue.escalated`
- `issue.resolved`
- `tool.adapter.connected`
- `test.run.imported`
- `calibration.result.imported`
- `ai.summary.generated`
- `ai.recommendation.accepted`
- `ai.recommendation.rejected`
- `permission.granted`

## JSON Envelope Patterns

### Standard Event Envelope

```json
{
  "event_id": "evt_01jws8k5x0y3",
  "event_type": "note.created",
  "facility_id": "fac_niagara_001",
  "workstation_id": "ws_testbench_a01",
  "job_id": "job_2026_niagara_tb_a01_0042",
  "issue_id": null,
  "conversation_id": null,
  "actor": {
    "actor_id": "person_taylor_01",
    "actor_type": "human"
  },
  "source_type": "human_ui",
  "occurred_at": "2026-03-23T13:18:00Z",
  "recorded_at": "2026-03-23T13:18:01Z",
  "payload": {
    "note_id": "note_01jws8jv9a0e",
    "note_type": "observation"
  },
  "artifact_refs": []
}
```

### Standard Message Envelope

```json
{
  "message_id": "msg_01jws8mmd0qe",
  "conversation_id": "conv_job_0042",
  "sender_actor_id": "agent_summary_01",
  "sender_type": "ai_agent",
  "message_type": "recommendation",
  "review_state": "pending_review",
  "job_id": "job_2026_niagara_tb_a01_0042",
  "body": "Three recent anomalies resemble prior voltage drift failures. Recommend connector inspection before rerun.",
  "artifact_refs": ["art_report_0192"],
  "created_at": "2026-03-23T13:24:11Z"
}
```

## Retention And Mutability Guidance

Version 1 should treat these as effectively immutable once recorded, subject only to additive correction events:

- events
- sent messages
- attached artifacts
- AI recommendation outcome records
- permission grant history

Version 1 can allow revision with linked history for:

- notes
- task descriptions
- job metadata
- issue summaries

## Search And Query Priorities

Search should be able to filter by:

- facility
- workstation
- job
- issue
- actor
- event type
- artifact type
- date range
- tool
- message or note content

## Data Model Risks To Avoid

Version 1 should avoid:

- overly generic polymorphic models that make accountability hard to query
- unscoped notes and messages with weak context
- silent mutation of historical records
- AI content without review or attribution metadata
- attachments stored without stable artifact IDs and checksums

## Near-Term Implementation Questions

- Should `Actor` be a unified abstract table with typed specializations, or separate tables joined through a shared identity layer?
- Which event families must be immutable from day one?
- How much denormalized read-model materialization is needed for fast facility dashboards?
- Which artifact metadata fields should be standardized for test outputs in the Niagara pilot?




# Clarkware Messaging Model v1

## Purpose

This document defines the version 1 messaging model for Clarkware. It describes how conversation, presence, AI participation, and event-linked communication should work inside an XMPP-based or strongly XMPP-aligned architecture.

The messaging system is not a side feature. It is part of Clarkware's operational memory system.

## Messaging Principles

### 1. Conversation Must Stay Tied To Work

Messages should usually be linked to a job, issue, workstation, or workspace context.

### 2. Identity Must Be Explicit

Every participant must have a clear identity, including human users, AI agents, and automation services.

### 3. AI Participation Must Be Visible

AI agents should participate as named actors, not as invisible background behavior.

### 4. Messaging Must Support Auditability Without Becoming Rigid

Operational communication needs to be searchable, attributable, and preservable without becoming so formal that people avoid using it.

## Core Messaging Entities

Version 1 messaging should support:

- direct conversations
- workspace channels
- job-linked threads
- issue-linked threads
- AI assist threads
- system alert streams

## Conversation Types

### Direct Conversation

Used for person-to-person or person-to-agent exchanges.

Example ID pattern:

- `dm.person_taylor_01.person_morgan_01`

### Workspace Channel

Used for an ongoing station, bench, line, or team context.

Example ID pattern:

- `ws.ws_testbench_a01`

### Job Thread

Used for communication specific to a job.

Example ID pattern:

- `job.job_2026_niagara_tb_a01_0042`

### Issue Thread

Used for anomalies, exceptions, or escalations.

Example ID pattern:

- `issue.issue_01jws8pxd6w4`

### AI Assist Thread

Used when an AI assistant participates in a bounded support context.

Example ID pattern:

- `ai.job_2026_niagara_tb_a01_0042.summary`

## XMPP-Oriented Model

Version 1 should use XMPP concepts in a practical way:

- JIDs for actor identity
- rooms or room-like constructs for shared threads
- direct messages for bounded exchanges
- presence states for operational availability
- extension payloads or payload-linked references for job, issue, and workstation context

Illustrative JID patterns:

- `person.taylor@niagara.clark`
- `agent.summary01@niagara.clark`
- `svc.adapter.scope01@niagara.clark`
- `room.job_2026_niagara_tb_a01_0042@conference.niagara.clark`

## Message Classes

Version 1 should classify messages for downstream handling.

Allowed message classes:

- `chat`
- `question`
- `alert`
- `handoff`
- `recommendation`
- `resolution`
- `status_update`
- `system_notice`

## Standard Message Payload

Every message should be able to carry:

- `message_id`
- `conversation_id`
- `sender_actor_id`
- `sender_type`
- `message_class`
- `body`
- `created_at`
- `job_id`
- `issue_id`
- `workstation_id`
- `artifact_refs`
- `review_state`
- `reply_to_message_id`
- `metadata`

Example:

```json
{
  "message_id": "msg_01jwsa17cg3p",
  "conversation_id": "job.job_2026_niagara_tb_a01_0042",
  "sender_actor_id": "person_taylor_01",
  "sender_type": "human",
  "message_class": "status_update",
  "body": "Imported first scope run and attached trace image. Investigating drift on channel 2.",
  "created_at": "2026-03-23T13:27:18Z",
  "job_id": "job_2026_niagara_tb_a01_0042",
  "issue_id": null,
  "workstation_id": "ws_testbench_a01",
  "artifact_refs": ["art_scope_trace_001"],
  "review_state": "not_required"
}
```

## Review States

Review state matters most for AI-originated messages, but the field can exist for all messages.

Allowed values:

- `not_required`
- `pending_review`
- `accepted`
- `rejected`
- `edited`

## AI Message Behavior

AI agents should be allowed to:

- summarize recent activity in a thread
- suggest likely next actions
- draft handoff notes
- flag missing evidence
- route alerts to the right human roles

AI messages should always include:

- visible agent identity
- message class
- review state where applicable
- context linkage to the triggering job, issue, or workspace

AI agents should not silently alter historical messages. Corrections should appear as new messages or linked note revisions.

## Presence Model

Version 1 should support operational presence states.

Allowed states:

- `available`
- `heads_down`
- `on_station`
- `in_review`
- `awaiting_response`
- `unavailable`
- `automation_active`

Presence should be scoped where possible. A user may be `on_station` for one workstation context and still reachable in a limited remote-support capacity elsewhere.

## Notification Semantics

Clarkware messaging should support light workflow semantics without becoming a BPM engine.

Key notification types:

- assignment notice
- escalation notice
- waiting-on-response notice
- test exception notice
- quality review request
- remote expert invite
- AI summary available notice

## Context Binding Rules

Version 1 should apply these rules:

- every job thread must include `job_id`
- every issue thread must include `issue_id`
- every workstation channel should include `workstation_id`
- every message with an attached artifact should include `artifact_refs`
- every AI recommendation should include a `review_state`
- every system alert should identify its originating service or adapter

## Event Emission From Messaging

Important messaging actions should also emit Clarkware events.

Examples:

- `conversation.created`
- `message.sent`
- `message.edited`
- `message.reviewed`
- `alert.escalated`
- `presence.changed`
- `ai.recommendation.accepted`
- `ai.recommendation.rejected`

## Room And Access Model

Version 1 room access should be governed by the Clarkware permission model.

Examples:

- a job thread is visible only to actors with rights to that job
- an issue thread can temporarily include a remote expert through an issue-scoped grant
- a workstation channel is visible to authorized facility actors for that station
- an AI assist thread may be visible only to the core participants plus reviewers

## Retention Rules

Version 1 should retain:

- job and issue threads as part of the operational record
- system notices linked to operational decisions
- AI recommendation messages and outcomes
- handoff messages with durable relevance

Version 1 may treat low-value transient presence events with shorter retention than substantive operational messages.

## Suggested XMPP Extension Payload Areas

Clarkware will likely need custom payload support or linked metadata for:

- facility context
- workstation context
- job and issue references
- artifact references
- AI review state
- message class
- actor type

## Niagara Pilot Messaging Design

The first Niagara pilot should implement:

- one workstation channel for the pilot station
- one job-thread model for active diagnostics or test jobs
- one issue-thread escalation path
- one AI summarizer identity
- one remote expert identity path
- one manager or quality-review visibility pattern

## Open Messaging Questions

- Which XMPP server implementation best fits node-resident control and extensibility requirements?
- Which message classes require immutable retention from day one?
- How much of the review-state behavior should live in XMPP payloads versus Clarkware application metadata?
- Which presence states are actually useful to operators versus merely attractive in theory?



# Clarkware Roadmap And Milestones

## Purpose

This document defines the staged roadmap for Clarkware from version 1 proof through broader node-level expansion. It is intended to show technical investors, operators, and internal builders how the product should mature without losing discipline.

## Roadmap Principle

Clarkware should expand outward from a credible workstation wedge.

The correct sequence is:

1. prove the workstation environment
2. prove evidence capture and messaging in live work
3. prove facility-level reporting from real records
4. prove repeatability across more stations and nodes
5. only then broaden into deeper orchestration and cross-node intelligence

## Phase 0: Definition

### Objective

Lock the concept, architecture, data model, messaging model, and Niagara pilot scope.

### Outputs

- Clarkware concept doc
- Clarkware architecture doc
- Clarkware data model doc
- Clarkware messaging model doc
- Niagara pilot plan

### Exit Condition

CLARK can explain the product clearly and consistently to technical investors and implementation partners.

## Phase 1: Workstation Proof

### Objective

Prove that one workstation-level IPE can operate inside Niagara Assembly during real work.

### Scope

- Theia-based IPE shell
- job context
- note capture
- artifact attachment
- job-linked messaging
- one AI summarizer or router
- one test-tool evidence path

### Milestones

- M1.1: IPE shell operational at pilot station
- M1.2: XMPP messaging operational for pilot users
- M1.3: first successful evidence import from pilot tool
- M1.4: first live AI-assisted summary or routing loop
- M1.5: first exportable record package generated from pilot usage

### Success Standard

Multiple real jobs move through the station with usable notes, evidence, and messaging tied to the same operational record.

## Phase 2: Facility Visibility

### Objective

Turn workstation records into credible facility-level visibility.

### Scope

- manager and quality-facing reporting views
- issue and exception views
- shift and handoff summaries
- review state tracking for AI recommendations
- permission-scoped remote participation

### Milestones

- M2.1: live facility dashboard from workstation records
- M2.2: issue escalation path working end to end
- M2.3: quality or supervisor review flow operational
- M2.4: handoff summary view generated from recorded activity

### Success Standard

Facility leadership can understand current activity and exceptions from the underlying record without parallel manual reporting.

## Phase 3: Station Expansion

### Objective

Prove that Clarkware is adaptable across more than one station type.

### Scope

- second workstation deployment
- third workstation deployment if justified
- broader artifact types
- broader role coverage
- additional adapter patterns

### Milestones

- M3.1: second station live
- M3.2: second tool integration path stable
- M3.3: repeated onboarding pattern documented
- M3.4: operator training and rollout checklist validated

### Success Standard

The product can be configured for additional stations without behaving like a bespoke one-off implementation every time.

## Phase 4: Node Standardization

### Objective

Turn Niagara learnings into a repeatable node package.

### Scope

- documented node deployment architecture
- standard permission templates
- standard export and backup package
- repeatable adapter interface contract
- standard facility and workstation setup flows

### Milestones

- M4.1: node deployment guide complete
- M4.2: standard identity and access templates complete
- M4.3: first reproducible staging deployment outside the original pilot environment
- M4.4: inspection and owner-export process documented

### Success Standard

CLARK can credibly describe Clarkware not only as a pilot product, but as a repeatable component of node launch and node operations.

## Phase 5: Cross-Node Intelligence

### Objective

Add carefully bounded cross-node capabilities without violating privacy or jurisdictional requirements.

### Scope

- privacy-preserving benchmarking
- reusable pattern libraries for common issues and resolutions
- broader AI retrieval across approved records
- selective federation of messaging and identity where justified

### Milestones

- M5.1: approved cross-node pattern retrieval working
- M5.2: privacy-scoped benchmarking views available
- M5.3: multi-node identity and trust model validated

### Success Standard

Clarkware creates network leverage without weakening local ownership, customer privacy, or regulatory boundaries.

## Phase 6: Deeper Automation And Orchestration

### Objective

Expand from assisted coordination into more advanced orchestration where operationally justified.

### Scope

- richer workflow automation
- semi-automated escalation and routing
- deeper machine integration
- broader AI support with strong review controls

### Milestones

- M6.1: semi-automated routing policies deployed
- M6.2: richer machine event normalization implemented
- M6.3: bounded automation policy framework approved

### Success Standard

Automation adds visible operator value without hiding responsibility or weakening trust.

## Strategic Milestones For Investors

Technical investors should care most about these milestone moments:

- a live pilot station used in real work
- a stable evidence-import path
- manager-visible reporting from live records
- a second station rollout that proves adaptability
- a node package that looks repeatable rather than artisanal

## Milestones That Matter Less Early

These should not dominate the narrative too early:

- complete ERP overlap
- full digital-twin claims
- broad machine-control claims
- large-scale federated multi-node orchestration before the first node is proven

## Roadmap Risks

- over-building before the pilot wedge is proven
- treating every station as a custom software project
- promising cross-node intelligence before governance and privacy controls exist
- adding AI faster than reviewability and trust mechanisms can support

## Recommended Investor Narrative

The strongest roadmap narrative is:

- Clarkware starts with one believable workstation wedge
- then becomes a facility reporting and coordination layer
- then becomes a repeatable node software package
- and only later becomes a broader cross-node intelligence layer

That is a much stronger story than claiming a universal industrial software platform on day one.
