# OpenStream — Phase 02 Architecture Specification

**Phase:** 02 — Architecture Foundation
**Status:** Planned / Next
**Previous Phase:** 01 — Project Foundation — Complete
**Platform:** Android first, cross-platform-ready Flutter architecture
**Framework:** Flutter + Dart
**Project:** OpenStream

---

## 1. Phase Objective

Establish and document the complete technical architecture of OpenStream before implementing substantial product features.

Phase 02 must answer:

* How the application is structured
* How features communicate
* Where business logic lives
* How state is managed
* How data is stored
* How networking works
* How errors are represented
* How dependencies are injected
* How extensions integrate with OpenStream
* How playback integrates with the application
* How caching works
* How connectivity is handled
* How platform-specific functionality is isolated
* How the architecture remains testable
* How the architecture remains suitable for future iOS and other Flutter platforms

Phase 02 establishes **contracts and boundaries**, not feature implementation.

---

# 2. Architectural Principles

OpenStream architecture must follow:

### 2.1 Stability First

Architecture decisions must prioritize long-term reliability over implementation speed.

### 2.2 Separation of Concerns

UI, business logic, data access, networking, storage, extensions and platform integration must have clear responsibilities.

### 2.3 Dependency Direction

Higher-level business logic must not depend directly on platform-specific implementations.

### 2.4 Testability

Important business logic must be testable without requiring:

* Android devices
* network access
* real extensions
* real media streams
* real databases

### 2.5 Replaceability

Major infrastructure components should be replaceable without rewriting the entire application.

Examples:

```text
Database
Networking client
State management solution
Player implementation
Cache implementation
Secure storage
```

### 2.6 Cross-Platform Readiness

Android is the current target.

However, avoid unnecessary Android-specific dependencies in:

* Domain
* Models
* Repository contracts
* Aggregation
* Extension contracts
* Business logic

Platform-specific implementations must be isolated.

---

# 3. Proposed Architectural Model

The architecture should be based on clear boundaries:

```text
Presentation
      ↓
Application / Use Cases
      ↓
Domain
      ↓
Repository Contracts
      ↓
Data / Infrastructure
      ↓
External Systems
```

A more complete flow:

```text
UI
 │
 ▼
Feature State / Controller
 │
 ▼
Use Case
 │
 ▼
Domain Repository
 │
 ├──────────────┐
 ▼              ▼
Local Data    Remote Data
 │              │
 ▼              ▼
Database       Network
 │              │
 └──────┬───────┘
        ▼
      Domain
```

The exact state-management and dependency-injection technologies remain **TBD until evaluated**.

---

# 4. Application Layers

## 4.1 Presentation Layer

Responsible for:

* Screens
* Widgets
* UI state
* User interaction
* Navigation presentation
* Loading states
* Empty states
* Error states
* Offline states
* Accessibility

Presentation must NOT directly:

* call HTTP clients
* access database implementations
* resolve extensions
* manipulate files
* contain business rules

---

## 4.2 Application Layer

Responsible for coordinating application actions.

Examples:

```text
SearchContent
LoadHome
LoadDetails
PlayContent
AddToWatchlist
RemoveFromWatchlist
RecordHistory
DownloadContent
RestoreBackup
UpdateExtensions
```

This layer coordinates domain operations but should not contain UI code.

---

## 4.3 Domain Layer

Contains stable business concepts and contracts.

Examples:

```text
Movie
TvShow
Season
Episode
Anime
LiveChannel
Stream
Subtitle
AudioTrack
Quality
WatchHistory
WatchlistItem
Bookmark
Download
Extension
ExtensionRepository
```

Domain should not depend on Flutter UI or Android APIs.

---

## 4.4 Data / Infrastructure Layer

Responsible for:

* API communication
* Database implementation
* Cache
* File storage
* Extension persistence
* Repository implementations
* Serialization
* Mapping external data into domain models

---

## 4.5 Platform Layer

Contains platform-specific functionality where required.

Examples:

