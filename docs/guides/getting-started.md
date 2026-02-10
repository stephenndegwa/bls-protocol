# Getting Started with BLS Protocol

Welcome to BLS Protocol! This guide will help you create your first Business Logic Specification.

## What You'll Learn

- What BLS is and why it's useful
- How to create your first BLS file
- How to use BLS with AI coding agents
- Best practices

## Prerequisites

- Basic understanding of business logic
- Familiarity with YAML or JSON
- A text editor

## Step 1: Understand BLS

BLS (Business Logic Specification) is a structured format for documenting business logic that both humans and AI can understand.

**Think of it as:**
- A blueprint for logic (not the building itself)
- Documentation that AI agents can read and implement
- A single source of truth for business rules

## Step 2: Choose Your Logic Type

BLS supports 6 types of logic:

| Logic Type | Use Case | Example |
|------------|----------|---------|
| **Decision Policy** | Select among options based on rules | Provider selection, routing |
| **Workflow** | Multi-step processes | Order fulfillment, onboarding |
| **State Machine** | State transitions | Call lifecycle, order status |
| **Decision Tree** | Hierarchical conditions | Lead qualification, risk assessment |
| **Event Handler** | React to events | Webhook processing, notifications |
| **Validation Rules** | Data correctness | Input validation, constraints |

## Step 3: Create Your First BLS File

Let's create a simple **Decision Policy** for selecting an SMS provider.

### Create the File

```bash
touch provider-selection.bls.yaml
```

### Add the Specification

```yaml
bls_version: "1.0.1"

specification:
  # Identity
  id: "sms-provider-selection"
  version: "1.0.0"
  name: "SMS Provider Selection Policy"
  description: "Selects the optimal SMS provider based on destination country and priority"
  
  # Classification
  domain: "communication"
  logic_type: "decision_policy"
  status: "active"
  
  # Ownership
  owner: "communications-team"
  
  # Define inputs
  inputs:
    - name: "destination_country"
      type: "string"
      description: "ISO 3166-1 alpha-2 country code (e.g., US, KE, GB)"
      required: true
      validation:
        pattern: "^[A-Z]{2}$"
      examples: ["US", "KE", "GB"]
      
    - name: "priority"
      type: "enum"
      description: "Message priority level"
      required: false
      default: "normal"
      validation:
        enum: ["normal", "high", "urgent"]
  
  # Define outputs
  outputs:
    - name: "provider_id"
      type: "string"
      description: "Selected provider identifier"
      
    - name: "fallback_providers"
      type: "array"
      description: "Backup providers if primary fails"
  
  # Define the logic
  logic:
    evaluation_strategy: "first_match"
    
    rules:
      # Rule 1: High-priority Kenya calls
      - id: "kenya-high-priority"
        name: "Kenya High Priority Routing"
        priority: 1
        condition:
          type: "expression"
          expression: "destination_country == 'KE' AND priority IN ['high', 'urgent']"
        action:
          type: "select_provider"
          params:
            provider_id: "africas_talking"
            fallback_providers: ["twilio", "vonage"]
        metadata:
          reason: "Africa's Talking has 99.9% delivery rate in Kenya"
      
      # Rule 2: All Kenya calls
      - id: "kenya-normal"
        name: "Kenya Normal Routing"
        priority: 2
        condition:
          type: "expression"
          expression: "destination_country == 'KE'"
        action:
          type: "select_provider"
          params:
            provider_id: "africas_talking"
            fallback_providers: ["twilio"]
      
      # Rule 3: USA calls
      - id: "usa-routing"
        name: "USA Routing"
        priority: 10
        condition:
          type: "expression"
          expression: "destination_country == 'US'"
        action:
          type: "select_provider"
          params:
            provider_id: "twilio"
            fallback_providers: ["vonage"]
      
    # Default for all other countries
    default_action:
      type: "select_cheapest"
      params:
        fallback_providers: ["twilio", "vonage"]
  
  # Define constraints
  constraints:
    - id: "fallback-required"
      type: "business_rule"
      description: "MUST have at least one fallback provider"
      expression: "len(output.fallback_providers) >= 1"
      severity: "error"
  
  # Define test cases
  test_cases:
    - id: "test-kenya-high-priority"
      name: "Kenya high priority call"
      category: "happy_path"
      input:
        destination_country: "KE"
        priority: "high"
      expected_output:
        provider_id: "africas_talking"
        fallback_providers: ["twilio", "vonage"]
        
    - id: "test-usa-normal"
      name: "USA normal priority call"
      category: "happy_path"
      input:
        destination_country: "US"
        priority: "normal"
      expected_output:
        provider_id: "twilio"
        fallback_providers: ["vonage"]
        
    - id: "test-unknown-country"
      name: "Unknown country defaults to cheapest"
      category: "edge_case"
      input:
        destination_country: "XX"
        priority: "normal"
      expected_output:
        # Will use default_action: select_cheapest
        fallback_providers: ["twilio", "vonage"]
```

## Step 4: Validate Your BLS

Check that your BLS file is valid:

```bash
# Using the BLS validator (when available)
bls validate provider-selection.bls.yaml

# Or manually check:
# ✓ bls_version is set
# ✓ specification.id is unique and lowercase
# ✓ All inputs have types
# ✓ All outputs are defined
# ✓ Logic matches the logic_type
# ✓ Test cases cover main scenarios
```

## Step 5: Use BLS with AI

