# Changelog

All notable changes to the BLS Protocol will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Planned
- Visual BLS editor
- VS Code extension
- CLI tool with code generation
- BLS registry service

---

## [1.0.3] - 2025-02-12

### Added (Resolves final production-scale gaps from expert review)
- **Section 15: Side Effect Formalization**
  - Side effect taxonomy and explicit side-effect declarations
  - Idempotency and retry-safety rules + static checks
  - Compensation modeling and completeness checks

- **Section 16: Formal Action Registry**
  - Action catalog format defining action contracts (inputs/outputs, determinism, failure modes)
  - Catalog-based validation for action references, params, outputs, and side-effect consistency

- **Section 17: Partial Evaluation & Simulation Engine**
  - Simulation specification and output trace format
  - Interactive playground and batch simulation patterns
  - CI/CD simulation guidance

- **Section 18: Multi-BLS System Composition**
  - System graph specification for end-to-end products
  - System-level constraints, scenarios, and validation rules

- **Section 19: Human Approval Protocol**
  - Approval schema, cryptographic signature computation, and validation rules
  - Quality gate enforcement for approved specs

---

## [1.0.1] - 2025-02-10

### Added
- **Validation Framework** - Complete BLS validation system
  - Schema validation
  - Completeness checks
  - PUD-to-BLS traceability matrix
  - Consistency validation
  
- **AI Consumption Guidelines** - Detailed instructions for AI agents
  - Step-by-step implementation guide
  - Common pitfalls to avoid
  - Implementation checklist
  - Metadata requirements
  
- **Discovery-to-BLS Mapping** - Clear mapping from PUD to BLS
  - Conversion rules
  - Completeness matrix
  - Traceability requirements
  
- **Change Management** - Comprehensive change handling
  - Semantic versioning rules
  - Change impact analysis format
  - Deprecation strategy
  - Migration guides
  
- **Dependencies & Composition** - BLS file relationships
  - Import mechanism
  - Shared BLS libraries
  - Composition patterns (inheritance, composition)
  
- **Domain Extensions** - Industry-specific extensions
  - Healthcare (HIPAA)
  - Finance (PCI-DSS)
  - E-commerce
  
- **Observability** - Monitoring and logging specifications
  - Logging requirements
  - Metrics & monitoring
  - Distributed tracing
  
- **Security & Compliance** - Security specifications
  - Authentication/authorization requirements
  - Input validation
  - Compliance specifications (GDPR, HIPAA, TCPA)
  - Audit requirements
  
- **Performance Requirements** - Performance specifications
  - Response time targets
  - Throughput requirements
  - Scalability specifications
  
- **Error Handling** - Comprehensive error management
  - Error classification
  - Failure modes
  - Recovery strategies
  
- **Quality Gates** - Pre/post implementation validation
  - Pre-implementation gates
  - Post-implementation gates
  - Quality criteria

### Changed
- Enhanced specification structure with lifecycle fields
- Improved traceability with source_pud and requirements fields
- Expanded metadata section

### Documentation
- Complete BLS Protocol v1.0.1 specification
- AI implementation guide
- Contributing guidelines
- Examples directory structure

---

## [1.0.0] - 2025-02-03

### Added
- **Core BLS Specification**
  - Top-level structure
  - Input/Output specifications
  - Constraint definitions
  - Test case format
  
- **Six Logic Types**
  - Decision Policy
  - Workflow
  - State Machine
  - Decision Tree
  - Event Handler
  - Validation Rules
  
- **Expression Language**
  - Safe, sandboxed syntax
  - Standard operators (comparison, logical, membership, string)
  - Built-in functions
  - Variable references
  - Safety guarantees
  
- **JSON Schemas**
  - Core schema
  - Schemas for all 6 logic types
  - Extension schemas
  
- **Documentation**
  - Getting started guide
  - Tutorial series
  - API reference
  - Migration guide from code-first approaches
  
- **Examples**
  - VoiceFlow AI (complete working example)
  - Provider selection
  - Lead qualification
  - Order processing workflow
  
- **Tooling Specification**
  - Validator requirements
  - Visualizer requirements
  - Parser requirements
  - Executor requirements
  
### Core Principles Established
- "Business logic is not coded directly. It is discovered, modeled, validated, and then implemented."
- BLS as specification, not code generation
- Language-agnostic format
- AI-readable documentation
- Human-reviewable logic

---

## [0.9.0] - 2025-01-15 (Internal Alpha)

### Added
- Initial concept and prototype
- Basic YAML schema
- Decision policy proof of concept
- Internal testing with sample projects