```text
Android playback integration
Android download services
Biometric authentication
File picker
Notifications
PiP
Background execution
Secure platform storage
```

Platform-specific code must remain isolated behind interfaces where practical.

---

# 5. Feature Architecture

Features should be organized by capability rather than creating one enormous application-wide controller.

Target structure:

```text
lib/
├── app/
├── core/
├── domain/
├── data/
└── features/
    ├── home/
    ├── search/
    ├── details/
    ├── library/
    ├── player/
    ├── downloads/
    ├── extensions/
    ├── settings/
    ├── onboarding/
    ├── backup/
    └── security/
```

This structure is **proposed during Phase 02 and must be formally reviewed before implementation**.

---

# 6. Core Layer

The core layer should contain reusable infrastructure and utilities.

Potential areas:

```text
core/
├── error/
├── network/
├── storage/
├── connectivity/
├── logging/
├── configuration/
├── result/
├── utils/
└── platform/
```

Rules:

* Core must not become a dumping ground.
* Feature-specific business logic belongs inside the relevant feature/domain area.
* Utilities must have a clear reason to exist.

---

# 7. State Management Architecture

A state-management solution must be evaluated before selection.

Evaluation criteria:

* Flutter compatibility
* Dart compatibility
* Null safety
* Maintainability
* Testing
* Lifecycle handling
* Async support
* Cancellation
* Dependency management
* Performance
* Error handling
* Community/maintenance
* Cross-platform support
* Complexity
* Long-term suitability

The selected solution must support:

```text
Loading
Success
Empty
Error
Offline
Refreshing
Partial success
Cancellation
Retry
```

State must not contain unnecessary business logic.

The chosen technology must be documented in:

```text
TECH_STACK.md
docs/decisions/
docs/architecture/state-management.md
```

---

# 8. Dependency Injection

Dependency injection must provide:

* Clear dependency ownership
* Testability
* Replaceable implementations
* Lifecycle management
* Minimal global state
* Easy mocking/fakes

Avoid service-locator abuse.

Dependency direction must remain explicit.

The DI technology is **TBD** until evaluated.

---

# 9. Repository Architecture

Repositories abstract data access from business logic.

Example:

```text
ContentRepository
 ├── search()
 ├── getDetails()
 ├── getHome()
 └── getEpisodes()

LibraryRepository
 ├── getWatchlist()
 ├── addWatchlist()
 ├── removeWatchlist()
 └── history()

ExtensionRepository
 ├── getExtensions()
 ├── install()
 ├── update()
 └── remove()

DownloadRepository
 ├── start()
 ├── pause()
 ├── cancel()
 └── getDownloads()
```

Repository interfaces belong at the appropriate abstraction/domain boundary.

Implementations belong to data/infrastructure.

---

# 10. Result & Error Architecture

OpenStream must avoid uncontrolled exceptions crossing application boundaries.

Define a consistent error/result strategy.

Potential categories:

```text
NetworkError
TimeoutError
AuthenticationError
ParsingError
StorageError
ExtensionError
PlaybackError
DownloadError
ValidationError
UnknownError
```

Errors should contain enough information for:

* UI messaging
* logging
* debugging
* retry decisions
* analytics if introduced later

Sensitive information must never be exposed through user-facing errors or logs.

---

# 11. Networking Architecture

Networking must support:

* Request timeout
* Connection timeout
* Cancellation
* Retry where appropriate
* HTTP error handling
* Response validation
* JSON parsing
* Logging with sensitive-data protection
* Connectivity awareness
* Cache integration

Network operations must never block the UI thread.

Network clients must remain behind abstractions where appropriate.

The HTTP package is **TBD until Phase 02 evaluation**.

---

# 12. Connectivity Architecture

OpenStream needs a centralized connectivity abstraction.

Possible states:

```text
Online
Offline
Unknown
```

Connectivity must not be treated as proof that a particular server/API is reachable.

Offline behavior must be controlled by the feature/data layer.

---

# 13. Storage Architecture

