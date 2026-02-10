# Business Logic Specification (BLS) Protocol v1.0.1

## Official Specification Document

**Version:** 1.0.1  
**Status:** Production  
**Last Updated:** 2025-02-10  
**Authors:** Autonomous Coding Systems Initiative  
**License:** Apache 2.0

---

## Core Principles

### The Foundation

**Business logic is not coded directly. It is discovered, modeled, validated, and then implemented.**

This protocol embodies this principle through:

1. **Discovery First** - Logic must be fully understood before specification
2. **Modeling Second** - Logic is formally modeled in BLS
3. **Validation Third** - BLS is validated for completeness and correctness
4. **Implementation Last** - AI agents read BLS to generate code

### BLS is Specification, Not Code

```
BLS = Documentation (that AI can read)
BLS ≠ Code generation tool
BLS ≠ Execution engine

BLS files are:
✓ Human-readable specifications
✓ AI-readable references
✓ Single source of truth for logic
✓ Version-controlled documentation

BLS files are NOT:
✗ Code that executes
✗ Tools that generate code
✗ Tied to any language
```

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Core Schema](#2-core-schema)
3. [Logic Types](#3-logic-types)
4. [Expression Language](#4-expression-language)
5. [Validation Framework](#5-validation-framework)
6. [AI Consumption Guidelines](#6-ai-consumption-guidelines)
7. [Discovery-to-BLS Mapping](#7-discovery-to-bls-mapping)
8. [Change Management](#8-change-management)
9. [Dependencies & Composition](#9-dependencies--composition)
10. [Domain Extensions](#10-domain-extensions)
11. [Observability](#11-observability)
12. [Security & Compliance](#12-security--compliance)
13. [Performance Requirements](#13-performance-requirements)
14. [Error Handling](#14-error-handling)
15. [Quality Gates](#15-quality-gates)
16. [Tooling Ecosystem](#16-tooling-ecosystem)

---

## 1. Introduction

### 1.1 What is BLS?

BLS (Business Logic Specification) is a **structured documentation format** for describing business logic in a way that:

- **Humans** can read and understand
- **AI agents** can read and implement
- **Teams** can review and approve
- **Systems** can validate and verify

**Analogy:**
```
OpenAPI/Swagger : APIs :: BLS : Business Logic
Database Schema : Data :: BLS : Logic
Architecture Blueprints : Buildings :: BLS : Software Logic
```

### 1.2 What BLS Solves

**Problem:** Business logic is typically:
- Scattered across codebases
- Buried in implementation details
- Hard to change without breaking things
- Impossible to review without coding knowledge
- Different across services (drift)

**Solution:** BLS provides:
- Single source of truth for logic
- Clear, reviewable specifications
- Language-agnostic documentation
- AI-readable format for implementation
- Trackable changes via version control

### 1.3 The BLS Workflow

```
Phase 1: DISCOVERY
├─ Business analyst/AI asks questions
├─ Requirements gathered (PUD)
└─ Complete understanding achieved

Phase 2: MODELING
├─ Read requirements (PUD)
├─ Model logic formally
├─ Write BLS specifications
└─ Validate completeness

Phase 3: VALIDATION
├─ Review BLS for correctness
├─ Check constraints satisfied
├─ Verify test coverage
└─ Get stakeholder approval

Phase 4: IMPLEMENTATION
├─ AI agent (Claude Code, etc.) reads BLS
├─ AI generates code in target language
├─ AI writes tests from BLS test cases
└─ Code deployed

Phase 5: EVOLUTION
├─ Business logic changes
├─ Update BLS specification
├─ AI regenerates implementation
└─ Deploy changes
```

### 1.4 File Format

**Extension:** `.bls.yaml` or `.bls.json`  
**Encoding:** UTF-8  
**Format:** YAML or JSON  
**MIME Type:** `application/vnd.bls+yaml` or `application/vnd.bls+json`

---

## 2. Core Schema

### 2.1 Top-Level Structure

```yaml
bls_version: "1.0.1"

specification:
  # Identity
  id: string                    # Unique identifier (kebab-case)
  version: string               # Semantic version (x.y.z)
  name: string                  # Human-readable name
  description: string           # What this logic does
  
  # Classification
  domain: string                # Business domain
  logic_type: enum              # Type of logic (see section 3)
  category: string              # Optional subcategory
  tags: array<string>           # Searchable tags
  
  # Lifecycle
  status: enum                  # draft | active | deprecated
  created_at: datetime
  updated_at: datetime
  deprecated_at: datetime       # If deprecated
  replacement: string           # BLS ID that replaces this
  
  # Ownership
  owner: string                 # Team/person responsible
  maintainer: string
  reviewers: array<string>
  
  # Traceability
  source_pud: string            # Link to PUD document
  requirements: array<string>   # Requirement IDs from PUD
  
  # Definition
  inputs: array<Input>
  outputs: array<Output>
  logic: object                 # Type-specific (see section 3)
  
  # Quality
  constraints: array<Constraint>
  test_cases: array<TestCase>
  validation_rules: array<ValidationRule>
  
  # Documentation
  examples: array<Example>
  notes: string
  related_bls: array<string>    # Related BLS IDs
  
  # Non-functional Requirements
  performance: PerformanceSpec
  security: SecuritySpec
  observability: ObservabilitySpec
  
  # Metadata
  metadata: object
```

### 2.2 Input Specification

```yaml
Input:
  name: string                  # Parameter name
  type: enum                    # string | number | boolean | array | object | date | datetime | enum
  description: string
  required: boolean             # Default: true
  default: any                  # Default value if not required
  
  # Validation
  validation:
    min: number                 # For numbers/arrays
    max: number
    min_length: number          # For strings/arrays
    max_length: number
    pattern: string             # Regex pattern
    enum: array                 # Valid values
    custom: string              # Custom validation expression
    
  # Schema (for object/array types)
  schema: object                # JSON Schema
  
  # Documentation
  examples: array<any>
  notes: string
  
  # Sensitivity
  sensitive: boolean            # PII/confidential data
  pii_type: enum                # email | phone | ssn | credit_card | etc.
```

### 2.3 Output Specification

```yaml
Output:
  name: string
  type: enum
  description: string
  deterministic: boolean        # Same inputs → same outputs?
  
  schema: object                # For complex types
  examples: array<any>
  notes: string
  
  # Side effects
  side_effects: array<SideEffect>
```

### 2.4 Constraint Specification

```yaml
Constraint:
  id: string
  type: enum                    # invariant | precondition | postcondition | business_rule | compliance
  description: string
  expression: string            # BLS expression
  severity: enum                # error | warning | info
  
  # Compliance
  regulation: string            # GDPR | HIPAA | TCPA | etc.
  requirement_id: string
  
  # Enforcement
  enforce_at: enum              # compile_time | runtime | both
  violation_action: enum        # reject | warn | log
```

### 2.5 Test Case Specification

```yaml
TestCase:
  id: string
  name: string
  description: string
  
  # Test definition
  input: object
  expected_output: object
  expected_side_effects: array<SideEffect>
  
  # Classification
  category: enum                # happy_path | edge_case | error_case | performance | security
  priority: enum                # critical | high | medium | low
  
  # Requirements traceability
  requirement_id: string        # From PUD
  
  # Metadata
  tags: array<string>
  notes: string
```

---

## 3. Logic Types

### 3.1 Decision Policy

**Purpose:** Select among options based on rules

**When to use:**
- Provider selection (Twilio vs Vonage)
- Routing decisions (which team handles this?)
- Qualification (is this lead qualified?)
- Pricing tier selection

**Schema:**
```yaml
logic_type: decision_policy

logic:
  evaluation_strategy: enum     # first_match | best_match | all_matches
  
  rules:
    - id: string
      name: string
      description: string
      
      priority: number          # Lower = higher priority
      
      condition:
        type: enum              # expression | always | never
        expression: string      # BLS expression
        
      action:
        type: string
        params: object
        
      metadata:
        reason: string          # Why this rule exists
        source: string          # From PUD section X
        examples: array
        
  default_action:
    type: string
    params: object
    
  # Conflict resolution
  conflict_resolution: enum     # priority | most_specific | custom
```

**Example:**
```yaml
bls_version: "1.0.1"
specification:
  id: "provider-selection"
  version: "1.0.0"
  name: "SMS Provider Selection Policy"
  logic_type: "decision_policy"
  
  inputs:
    - name: "destination_country"
      type: "string"
      description: "ISO 3166-1 alpha-2 country code"
      validation:
        pattern: "^[A-Z]{2}$"
        
  outputs:
    - name: "provider_id"
      type: "string"
    - name: "fallback_chain"
      type: "array"
      
  logic:
    evaluation_strategy: "first_match"
    
    rules:
      - id: "kenya-high-priority"
        priority: 1
        condition:
          type: "expression"
          expression: "destination_country == 'KE' AND priority IN ['high', 'urgent']"
        action:
          type: "select_provider"
          params:
            provider_id: "africas_talking"
            fallback_chain: ["twilio", "vonage"]
        metadata:
          reason: "Africa's Talking has 99.9% delivery rate in Kenya"
          source: "pud.business_rules.contextual_rules[0]"
          
      - id: "usa-routing"
        priority: 10
        condition:
          type: "expression"
          expression: "destination_country == 'US'"
        action:
          type: "select_provider"
          params:
            provider_id: "twilio"
            fallback_chain: ["vonage"]
            
    default_action:
      type: "select_cheapest"
      params:
        calculate_for: "destination_country"
        
  constraints:
    - type: "business_rule"
      description: "MUST have at least one fallback provider"
      expression: "output.fallback_chain.length >= 1"
      severity: "error"
      
  test_cases:
    - name: "kenya_high_priority"
      category: "happy_path"
      input:
        destination_country: "KE"
        priority: "high"
      expected_output:
        provider_id: "africas_talking"
        fallback_chain: ["twilio", "vonage"]
```

### 3.2 Workflow

**Purpose:** Multi-step processes with dependencies

**When to use:**
- Order fulfillment
- Onboarding flows
- Call handling
- Payment processing

**Schema:**
```yaml
logic_type: workflow

logic:
  execution_mode: enum          # sequential | parallel | hybrid
  
  initial_step: string          # Starting step ID
  
  steps:
    - id: string
      name: string
      description: string
      
      action: string
      params: object
      
      # Control flow
      on_success:
        next: string            # Next step ID
        side_effects: array<SideEffect>
        
      on_failure:
        strategy: enum          # halt | continue | retry | goto
        next: string            # Step ID if strategy is goto
        max_retries: number
        retry_delay_ms: number
        
      # Execution
      timeout_ms: number
      async: boolean
      parallel: boolean
      depends_on: array<string> # Step IDs
      
      # Observability
      log_level: enum           # debug | info | warn | error
      emit_metrics: boolean
      
  # Global settings
  overall_timeout_ms: number
  failure_strategy: enum        # fail_fast | continue_on_error
```

### 3.3 State Machine

**Purpose:** State transitions based on events

**Schema:**
```yaml
logic_type: state_machine

logic:
  initial_state: string
  
  states:
    - id: string
      name: string
      type: enum                # normal | final | error
      
      on_enter: array<Action>
      on_exit: array<Action>
      
      # State data
      data_schema: object
      
  transitions:
    - from: string              # State ID
      to: string                # State ID
      event: string
      
      # Guards
      condition: string         # Optional guard condition
      
      # Actions
      actions: array<Action>
      
      # Timing
      timeout_ms: number        # Auto-transition
      
  # Configuration
  allow_self_transitions: boolean
  track_history: boolean
```

### 3.4 Decision Tree

**Purpose:** Hierarchical conditional logic

**Schema:**
```yaml
logic_type: decision_tree

logic:
  root: string                  # Root node ID
  
  max_depth: number             # Prevent infinite trees
  
  nodes:
    - id: string
      name: string
      type: enum                # condition | action | leaf
      
      # If type is condition:
      condition:
        field: string
        operator: enum          # eq | ne | gt | lt | gte | lte | in | contains | matches
        value: any
        
      true_branch: string       # Node ID
      false_branch: string      # Node ID
      
      # If type is action or leaf:
      action:
        type: string
        params: object
        result: any
        
      # Metadata
      description: string
      weight: number            # For weighted decisions
```

### 3.5 Event Handler

**Purpose:** Event-driven reactions

**Schema:**
```yaml
logic_type: event_handler

logic:
  handlers:
    - event: string             # Event name or pattern
      event_pattern: string     # Regex for event matching
      
      # Filtering
      filter: string            # Filter expression
      
      # Priority
      priority: number
      
      # Actions
      actions:
        - type: string
          params: object
          async: boolean
          retry: RetryConfig
          
      # Rate limiting
      debounce_ms: number       # Ignore rapid events
      throttle_ms: number       # Max frequency
      
      # Idempotency
      idempotent: boolean
      idempotency_key: string
```

### 3.6 Validation Rules

**Purpose:** Data correctness constraints

**Schema:**
```yaml
logic_type: validation_rules

logic:
  validation_strategy: enum     # fail_fast | collect_all_errors
  
  rules:
    - field: string             # JSON path
      
      type: enum                # required | type | format | range | pattern | custom | cross_field
      
      validation:
        # Type-specific validation config
        
      error_message: string
      error_code: string
      
      severity: enum            # error | warning
      
      # Conditional validation
      when: string              # Only validate if condition true
```

---

## 4. Expression Language

### 4.1 Syntax

BLS expressions are simple, safe, and deterministic.

**Operators:**
```
Comparison: == != > < >= <=
Logical:    AND OR NOT
Membership: IN NOT_IN
String:     CONTAINS STARTSWITH ENDSWITH MATCHES
Arithmetic: + - * / %
Null:       IS_NULL IS_NOT_NULL
```

**Functions:**
```
String:     upper(s) lower(s) trim(s) length(s)
Array:      len(arr) contains(arr, val) first(arr) last(arr)
Date/Time:  now() today() date(s) hour(dt) day(dt) month(dt) year(dt)
Math:       abs(n) ceil(n) floor(n) round(n) min(...) max(...)
Type:       type(x) is_string(x) is_number(x) is_array(x) is_object(x)
```

**Examples:**
```
# Simple comparison
destination_country == 'KE'

# Logical operators
budget >= 10000 AND timeline_days <= 90

# Membership
industry IN ['healthcare', 'finance', 'technology']

# String operations
message CONTAINS 'urgent' OR message STARTSWITH 'ALERT'

# Functions
len(recipients) > 0 AND len(recipients) <= 10000

# Date/time
hour(schedule_time) >= 8 AND hour(schedule_time) <= 20

# Complex
(is_fortune_500 == true) OR 
(budget >= 50000 AND authority_level IN ['decision_maker', 'budget_holder'])
```

### 4.2 Variable References

```yaml
# Input variables
{{inputs.phone_number}}
{{inputs.workspace.credits_remaining}}

# Previous step outputs (in workflows)
{{validate_number.result.normalized_number}}
{{select_provider.result.provider_id}}

# Context variables (provided at runtime)
{{context.user.id}}
{{context.workspace.plan}}

# Environment
{{env.ENVIRONMENT}}  # development | staging | production
```

### 4.3 Safety Guarantees

BLS expressions MUST:
- ✓ Be sandboxed (no code execution)
- ✓ Be deterministic (same inputs → same outputs)
- ✓ Terminate in finite time (no infinite loops)
- ✓ Handle null/undefined gracefully
- ✓ Have no side effects
- ✓ Be reversible (for debugging)

---

## 5. Validation Framework

### 5.1 BLS Completeness Validation

**Every BLS file MUST pass these checks:**

```yaml
completeness_checks:
  - name: "Schema validation"
    check: "Valid against BLS JSON Schema"
    severity: "error"
    
  - name: "All inputs defined"
    check: "No undefined variables in expressions"
    severity: "error"
    
  - name: "All outputs defined"
    check: "All action results map to outputs"
    severity: "error"
    
  - name: "Default case exists"
    check: "Decision policies have default action"
    severity: "warning"
    
  - name: "All paths terminate"
    check: "Workflows reach final state"
    severity: "error"
    
  - name: "No unreachable logic"
    check: "All rules/steps/nodes are reachable"
    severity: "warning"
    
  - name: "Test coverage"
    check: "At least one test per major path"
    severity: "warning"
    
  - name: "Constraint coverage"
    check: "All constraints have tests"
    severity: "warning"
```

### 5.2 PUD-to-BLS Traceability

**Every BLS file MUST trace to PUD:**

```yaml
traceability_checks:
  - name: "PUD reference exists"
    check: "specification.source_pud is set"
    
  - name: "Requirements mapped"
    check: "All PUD requirements referenced in BLS"
    
  - name: "Business rules covered"
    check: "All PUD business_rules have corresponding BLS logic"
    
  - name: "Edge cases handled"
    check: "All PUD edge_cases have BLS test cases"
    
  - name: "Constraints formalized"
    check: "All PUD constraints appear in BLS constraints"
```

### 5.3 Consistency Validation

**BLS files MUST be internally consistent:**

```yaml
consistency_checks:
  - name: "No contradictory rules"
    check: "Rules don't conflict (same conditions, different actions)"
    
  - name: "Type consistency"
    check: "Outputs match expected schema"
    
  - name: "Constraint satisfaction"
    check: "Test cases satisfy all constraints"
    
  - name: "Version compatibility"
    check: "Dependencies use compatible versions"
```

---

## 6. AI Consumption Guidelines

### 6.1 How AI Agents Should Read BLS

**For AI agents like Claude Code, GPT Engineer, etc.**

#### Step 1: Parse and Validate

```
1. Load BLS file
2. Validate against JSON Schema
3. Check completeness
4. Verify traceability to PUD
5. If invalid → Report errors and STOP
```

#### Step 2: Understand Context

```
Read in this order:
1. specification.description - What is this logic for?
2. specification.domain - What business domain?
3. specification.inputs - What data comes in?
4. specification.outputs - What data goes out?
5. specification.constraints - What MUST be true?
6. specification.logic - How to transform inputs → outputs
```

#### Step 3: Identify Logic Type

```
Based on logic_type, understand the pattern:

decision_policy:
  → Implement as conditional logic (if/else)
  → Respect rule priority
  → Include default case

workflow:
  → Implement as step-by-step process
  → Handle retries, timeouts
  → Include error paths

state_machine:
  → Implement state tracking
  → Handle transitions
  → Respect guards

decision_tree:
  → Implement as nested conditionals
  → Traverse from root
  → Reach leaf nodes

event_handler:
  → Implement as event listeners
  → Handle async execution
  → Include idempotency

validation_rules:
  → Implement as validation functions
  → Collect errors vs fail-fast
  → Return validation results
```

#### Step 4: Generate Implementation

```
For each BLS file:

1. Generate type definitions from inputs/outputs
   → TypeScript: interfaces
   → Python: dataclasses/Pydantic
   → Go: structs
   
2. Generate main logic function
   → Name from specification.id
   → Parameters from inputs
   → Return type from outputs
   
3. Implement logic per logic_type
   → Follow language idioms
   → Add comments referencing BLS
   → Include traceability
   
4. Generate validation
   → From constraints
   → Check before/after execution
   
5. Generate tests
   → From test_cases
   → Include edge cases
   
6. Add observability
   → Logging from observability spec
   → Metrics if specified
```

#### Step 5: Include Metadata

**Every generated file MUST include:**

```typescript
/**
 * Generated from BLS: bls/policies/provider-selection.bls.yaml
 * BLS Version: 1.0.0
 * Generated: 2025-02-10T16:00:00Z
 * 
 * DO NOT EDIT THIS FILE DIRECTLY
 * To modify logic:
 * 1. Edit the BLS file
 * 2. Regenerate with: npm run generate (or tell AI to regenerate)
 * 
 * Description: {specification.description}
 * Owner: {specification.owner}
 */
```

### 6.2 Common Pitfalls for AI

**AI agents MUST avoid:**

❌ **Ignoring constraints** - All constraints MUST be enforced
❌ **Missing edge cases** - Implement ALL test cases
❌ **Changing logic** - Don't "improve" the logic, implement exactly as specified
❌ **Skipping validation** - Always validate inputs per BLS
❌ **Ignoring error handling** - Implement all failure paths
❌ **Missing traceability** - Always link back to BLS
❌ **Hardcoding values** - Use BLS parameters, not magic numbers
❌ **Optimizing prematurely** - Correctness first, optimization later

### 6.3 AI Implementation Checklist

```yaml
ai_implementation_checklist:
  before_coding:
    - [ ] Read entire BLS file
    - [ ] Understand logic type
    - [ ] Review all constraints
    - [ ] Check test cases
    - [ ] Verify traceability to PUD
    
  during_coding:
    - [ ] Follow language best practices
    - [ ] Add BLS traceability comments
    - [ ] Implement ALL constraints
    - [ ] Handle ALL error cases
    - [ ] Include logging/metrics per spec
    
  after_coding:
    - [ ] Generated all test cases
    - [ ] Tests pass
    - [ ] Constraints validated
    - [ ] Code matches BLS exactly
    - [ ] Documentation generated
```

---

## 7. Discovery-to-BLS Mapping

### 7.1 PUD → BLS Conversion Rules

**For BLS Agent creating BLS from PUD:**

```yaml
pud_section → bls_field:
  
  # Identity
  pud.project.id → specification.id
  pud.project.name → specification.name
  pud.project.domain → specification.domain
  
  # Requirements
  pud.problem_statement → specification.description
  pud.stakeholders → specification.owner, reviewers
  
  # Inputs
  pud.inputs_outputs.inputs → specification.inputs
  pud.inputs_outputs.outputs → specification.outputs
  
  # Logic (depends on type)
  pud.business_rules.invariants → specification.constraints
  pud.business_rules.contextual_rules → specification.logic.rules
  pud.business_rules.conflict_resolution → specification.logic.conflict_resolution
  
  # Validation
  pud.edge_cases → specification.test_cases
  pud.business_rules.constraints → specification.constraints
  
  # Requirements
  pud.compliance_requirements → specification.security, constraints
  pud.performance_requirements → specification.performance
```

### 7.2 Completeness Matrix

**BLS Agent MUST ensure:**

```yaml
completeness_matrix:
  all_decisions_covered:
    source: "pud.system_decisions"
    target: "bls logic files"
    check: "Each decision has corresponding BLS file"
    
  all_rules_formalized:
    source: "pud.business_rules"
    target: "bls logic"
    check: "Each rule appears in BLS"
    
  all_edge_cases_tested:
    source: "pud.edge_cases"
    target: "bls test_cases"
    check: "Each edge case has test"
    
  all_constraints_explicit:
    source: "pud.business_rules.constraints"
    target: "bls constraints"
    check: "All constraints formalized"
    
  all_integrations_specified:
    source: "pud.integration_requirements"
    target: "bls dependencies"
    check: "External deps documented"
```

---

## 8. Change Management

### 8.1 Semantic Versioning

```yaml
version: "MAJOR.MINOR.PATCH"

MAJOR: Breaking changes
  - Different logic outcomes for same inputs
  - Removed inputs/outputs
  - Changed constraint from warning → error
  
MINOR: Backward-compatible additions
  - New rules (that don't change existing outcomes)
  - New optional inputs
  - New outputs
  - New test cases
  
PATCH: Bug fixes, clarifications
  - Fixed incorrect constraint
  - Updated documentation
  - Added examples
```

### 8.2 Change Impact Analysis

**Every BLS change MUST include:**

```yaml
change_impact:
  version: "1.2.0"
  previous_version: "1.1.0"
  
  breaking_changes: boolean
  
  changes:
    - type: enum              # added | modified | removed | deprecated
      element: string         # What changed (rule:usa-routing)
      description: string
      reason: string
      
  affected_systems:
    - system: string
      impact: enum            # none | low | medium | high | critical
      action_required: string
      
  migration_guide: string
  rollback_plan: string
```

### 8.3 Deprecation Strategy

```yaml
# Marking logic as deprecated

specification:
  status: "deprecated"
  deprecated_at: "2025-03-01T00:00:00Z"
  replacement: "provider-selection-v2"
  deprecation_reason: "New algorithm improves delivery rates by 15%"
  
  deprecation_timeline:
    - date: "2025-03-01"
      milestone: "Deprecated, both versions active"
    - date: "2025-04-01"
      milestone: "Warning logs added to old version"
    - date: "2025-05-01"
      milestone: "Old version removed"
```

---

## 9. Dependencies & Composition

### 9.1 Importing Other BLS

```yaml
# File: bls/workflows/outbound-call.bls.yaml

specification:
  dependencies:
    - id: "provider-selection"
      version: "^1.2.0"          # Semver range
      path: "../policies/provider-selection.bls.yaml"
      alias: "provider_selector"
      
  logic:
    steps:
      - id: "select_provider"
        action: "execute_bls"
        params:
          bls_id: "provider_selector"
          inputs:
            destination_country: "{{inputs.phone_number.country}}"
```

### 9.2 Shared BLS Libraries

```yaml
# Organizational BLS library

company/
├─ shared-bls/
│  ├─ authentication/
│  │  ├─ session-management.bls.yaml
│  │  └─ rbac-policy.bls.yaml
│  ├─ billing/
│  │  ├─ usage-tracking.bls.yaml
│  │  └─ payment-processing.bls.yaml
│  └─ communication/
│     ├─ provider-selection.bls.yaml
│     └─ webhook-handlers.bls.yaml
│
├─ project-a/
│  └─ bls/
│     ├─ index.yaml
│     └─ dependencies:
│        - "@shared/communication/provider-selection@^1.0.0"
│
└─ project-b/
   └─ bls/
      └─ dependencies:
         - "@shared/authentication/rbac-policy@^2.0.0"
         - "@shared/billing/usage-tracking@^1.0.0"
```

### 9.3 Composition Patterns

**Inheritance:**
```yaml
specification:
  extends: "@shared/base-authentication"
  
  # Override specific rules
  overrides:
    rules:
      - id: "session-timeout"
        params:
          timeout_minutes: 60  # Override from base (30)
```

**Composition:**
```yaml
specification:
  composed_of:
    - bls_id: "validate-input"
    - bls_id: "process-payment"
    - bls_id: "send-confirmation"
```

---

## 10. Domain Extensions

### 10.1 Healthcare Extension

```yaml
bls_version: "1.0.1"
extension: "healthcare"

specification:
  # Additional fields
  hipaa_compliant: boolean
  phi_handling:
    contains_phi: boolean
    phi_fields: array<string>
    encryption_required: boolean
    audit_log_required: boolean
    
  # Healthcare-specific constraints
  constraints:
    - type: "hipaa"
      description: "Must log all PHI access"
      
  # Healthcare-specific test cases
  test_cases:
    - category: "hipaa_compliance"
```

### 10.2 Finance Extension

```yaml
extension: "finance"

specification:
  # PCI-DSS compliance
  pci_compliant: boolean
  handles_payment_data: boolean
  
  # Financial constraints
  constraints:
    - type: "financial_regulation"
      regulation: "PCI-DSS 4.0"
      requirement: "6.5.1"
```

### 10.3 E-commerce Extension

```yaml
extension: "ecommerce"

specification:
  # Cart/Order specific
  shopping_cart_logic: boolean
  inventory_aware: boolean
  
  # Pricing logic
  pricing_rules:
    supports_discounts: boolean
    supports_tax_calculation: boolean
```

---

## 11. Observability

### 11.1 Logging Requirements

```yaml
specification:
  observability:
    logging:
      enabled: boolean
      level: enum             # debug | info | warn | error
      
      log_inputs: boolean
      log_outputs: boolean
      log_intermediate_steps: boolean
      
      # PII handling
      redact_sensitive: boolean
      redact_fields: array<string>
      
      # Structure
      structured_logging: boolean
      log_format: enum        # json | text
      
      # Context
      include_correlation_id: boolean
      include_user_id: boolean
      include_workspace_id: boolean
```

### 11.2 Metrics & Monitoring

```yaml
specification:
  observability:
    metrics:
      enabled: boolean
      
      # Standard metrics
      track_execution_time: boolean
      track_success_rate: boolean
      track_error_rate: boolean
      
      # Custom metrics
      custom_metrics:
        - name: "provider_selection_distribution"
          type: "counter"
          labels: ["provider_id", "country"]
          
        - name: "call_duration_seconds"
          type: "histogram"
          buckets: [30, 60, 120, 300, 600]
```

### 11.3 Tracing

```yaml
specification:
  observability:
    tracing:
      enabled: boolean
      
      # Distributed tracing
      trace_id_propagation: boolean
      
      # Span details
      span_name: string
      span_attributes:
        - key: "bls.id"
          value: "{{specification.id}}"
        - key: "bls.version"
          value: "{{specification.version}}"
```

---

## 12. Security & Compliance

### 12.1 Security Requirements

```yaml
specification:
  security:
    # Authentication
    authentication_required: boolean
    authentication_method: enum   # api_key | jwt | oauth | custom
    
    # Authorization
    authorization_required: boolean
    required_permissions: array<string>
    
    # Input validation
    validate_all_inputs: boolean
    sanitize_inputs: boolean
    
    # Output protection
    sanitize_outputs: boolean
    prevent_injection: boolean
    
    # Rate limiting
    rate_limit:
      enabled: boolean
      requests_per_minute: number
      
    # Encryption
    encrypt_sensitive_data: boolean
    encryption_algorithm: string
```

### 12.2 Compliance Specifications

```yaml
specification:
  compliance:
    regulations:
      - name: "GDPR"
        version: "2016/679"
        requirements:
          - id: "right_to_deletion"
            implemented: boolean
            notes: string
            
      - name: "HIPAA"
        requirements:
          - id: "164.312(a)(1)"
            description: "Access Control"
            
      - name: "TCPA"
        requirements:
          - id: "do_not_call"
            description: "Check DNC list before dialing"
            
    # Audit requirements
    audit:
      enabled: boolean
      log_all_access: boolean
      retention_days: number
      
    # Data retention
    data_retention:
      enabled: boolean
      retention_period_days: number
      deletion_method: enum       # soft_delete | hard_delete | anonymize
```

---

## 13. Performance Requirements

### 13.1 Response Time

```yaml
specification:
  performance:
    # Latency
    response_time:
      target_p50_ms: number
      target_p95_ms: number
      target_p99_ms: number
      max_acceptable_ms: number
      
    # Timeout
    timeout_ms: number
    
    # Caching
    cacheable: boolean
    cache_ttl_seconds: number
    cache_key: string
```

### 13.2 Throughput

```yaml
specification:
  performance:
    # Capacity
    throughput:
      target_requests_per_second: number
      max_requests_per_second: number
      
    # Concurrency
    max_concurrent_executions: number
    
    # Resource limits
    resource_limits:
      max_memory_mb: number
      max_cpu_percent: number
```

### 13.3 Scalability

```yaml
specification:
  performance:
    scalability:
      horizontal_scaling: boolean
      stateless: boolean
      
      # Load characteristics
      expected_daily_volume: number
      expected_peak_volume: number
      peak_time_windows: array<string>
```

---

## 14. Error Handling

### 14.1 Error Classification

```yaml
specification:
  error_handling:
    errors:
      - code: string
        name: string
        category: enum        # validation | business | system | external
        severity: enum        # low | medium | high | critical
        
        # Response
        user_message: string
        technical_message: string
        http_status_code: number
        
        # Recovery
        retryable: boolean
        retry_strategy: enum  # exponential | linear | fixed
        max_retries: number
        
        # Actions
        fallback_action: string
        escalation_required: boolean
```

### 14.2 Failure Modes

```yaml
specification:
  error_handling:
    failure_modes:
      - scenario: string
        probability: enum     # rare | occasional | frequent
        
        detection: string
        handling: string
        recovery: string
        
        # Prevention
        preventive_measures: array<string>
        
        # Testing
        test_case_id: string
```

---

## 15. Quality Gates

### 15.1 Pre-Implementation Gates

**Before AI generates code from BLS:**

```yaml
quality_gates:
  pre_implementation:
    - name: "BLS Validation"
      checks:
        - Schema valid
        - No validation errors
        - Completeness score ≥ 95%
      required: true
      
    - name: "PUD Traceability"
      checks:
        - All requirements mapped
        - All business rules covered
        - All edge cases have tests
      required: true
      
    - name: "Stakeholder Approval"
      checks:
        - Business owner approved
        - Technical reviewer approved
      required: true
```

### 15.2 Post-Implementation Gates

**After code is generated:**

```yaml
quality_gates:
  post_implementation:
    - name: "Code Quality"
      checks:
        - All tests pass
        - Code coverage ≥ 90%
        - No linting errors
        - Security scan passed
      required: true
      
    - name: "BLS Compliance"
      checks:
        - All constraints enforced in code
        - All test cases implemented
        - Traceability comments present
      required: true
      
    - name: "Performance"
      checks:
        - Response time meets SLA
        - Resource usage within limits
      required: false
```

---

## 16. Tooling Ecosystem

### 16.1 Required Tools

**BLS Validator:**
```bash
$ bls validate bls/policies/provider-selection.bls.yaml

✓ Schema valid
✓ Completeness: 100%
✓ Traceability: 100%
✓ Ready for implementation
```

**BLS Differ:**
```bash
$ bls diff v1.0.0 v1.1.0

Changes in provider-selection.bls.yaml:
  Modified: rule:usa-routing
    - provider: "twilio"
    + provider: "vonage"
    
Impact: Medium
Breaking: No
```

**BLS Visualizer:**
```bash
$ bls visualize bls/workflows/outbound-call.bls.yaml

[Generates flowchart/diagram]
```

### 16.2 Recommended Tools

**BLS Editor:**
- Visual editor for creating BLS
- Form-based or diagram-based
- Validates in real-time

**BLS Registry:**
- Central repository of BLS files
- Version management
- Dependency tracking
- Search and discovery

**BLS Merger:**
- Merge BLS changes
- Conflict detection
- Resolution strategies

---

## Appendix A: Complete Example

See: `examples/voiceflow-ai/` for complete working example

---

## Appendix B: Migration Guide

**From BLS v1.0.0 to v1.0.1:**

Added:
- Discovery-to-BLS mapping
- AI consumption guidelines
- Observability specifications
- Security specifications
- Performance requirements
- Quality gates

No breaking changes.

---

## Appendix C: JSON Schemas

Full schemas available at:
- https://bls-protocol.org/schemas/v1.0.1/core.json
- https://bls-protocol.org/schemas/v1.0.1/extensions/

---

**Version:** 1.0.1  
**Last Updated:** 2025-02-10  
**License:** Apache 2.0  
**Copyright:** 2025 BLS Protocol Contributors