Now you can give this BLS file to an AI coding agent like Claude Code:

### Example Prompt to Claude Code:

```
I have a Business Logic Specification (BLS) file that defines 
SMS provider selection logic.

Please read the attached provider-selection.bls.yaml file and:
1. Implement the logic in Python
2. Create a function that takes the inputs and returns the outputs
3. Generate tests based on the test cases in the BLS file
4. Include traceability comments linking back to the BLS

The implementation should follow the exact logic specified in the 
BLS file.
```

### What the AI Will Generate:

**Python Implementation:**
```python
# provider_selection.py
# Generated from: provider-selection.bls.yaml
# BLS Version: 1.0.0

from typing import Dict, List, Literal

def select_sms_provider(
    destination_country: str,
    priority: Literal["normal", "high", "urgent"] = "normal"
) -> Dict[str, any]:
    """
    SMS Provider Selection Policy
    
    Selects the optimal SMS provider based on destination country 
    and priority.
    
    BLS Source: provider-selection.bls.yaml
    
    Args:
        destination_country: ISO 3166-1 alpha-2 country code
        priority: Message priority level
        
    Returns:
        Dict with provider_id and fallback_providers
    """
    
    # Rule: kenya-high-priority (priority 1)
    # Source: provider-selection.bls.yaml#rule:kenya-high-priority
    if destination_country == "KE" and priority in ["high", "urgent"]:
        return {
            "provider_id": "africas_talking",
            "fallback_providers": ["twilio", "vonage"]
        }
    
    # Rule: kenya-normal (priority 2)
    # Source: provider-selection.bls.yaml#rule:kenya-normal
    if destination_country == "KE":
        return {
            "provider_id": "africas_talking",
            "fallback_providers": ["twilio"]
        }
    
    # Rule: usa-routing (priority 10)
    # Source: provider-selection.bls.yaml#rule:usa-routing
    if destination_country == "US":
        return {
            "provider_id": "twilio",
            "fallback_providers": ["vonage"]
        }
    
    # Default action: select_cheapest
    # Source: provider-selection.bls.yaml#default_action
    cheapest = get_cheapest_provider(destination_country)
    return {
        "provider_id": cheapest,
        "fallback_providers": ["twilio", "vonage"]
    }
```

**Tests:**
```python
# test_provider_selection.py
# Generated from: provider-selection.bls.yaml test cases

import pytest
from provider_selection import select_sms_provider

def test_kenya_high_priority():
    """Test case: kenya-high-priority"""
    result = select_sms_provider(
        destination_country="KE",
        priority="high"
    )
    
    assert result["provider_id"] == "africas_talking"
    assert result["fallback_providers"] == ["twilio", "vonage"]

def test_usa_normal():
    """Test case: usa-normal"""
    result = select_sms_provider(
        destination_country="US",
        priority="normal"
    )
    
    assert result["provider_id"] == "twilio"
    assert result["fallback_providers"] == ["vonage"]

def test_unknown_country():
    """Test case: unknown-country"""
    result = select_sms_provider(
        destination_country="XX",
        priority="normal"
    )
    
    # Default action applies
    assert "provider_id" in result
    assert len(result["fallback_providers"]) >= 1
```

## Step 6: Change Logic (Later)

When business logic changes:

```yaml
# Just edit the BLS file
rules:
  - id: "usa-routing"
    condition:
      expression: "destination_country == 'US'"
    action:
      params:
        provider_id: "vonage"  # Changed from "twilio"
```

Then tell the AI to regenerate:
```
The BLS file has been updated. Please regenerate the code 
from the updated provider-selection.bls.yaml file.
```

## Next Steps

- Read the [full BLS specification](../specifications/BLS_Protocol_v1.0.1.md)
- Explore [more examples](../../examples/)
- Learn about other [logic types](./logic-types.md)
- Try the [tutorial series](../tutorials/)

## Best Practices

### ✅ DO:
- Keep BLS files focused (one logic unit per file)
- Add clear descriptions
- Include test cases for all major paths
- Document business reasons in metadata
- Use meaningful IDs (kebab-case)
- Version your BLS files

### ❌ DON'T:
- Hardcode sensitive values (use parameters)
- Create circular dependencies
- Skimp on test cases
- Forget to update version when logic changes
- Mix multiple logic types in one file

## Common Questions

**Q: Can I use BLS without AI?**  
A: Yes! BLS is valuable as documentation even without AI. But AI makes implementing it much faster.

**Q: What if my logic doesn't fit the 6 types?**  
A: Most business logic fits one of these types. If not, consider combining multiple BLS files or proposing a new logic type.

**Q: How do I handle secrets/credentials?**  
A: Never put secrets in BLS files. Use parameters and environment variables.

**Q: Can I use BLS for frontend logic?**  
A: Yes! BLS works for any business logic, frontend or backend.

**Q: Does BLS replace my code?**  
A: No, BLS documents your logic. AI generates code from BLS, but the code is still yours to own and deploy.

## Getting Help

- Read the [FAQ](./faq.md)
- Check [examples](../../examples/)
- Ask in [GitHub Discussions](https://github.com/bls-protocol/bls-protocol/discussions)
- Report issues on [GitHub](https://github.com/bls-protocol/bls-protocol/issues)

---

**Congratulations!** You've created your first BLS file and learned how to use it with AI. 🎉

Next: [Learn about Workflows →](./workflows.md)