Storage must be divided according to data type.

### Structured persistent data

Examples:

```text
Watch history
Watchlist
Bookmarks
Continue Watching
Content metadata
Extension metadata
Download metadata
Settings requiring structured persistence
```

### Preferences

Examples:

```text
Theme
Language
Playback preferences
Onboarding state
Feature preferences
```

### Sensitive data

Examples:

```text
PIN-related secrets
Authentication credentials if ever required
Security keys/tokens
```

Sensitive data must use appropriate secure storage.

### Files

Examples:

```text
Downloaded media
Backup files
Extension files
Logs
Cached assets
```

Database, key-value storage, secure storage and file storage must be treated as separate concerns.

Storage technologies remain **TBD until evaluated**.

---

# 14. Database Requirements

The selected database must support:

* Strong typing
* Migrations
* Transactions
* Queries
* Indexing
* Offline access
* Corruption handling
* Testing
* Future schema evolution
* Reasonable performance for large libraries

Database schema must be versioned.

Migrations must be tested.

---

# 15. Cache Architecture

Caching should exist at appropriate levels:

```text
Network response cache
Metadata cache
Image cache
Extension metadata cache
Search cache
Home cache
```

Cache rules must define:

* TTL
* invalidation
* stale data behavior
* offline behavior
* maximum size
* corruption recovery

Cache must never silently replace authoritative data where freshness matters.

---

# 16. Content Architecture

Content models must support:

```text
Movie
TV Show
Anime
Season
Episode
Live TV
Person
Genre
Artwork
Metadata
```

Models should distinguish:

```text
Domain entity
External API model
Database model
Extension model
```

Do not directly expose external API response models throughout the application.

Mapping must occur at appropriate boundaries.

---

# 17. Search Architecture

Search must support multiple extensions/providers.

Target flow:

```text
Search UI
 ↓
Search State
 ↓
Search Use Case
 ↓
Aggregation Engine
 ↓
┌─────────────┬─────────────┬─────────────┐
│ Extension A │ Extension B │ Extension C │
└─────────────┴─────────────┴─────────────┘
 ↓
Validation
 ↓
Deduplication
 ↓
Aggregation
 ↓
Progressive results
 ↓
UI
```

Requirements:

* Parallel execution
* Cancellation
* Timeout
* Partial failures
* Deduplication
* Progressive results
* Consistent result model
* Extension isolation

One failed provider must not terminate the complete search.

---

# 18. Home Aggregation Architecture

Home must support rows from multiple sources.

Potential row types:

```text
Trending
Recently Added
Top Rated
Popular
Anime
Live
Continue Watching
AI Recommendations
Extension-specific rows
```

Aggregation must support:

* Partial failure
* Deduplication where applicable
* Ordering
* Source attribution where useful
* Progressive loading
* Caching
* Refresh

---

# 19. Extension Architecture

This is a **critical architectural subsystem**.

OpenStream must NOT simply copy CloudStream's plugin implementation.

The extension architecture must be independently designed for Flutter/Dart.

Extensions should be treated as untrusted.

Potential capabilities:

```text
Search
Home
Details
Episodes
Streams
Subtitles
Metadata
```

The architecture must define:

* Extension manifest
* Extension identity
* Version
* Capabilities
* Compatibility
* Lifecycle
* Installation
* Enable/disable
* Updates
* Rollback
* Failure isolation
* Timeout
* Cancellation
* Validation
* Repository management

The exact runtime mechanism is **TBD**.

Phase 02 must determine what is technically safe and practical.

---

# 20. Extension Security Requirements

Extensions must not automatically receive unrestricted access to:

* User data
* Credentials
* Private files
* Application secrets
* Arbitrary sensitive APIs

Extension operations must be:

```text
Validated
Bounded
Timeout-controlled
Cancellable
Logged safely
Isolated
```

A broken extension must not crash OpenStream.

---

# 21. Player Architecture

The player must be UI-independent.

Target architecture:

