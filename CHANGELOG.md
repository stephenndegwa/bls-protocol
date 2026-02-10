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

- [1.0.1]: https://github.com/bls-protocol/bls-protocol/releases/tag/v1.0.1
- [1.0.0]: https://github.com/bls-protocol/bls-protocol/releases/tag/v1.0.0
- [Unreleased]: https://github.com/bls-protocol/bls-protocol/compare/v1.0.1...HEAD