---

## Version Numbering

**Format:** MAJOR.MINOR.PATCH

**MAJOR:** Breaking changes
- Different logic outcomes for same inputs
- Removed inputs/outputs
- Changed constraint from warning → error
- Incompatible schema changes

**MINOR:** Backward-compatible additions
- New logic types
- New fields (optional)
- New domain extensions
- Enhanced features

**PATCH:** Bug fixes and clarifications
- Documentation improvements
- Schema clarifications
- Example corrections
- Non-breaking fixes

---

## Links

- [1.0.3]: https://github.com/stephenndegwa/bls-protocol/releases/tag/v1.0.3
- [1.0.2]: https://github.com/stephenndegwa/bls-protocol/releases/tag/v1.0.2
- [1.0.1]: https://github.com/stephenndegwa/bls-protocol/releases/tag/v1.0.1
- [1.0.0]: https://github.com/stephenndegwa/bls-protocol/releases/tag/v1.0.0
- [Unreleased]: https://github.com/stephenndegwa/bls-protocol/compare/v1.0.3...HEAD

---

## [1.0.2] - 2025-02-10

### Added (Resolves 10 critical gaps from expert review)

- **Section 5: Execution Semantics** — Formal contracts for all 6 logic types
  - Decision policy: first_match algorithm with precise evaluation order
  - Workflow: sequential/parallel with termination guarantees
  - State machine: event handling with ambiguity rules
  - Error taxonomy: EvaluationError, ValidationError, TerminationError, etc.

- **Section 6: Determinism Guarantees** — Cross-language behavioral consistency
  - Canonical evaluation rules for integers, floats, datetime, strings, nulls
  - Determinism profiles (strict | relaxed | none)
  - RE2-only regex requirement
  - IEEE 754 double precision standard

- **Section 7: Logic Completeness Proof Model** — Static analysis algorithms
  - PRIORITY_UNIQUE: detect duplicate priorities
  - REACHABILITY_ANALYSIS: detect shadowed rules
  - OVERLAP_DETECTION: detect rule conflicts
  - WORKFLOW_TERMINATION: prove workflows terminate
  - STATE_REACHABILITY: prove all states reachable
  - TRANSITION_AMBIGUITY: detect ambiguous state transitions
  - LIVENESS_CHECK: prove all paths reach terminal state
  - Completeness score: must be 100% to pass quality gate

- **Section 8: Versioned Execution Contract** — Engine compatibility matrix
  - BLS version ↔ engine version compatibility rules
  - Backward compatibility guarantees per semantic version level
  - Enforcement algorithm on BLS load

- **Section 9: Canonical Intermediate Representation (IR)**
  - Normalized AST format (JSON)
  - Deterministic IR generation algorithm
  - SHA-256 checksum for drift detection
  - AI agents should consume IR, not raw YAML

- **Section 10: Runtime Separation Model** — Four formal layers
  - Layer 1: Specification (BLS)
  - Layer 2: Validation
  - Layer 3: Execution Engine (generated code)
  - Layer 4: Runtime Infrastructure
  - Formal boundary rules and constraint enforcement model

- **Section 11: Formal Change Safety Guarantees** — Behavioral diffing
  - Change type taxonomy: BEHAVIORAL_BREAKING, STRUCTURAL_BREAKING, ADDITIVE, CLARIFICATION
  - BEHAVIORAL_DIFF algorithm comparing IR versions
  - Safe change workflow

- **Section 12: Logic Composability Guarantees** — Composition rules
  - Import types: reference | inline | extends
  - Merge precedence rules (explicit override required)
  - C3 linearization for diamond dependencies
  - Circular dependency detection algorithm

- **Section 13: Formal State Isolation Model**
  - State model declaration: stateless | stateful_immutable | stateful_mutable
  - Transaction model with compensation steps (saga pattern)
  - Event sourcing vs imperative execution semantics

- **Section 14: AI Drift Control Layer**
  - Conformance Test Suite (CTS): auto-generated from BLS
  - Conformance validation process
  - Conformance report format
  - Mandatory AI implementation constraints
  - Drift indicators

### Changed
- Quality gates updated to require conformance validation
- AI consumption guidelines updated with drift prevention instructions
- Expression language: RE2 requirement now mandatory (was recommended)
- Constraint enforce_at now has static analysis implications

### Migration
- v1.0.2 is backward compatible with v1.0.1
- New required fields emit warnings, not errors, until added
- See Appendix A for migration guide