```text
Content
 ↓
Playback Request
 ↓
Stream Resolver
 ↓
Resolved Stream
 ↓
Playback Session
 ↓
Media Engine
 ↓
Playback State
 ↓
Library/History
```

Player must support future:

* Video playback
* Audio tracks
* Subtitles
* Quality selection
* Resume
* Seeking
* Auto-next
* Picture-in-picture
* Playback speed
* Fullscreen
* Orientation
* Lifecycle handling
* Network recovery

Media technology remains **TBD until evaluated**.

---

# 22. Playback State

Playback state should represent things such as:

```text
Idle
Loading
Ready
Playing
Paused
Buffering
Seeking
Completed
Error
Stopped
```

Playback progress should be persisted appropriately without excessive database writes.

---

# 23. Downloads Architecture

Downloads must be designed separately from streaming playback.

Requirements:

```text
Queue
Progress
Pause
Resume
Cancel
Retry
Failure
Storage management
Metadata
File integrity
Background operation
```

Download state must survive application restarts where practical.

---

# 24. Offline Architecture

Offline mode should not simply mean "network unavailable."

Offline behavior should explicitly determine:

* What metadata is cached
* What can be displayed
* What downloaded media can play
* What actions are disabled
* How synchronization occurs after reconnecting

Offline states must be designed feature-by-feature.

---

# 25. Backup & Restore

Backup architecture must define a versioned schema.

Backup should eventually include appropriate:

```text
Watch history
Continue Watching
Watchlist
Bookmarks
Settings/preferences
Relevant library metadata
Download metadata
```

Do not blindly export sensitive credentials or secrets.

Restore must:

```text
Read
Validate
Version-check
Migrate if necessary
Preview/confirm
Restore
Report failures
```

---

# 26. Security Architecture

Security must cover:

* Sensitive storage
* App lock
* PIN
* Biometrics
* Extension isolation
* Network security
* Logging
* Backup protection
* Local files
* Downloaded content
* Secrets

Never log:

```text
Passwords
PINs
Tokens
Credentials
Private keys
Sensitive URLs containing secrets
```

---

# 27. Logging Architecture

Logging must support:

```text
Debug
Info
Warning
Error
Critical
```

Logs should include useful context without exposing secrets.

Production logging must be controlled.

Users should eventually be able to export diagnostic logs for bug reports.

---

# 28. Configuration Architecture

Application configuration must not be scattered throughout the codebase.

Configuration should distinguish:

```text
Build configuration
Runtime configuration
User preferences
Feature flags
Extension configuration
```

No unnecessary hardcoded values.

---

# 29. Navigation Architecture

Navigation must eventually support:

```text
Home
Movies
TV
Anime
Live TV
Search
Details
Player
Library
Extensions
Settings
Onboarding
```

Navigation should support:

* Deep links where appropriate
* Back navigation
* Player fullscreen
* State restoration where appropriate
* Invalid-route handling

Navigation technology remains subject to Phase 02 evaluation.

---

# 30. Testing Architecture

Architecture must support:

### Unit tests

Domain/use cases/mappers/state logic.

### Repository tests

Data and repository behavior.

### Integration tests

Major application flows.

### Widget tests

Critical UI behavior.

### End-to-end tests

Critical user journeys.

Testing must support fake/mock implementations of:

```text
Network
Database
Extensions
Player
File system
Connectivity
```

---

# 31. Performance Architecture

Performance requirements must be considered from the beginning.

Avoid:

* unnecessary rebuilds
* unbounded lists
* excessive memory retention
* blocking operations
* unnecessary network calls
* unnecessary database writes
* loading huge media metadata sets at once

Large datasets should support:

```text
Pagination
Lazy loading
Incremental processing
Caching
Cancellation
```

---

# 32. Lifecycle & Cancellation

All long-running operations must consider:

* Widget disposal
* Screen changes
* App backgrounding
* App termination
* Network changes
* User cancellation

No operation should continue indefinitely after its owner no longer needs it.

---

