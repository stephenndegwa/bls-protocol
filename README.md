# BLS Protocol - Business Logic Specification

**Transform how AI agents build software by separating business logic from implementation**

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/Version-1.0.1-green.svg)](CHANGELOG.md)
[![Documentation](https://img.shields.io/badge/docs-latest-brightgreen.svg)](docs/)

---

## What is BLS?

**BLS (Business Logic Specification)** is a structured format for documenting business logic in a way that both humans and AI agents can understand and use.

Think of it as:
- **OpenAPI/Swagger** for APIs → **BLS** for Business Logic
- **Database Schema** for data → **BLS** for Logic
- **Architecture Blueprints** for buildings → **BLS** for Software Logic

### The Core Principle

> **Business logic is not coded directly. It is discovered, modeled, validated, and then implemented.**

---

## Why BLS?

### The Problem

Traditional software development:
- ❌ Business logic buried in code
- ❌ Logic scattered across codebases
- ❌ Changes require hunting through files
- ❌ AI agents need to guess requirements
- ❌ Logic drift between services
- ❌ Non-technical stakeholders can't review logic

### The Solution

With BLS:
- ✅ Logic documented in structured format
- ✅ Single source of truth
- ✅ AI agents read BLS to generate code
- ✅ Changes = edit one file
- ✅ Consistent across all services
- ✅ Business users can review logic

---

## Quick Example

### Business Logic in BLS

```yaml
# File: bls/policies/provider-selection.bls.yaml

bls_version: "1.0.1"
specification:
  id: "provider-selection"
  name: "SMS Provider Selection Policy"
  logic_type: "decision_policy"
  
  inputs:
    - name: "destination_country"
      type: "string"
      
  outputs:
    - name: "provider_id"
      type: "string"
      
  logic:
    rules:
      - id: "kenya-routing"
        priority: 1
        condition:
          expression: "destination_country == 'KE'"
        action:
          provider: "africas_talking"
          fallback: ["twilio", "vonage"]
          
      - id: "usa-routing"
        priority: 2
        condition:
          expression: "destination_country == 'US'"
        action:
          provider: "twilio"
          
    default_action:
      type: "select_cheapest"
      
  test_cases:
    - name: "kenya_call"
      input: { destination_country: "KE" }
      expected_output: { provider_id: "africas_talking" }
```

### AI Generates Implementation

Give this BLS file to Claude Code, GPT Engineer, or any AI coding agent:

**Python:**
```python
def select_provider(destination_country: str) -> str:
    """Generated from: bls/policies/provider-selection.bls.yaml"""
    
    if destination_country == "KE":
        return "africas_talking"
    
    if destination_country == "US":
        return "twilio"
    
    return select_cheapest_provider(destination_country)
```

**TypeScript:**
```typescript
function selectProvider(destinationCountry: string): string {
  // Generated from: bls/policies/provider-selection.bls.yaml
  
  if (destinationCountry === 'KE') {
    return 'africas_talking';
  }
  
  if (destinationCountry === 'US') {
    return 'twilio';
  }
  
  return selectCheapestProvider(destinationCountry);
}
```

**Same BLS → Any Language!**

---

## When Business Logic Changes

### Before (Traditional):

```
1. Search codebase for provider selection logic
2. Find it scattered across 5 files
3. Update Python version
4. Update TypeScript version
5. Update Go version
6. Hope you didn't miss anything
7. Deploy all services

Time: 2-4 hours
Risk: High
```

### After (With BLS):

```yaml
# 1. Edit BLS file (one line)
rules:
  - id: "usa-routing"
    action:
      provider: "vonage"  # Changed from "twilio"
```

```bash
# 2. Tell AI to regenerate
$ npm run generate  # or just tell Claude Code

# 3. Deploy
$ deploy

Time: 2 minutes
Risk: Minimal (AI generates consistent code)
```

---

## Repository Contents

```
bls-protocol/
├── README.md                          # This file
├── LICENSE                            # Apache 2.0
├── CHANGELOG.md                       # Version history
├── CONTRIBUTING.md                    # How to contribute
│
├── docs/
│   ├── specifications/
│   │   ├── BLS_Protocol_v1.0.1.md    # Complete BLS specification
│   │   ├── PUD_Format.md              # Product Understanding Document format
│   │   └── Autonomous_Coding_System.md # Complete system architecture
│   │
│   ├── guides/
│   │   ├── getting-started.md
│   │   ├── ai-implementation-guide.md # For AI agents
│   │   ├── writing-bls.md
│   │   └── migration-guide.md
│   │
│   └── tutorials/
│       ├── 01-first-bls-file.md
│       ├── 02-decision-policies.md
│       ├── 03-workflows.md
│       └── 04-state-machines.md
│
├── schemas/
│   ├── v1.0.1/
│   │   ├── core.json                  # BLS JSON Schema
│   │   ├── decision-policy.json
│   │   ├── workflow.json
│   │   ├── state-machine.json
│   │   ├── decision-tree.json
│   │   ├── event-handler.json
│   │   └── validation-rules.json
│   │
│   └── extensions/
│       ├── healthcare.json
│       ├── finance.json
│       └── ecommerce.json
│
├── examples/
│   ├── voiceflow-ai/                  # Complete working example
│   │   ├── pud.yaml
│   │   └── bls/
│   │       ├── policies/
│   │       ├── workflows/
│   │       └── state-machines/
│   │
│   ├── ecommerce/
│   ├── healthcare/
│   └── saas-platform/
│
├── tools/
│   ├── validator/                     # BLS validation tool
│   │   ├── README.md
│   │   ├── package.json
│   │   └── src/
│   │
│   ├── visualizer/                    # BLS visualization tool
│   │   └── README.md
│   │
│   ├── ai-generator/                  # AI code generation helpers
│   │   └── prompts/
│   │
│   └── cli/                           # Command-line tools
│       └── README.md
│
├── agents/
│   ├── discovery-agent/               # Requirements gathering
│   │   ├── README.md
│   │   └── prompts/
│   │
│   └── bls-agent/                     # PUD → BLS conversion
│       ├── README.md
│       └── prompts/
│
└── templates/
    ├── decision-policy.bls.yaml
    ├── workflow.bls.yaml
    ├── state-machine.bls.yaml
    └── pud-template.yaml
```

---

## Getting Started

### 1. Read the Specifications

Start here:
- [BLS Protocol v1.0.1](docs/specifications/BLS_Protocol_v1.0.1.md) - Complete BLS format
- [Getting Started Guide](docs/guides/getting-started.md) - Quick tutorial
- [Examples](examples/) - Working examples

### 2. Try Creating Your First BLS File

```bash
# Use a template
cp templates/decision-policy.bls.yaml my-logic.bls.yaml

# Edit it
vim my-logic.bls.yaml

# Validate it
bls validate my-logic.bls.yaml
```

### 3. Give BLS to AI

```
You: "Here's my business logic in BLS format. 
Please implement it in Python with FastAPI."

Claude Code: *reads BLS* *generates code* "Done!"
```

---

## Use Cases

### ✅ AI-Powered Applications
- AI reads BLS to understand exact business logic
- No ambiguity, no guessing
- Consistent implementation

### ✅ Multi-Language Projects
- Same BLS → Python backend, TypeScript frontend, Go microservices
- Logic stays consistent across all services

### ✅ Regulated Industries
- Healthcare (HIPAA compliance)
- Finance (PCI-DSS compliance)
- Legal requirements explicitly documented

### ✅ Complex Business Logic
- Provider routing
- Pricing engines
- Lead qualification
- Workflow automation

### ✅ Rapid Changes
- Business logic changes frequently
- Need to update without full deployment
- Data-driven logic execution

---

## The Complete Workflow

```
┌─────────────────────────────────────────┐
│  1. DISCOVERY                           │
│  Gather requirements → PUD Document     │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  2. MODELING                            │
│  Convert PUD → BLS Specifications       │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  3. VALIDATION                          │
│  Review BLS, verify completeness        │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  4. IMPLEMENTATION                      │
│  AI reads BLS → Generates code          │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  5. DEPLOYMENT                          │
│  Tests pass → Deploy                    │
└─────────────────────────────────────────┘
```

---

## Features

### 🎯 **Six Logic Types**
- **Decision Policy** - Select among options based on rules
- **Workflow** - Multi-step processes
- **State Machine** - State transitions
- **Decision Tree** - Hierarchical conditional logic
- **Event Handler** - Event-driven reactions
- **Validation Rules** - Data correctness

### 🔍 **Complete Traceability**
- Every line of code traces back to BLS
- BLS traces back to requirements (PUD)
- Full audit trail

### 🌐 **Language Agnostic**
- Same BLS works for Python, TypeScript, Go, Rust, Java, etc.
- AI generates code in any language

### ✅ **Built-in Validation**
- Schema validation
- Completeness checks
- Constraint verification
- Test case coverage

### 📊 **Observability Built-in**
- Logging requirements
- Metrics specifications
- Tracing configuration

### 🔒 **Security & Compliance**
- GDPR, HIPAA, TCPA support
- Security requirements
- Audit trails

---

## Documentation

- **[Complete Specification](docs/specifications/BLS_Protocol_v1.0.1.md)** - Full BLS v1.0.1 spec
- **[Getting Started](docs/guides/getting-started.md)** - Quick start guide
- **[AI Implementation Guide](docs/guides/ai-implementation-guide.md)** - For AI agents
- **[Examples](examples/)** - Working examples
- **[Tutorials](docs/tutorials/)** - Step-by-step tutorials

---

## Tools

### Validator
```bash
npm install -g @bls-protocol/validator

bls validate my-logic.bls.yaml
```

### Visualizer
```bash
bls visualize my-workflow.bls.yaml
# Generates flowchart
```

### CLI
```bash
bls init              # Create new BLS project
bls new policy        # Create new decision policy
bls validate          # Validate all BLS files
bls generate --lang python  # Generate code
```

---

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for:
- How to contribute
- Code of conduct
- Development setup
- Submission guidelines

---

## Roadmap

### v1.0 ✅ (Current)
- [x] Core BLS specification
- [x] 6 logic types
- [x] JSON Schema
- [x] Documentation
- [x] Examples

### v1.1 (Q2 2025)
- [ ] Validator tool
- [ ] Visualizer tool
- [ ] CLI tool
- [ ] VS Code extension

### v1.2 (Q3 2025)
- [ ] Visual BLS editor
- [ ] BLS registry (central repository)
- [ ] AI agent implementations
- [ ] More domain extensions

### v2.0 (Q4 2025)
- [ ] BLS IDE
- [ ] Real-time collaboration
- [ ] BLS marketplace
- [ ] Advanced analytics

---

## Community

- **Discord:** [Join our Discord](https://discord.gg/bls-protocol) (coming soon)
- **Forum:** [BLS Protocol Discussions](https://github.com/bls-protocol/bls-protocol/discussions)
- **Twitter:** [@BLSProtocol](https://twitter.com/BLSProtocol) (coming soon)
- **Blog:** [blog.bls-protocol.org](https://blog.bls-protocol.org) (coming soon)

---

## License

Apache License 2.0 - see [LICENSE](LICENSE) for details.

---

## Citation

If you use BLS Protocol in your research or project, please cite:

```bibtex
@misc{bls-protocol-2025,
  title={BLS Protocol: Business Logic Specification for AI-Powered Development},
  author={BLS Protocol Contributors},
  year={2025},
  url={https://github.com/bls-protocol/bls-protocol}
}
```

---

## Acknowledgments

Built with insights from:
- Modern AI coding agents (Claude Code, GPT Engineer, Devin)
- OpenAPI/Swagger specification
- Domain-driven design principles
- Industry best practices in regulated industries

---

## Quick Links

- [📖 Full Documentation](docs/)
- [🚀 Getting Started](docs/guides/getting-started.md)
- [📋 Examples](examples/)
- [🔧 Tools](tools/)
- [💬 Discussions](https://github.com/bls-protocol/bls-protocol/discussions)
- [🐛 Issues](https://github.com/bls-protocol/bls-protocol/issues)

---

**Made with ❤️ by the BLS Protocol community**

*Transforming how AI agents understand and implement business logic*
