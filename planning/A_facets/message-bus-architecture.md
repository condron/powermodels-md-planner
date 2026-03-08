---
type: facet
id: message-bus-architecture
label: Message Bus Architecture
synonyms:
  - two-bus pattern
  - message flow
  - command routing
  - event publishing
  - SpreadsheetContextBus
  - envelope commands
  - message namespace boundaries
  - dispatcher pattern
links_to_facets:
  - event-sourced-architecture
  - isolation-acl-pattern
  - domain-model-structure
claims:
  - Two-bus architecture separates internal app communication from server communication
  - SpreadsheetContextBus decorator auto-attaches routing metadata to every message
  - Six-step message flow from UI action through ACL translation to domain handling and back
  - Three message namespace boundaries enforce layer separation (Domain, ACL, UI)
  - Envelope commands abstract workspace and model routing from the UI layer
  - NullBus provides replay safety during aggregate reconstruction
  - QueuedSubscriber services handle commands with automatic ack/success/failure responses
evidence_refs:
  - technical-codebase-review
owner: technical-team
status: active
---

# Message Bus Architecture

## Description

PowerModels uses a two-bus architecture with decorator pattern for metadata propagation. Messages flow through six steps: UI action → ACL routing → ACL translation → domain handling → read model projection → UI update. Three namespace boundaries (Domain, ACL, UI) enforce layer separation.

## Key Points

- **InternalBus**: App-to-app communication (local Dispatcher)
- **ExternalBus**: App-to-server communication (server Dispatcher)
- **SpreadsheetContextBus**: Decorator wrapping either bus, auto-attaches ClientId/WorkspaceId/WorkbookContextId
- **NullBus**: Replay safety — prevents re-publishing during aggregate reconstruction
- **Envelope commands**: WorkspaceCommandEnvelope and ModelCommandEnvelope abstract routing from UI

## Message Flow

1. ViewModel sends `SpreadSheetMsgs.*` via ExternalBus
2. SpreadsheetContextService routes by metadata to correct SpreadsheetContext
3. SpreadsheetContext translates UI gesture into domain command
4. QueuedSubscriber service loads aggregate, calls method, raises events
5. Read models project domain events into denormalized views
6. UIBehavior read models update SourceCache → ViewModels update via ReactiveUI

## Namespace Boundaries

| Layer | Namespace | Direction |
|-------|-----------|-----------|
| Domain | `ModelServer.Messages` | Internal to domain |
| ACL | `SpreadsheetAdapter.Messages` | Bridge layer |
| UI | `PowerModels.UIBehavior.Messages` | UI-to-ACL only |

## Implementation Reference

Full bus topology and message routing in `implementation/message-bus-topology.md` and `implementation/message-map.md`.