# 33. Dependency Rules

Dependencies must flow toward stable abstractions.

Rules:

```text
UI → Application/Domain
Domain → abstractions
Data → Domain abstractions
Platform → defined interfaces
```

Avoid:

```text
UI → Database directly
UI → HTTP client directly
Domain → Flutter UI
Domain → Android APIs
Extension → unrestricted application internals
```

---

# 34. Dependency Evaluation

Every new package must be evaluated for:

* Flutter compatibility
* Dart compatibility
* Android compatibility
* Future iOS compatibility
* Maintenance activity
* License
* Security
* Community adoption
* API stability
* Performance
* Package size
* Native alternatives
* Long-term replacement difficulty

No dependency is approved simply because it is popular.

---

# 35. Architecture Decision Records

Important decisions must create ADRs.

Examples:

```text
ADR-001 Architecture approach
ADR-002 State management
ADR-003 Database
ADR-004 Networking
ADR-005 Dependency injection
ADR-006 Player technology
ADR-007 Extension runtime
ADR-008 Storage strategy
ADR-009 Navigation
ADR-010 Error handling
```

Each ADR should contain:

```text
Context
Problem
Options considered
Decision
Reason
Consequences
Alternatives rejected
```

---

# 36. Phase 02 Deliverables

Phase 02 is complete only when the following exist and are internally consistent:

```text
ARCHITECTURE.md
TECH_STACK.md

docs/architecture/system-architecture.md
docs/architecture/application-layers.md
docs/architecture/data-flow.md
docs/architecture/state-management.md
docs/architecture/storage-architecture.md
docs/architecture/networking-architecture.md
docs/architecture/player-architecture.md
docs/architecture/extension-architecture.md

docs/decisions/ADR-*.md

docs/phases/phase-02.md
```

---

# 37. Phase 02 Must NOT Implement

Phase 02 must NOT implement:

* Home UI
* Search UI
* Details UI
* Library UI
* Player UI
* Downloads UI
* Extensions UI
* Onboarding
* Settings UI
* Actual extension repositories
* Actual streaming providers
* Production player
* Download engine

Small architecture validation/prototype code may be created **only when necessary to prove a technical decision**, and must be clearly identified as validation rather than product implementation.

---

# 38. Phase 02 Acceptance Criteria

Phase 02 passes only when:

### Architecture

* [ ] Layer boundaries documented
* [ ] Dependency direction documented
* [ ] Feature structure approved
* [ ] Domain boundaries defined
* [ ] Repository strategy defined
* [ ] Error strategy defined
* [ ] Lifecycle strategy defined

### Technology

* [ ] State management evaluated
* [ ] Database evaluated
* [ ] Networking evaluated
* [ ] DI evaluated
* [ ] Storage evaluated
* [ ] Player evaluated
* [ ] Navigation evaluated
* [ ] Extension architecture evaluated

### Extensions

* [ ] Extension trust model defined
* [ ] Extension lifecycle defined
* [ ] Extension capabilities defined
* [ ] Failure isolation defined
* [ ] Update/rollback model defined

### Testing

* [ ] Testing architecture defined
* [ ] Mock/fake boundaries defined
* [ ] Integration boundaries defined

### Cross-platform

* [ ] Platform boundaries identified
* [ ] Android-specific areas isolated
* [ ] Future iOS implications documented

### Documentation

* [ ] Architecture documents updated
* [ ] TECH_STACK updated
* [ ] ADRs created
* [ ] Phase 02 documented
* [ ] ROADMAP updated

### Quality Gate

* [ ] No undocumented architectural decisions
* [ ] No invented implementation claims
* [ ] No unnecessary dependencies
* [ ] No contradictions between documents
* [ ] Architecture reviewed before implementation

---

# 39. Phase 02 Final Rule

**Do not start feature development merely because the architecture document exists.**

The architecture must first be:

```text
Designed
↓
Evaluated
↓
Documented
↓
Reviewed
↓
Approved
↓
Then implemented
```

---