+++
title = "Semantic-Driven Software Engineering: MiniSpec and the SACC Five-Layer Model"
date = 2025-07-24
description = "An in-depth introduction to MiniSpec DSL and the SACC five-layer semantic model, exploring how to restructure our understanding of engineering language, context, and AI for semantic-driven development."

[taxonomies]
tags = ["AI Engineering", "MiniSpec", "SACC", "Software Architecture", "Semantic Models", "DevOps", "DSL", "Semantic-Driven Development"]

[extra]
show_comments = true
series = "SACC"
series_index = 1
+++

# Semantic-Driven Software Engineering: MiniSpec and the SACC Five-Layer Model

Software engineers have always dreamed of generating entire systems' code, tests, and deployment scripts through clear, structured documentation alone.

This dream is becoming reality in the wave of "Semantic-Driven Development."

This is the first article in the series, where we'll dive deep into what MiniSpec DSL is, and how its underlying SACC five-layer semantic model restructures our understanding of engineering language, context, and AI.

---

## The Ultimate Dream of Software Engineers: From UML to AI-native DSL

Over the past decades, UML was seen as the hope for achieving automated development: describing system design graphically, theoretically capable of generating code. But it ultimately failed in engineering practice.

Why?
- Semantic ambiguity, inconsistency between diagrams and code
- Lack of syntactic constraints, difficult to maintain
- Poor for version control and collaboration

Now, with the emergence of AI large models, "Prompt Engineering" has reignited hope for automated generation. However, it faces similar challenges:
- Unstructured, uncontrollable
- High hallucination and error rates
- Difficult for team collaboration

The lessons from both are clear: we need a new language layer to clearly describe engineering intent and context.

This is the birth background of MiniSpec DSL.

---

## What is MiniSpec?

MiniSpec (Minimal Specification) is a semantic-driven development syntax designed to make engineering intent understandable by both AI and humans, and mappable to executable artifacts.

It doesn't replace programming languages, but stands above them, providing a unified, clear, and generatable semantic description.

Its design principles include:

| Principle | Description |
|-----------|-------------|
| ✅ Readability | Human engineers can easily understand |
| ✅ Structurability | JSON/YAML expression, convenient for LLM parsing |
| ✅ Context-Intent Separation | Separate context from spec, clearly defining semantic boundaries |
| ✅ Action Mappable | Each spec can be mapped to an agent task |
| ✅ Modularity | Suitable for various scenarios: test, infra, deploy, api doc, prompt |

---

## MiniSpec Syntax Example

```yaml
version: "0.1"

context:
  env: "staging"
  user_role: "admin"
  feature_flag: "beta"
  git_branch: "feature/signup"
  runtime: "Node.js 18"

spec:
  goal: "Build new user registration flow"
  entities:
    - name: "User"
      attributes:
        - email
        - password
        - is_verified
  behavior:
    - "Send verification email after registration"
    - "email must be unique"
    - "password length must be greater than 8"
  test:
    - "Should successfully register"
    - "Duplicate email should error"
    - "Short password should be rejected"
  infra:
    deploy_target: "AWS Lambda"
    scaling: "auto"
    database: "PostgreSQL"
```

---

## Corresponding Application Outputs

| Type | LLM Can Generate |
|------|------------------|
| ✅ Test Code | RSpec / Jest / Pytest |
| ✅ Infra Code | Terraform / Pulumi |
| ✅ API Documentation | Swagger / OpenAPI |
| ✅ Prompt | Function schema / Assistant |
| ✅ DevOps | GitHub Action / CI pipeline |
| ✅ Documentation | README.md / Spec docs |
| ✅ UI Output | Figma DSL / HTML prototype |
| ✅ Scaffolding | model/controller/service |

---

## The SACC Five-Layer Semantic Model

Based on deep understanding of AI-code interaction, we propose the SACC (Spec and Context as Code) five-layer model:

### Layer Architecture

| Layer | Name | Function | Analogy |
|-------|------|----------|---------|
| L1 | Symbolic Layer | Bottom layer: language symbols, tokens, bitstreams | Similar to OSI physical/data layer |
| L2 | Instruction Layer | Basic syntax + API calls + control structures (like if/else, function) | Similar to OSI network/transport layer |
| L3 | Intent Layer | User's explicit intent, like "write tests for this code" | Similar to OSI session layer |
| L4 | Context Layer | Task background, conversation history, role settings, prior knowledge | Similar to OSI presentation layer |
| L5 | Specification Layer | Task standards, business specifications, constraints, meta-goals | Similar to OSI application layer |

### Detailed Analysis of Each Layer

**L1. Symbolic Layer — Language atoms received and processed by LLM**
- All language processing starts from symbols: tokens, characters, strings, opcodes
- Corresponds to: Tokenizer, Embeddings, Binary Instructions, Assembly
- Examples: string "def", token ##ing, or embedding vectors [0.82, -0.12, ...]

**L2. Instruction Layer — Syntactic structure of program behavior**
- Functions, conditional statements, API calls are the basic units at the execution level
- AI's understanding of this layer is key to behavior simulation (like Copilot's code completion)
- Examples: for, return, fetch('/api'), assert, and other syntax nodes

**L3. Intent Layer — Human's specific intent of "what to do"**
- This is the core level of prompt, e.g.: "Please automatically generate test code for this API"
- LLM plans program output based on intent
- Similar to function "calls" but more semantic and fuzzy

**L4. Context Layer — The source of AI understanding where context is king**
- This layer is LLM's "short-term memory" and "contextual understanding"
- Includes previous conversation content, role assumptions, input methods, prompt history
- Without context, AI is prone to hallucination or task misunderstanding

✅ **At this layer, we first propose: Spec/Context as Code**
Spec/Context determines behavior, just as Code determines output.

**L5. Specification Layer — Meta-instruction layer of semantics and logic**
- Like software specifications, describing "what should be accomplished" and its boundaries
- May come from: Product spec, Swagger, User story, design diagrams
- This layer doesn't directly appear in input but determines whether final output is correct

---

## Why Do We Need Such a Model?

### ✅ 1. Clearly Divide AI's Task "Thinking Levels"
- AI's understanding of prompts isn't simple text matching, but crosses semantic structures
- Establishing this layering helps build more stable, controllable AI behavior models

### ✅ 2. Help Us Design More Precise Prompt Flow
- You can adjust prompt tone and command structure at the Intent layer
- Design input flow and context control strategies at the Context layer
- Use DSL (like JSON schema) at the Specification layer to clearly limit generation behavior

### ✅ 3. Make AI "Programmable", Not Just Vague Dialogue
- Treating context as program code (Spec/Context as Code), we can:
- Version control context
- Unit test differences in output from different contexts
- Form "context script libraries": every prompt context is a reusable logic module

---

## Differences Between MiniSpec and Prompt Engineering

| Dimension | Prompt Engineering | MiniSpec DSL |
|-----------|-------------------|--------------|
| Reproducibility | ❌ Poor, slight input change completely different output | ✅ High, controllable output |
| Testability | ❌ Difficult to write unit tests | ✅ Can generate test cases |
| Team Consensus | ❌ Relies on individual skills | ✅ DSL + Schema |
| Multi-module Collaboration | ❌ Difficult to integrate with CI/CD | ✅ Can integrate with GitHub Workflow |
| Multi-scenario Extension | ⚠️ Requires lots of prompt writing | ✅ Naturally extends to Infra / UI / API Doc etc. |

---

## Vision: Building AI-native Engineering Ecosystem

Our goal isn't just MiniSpec, but an entire semantic-driven development ecosystem:
- ✅ MiniSpec DSL and Playground
- ✅ VSCode Extension and GitHub Workflow integration
- ✅ DevOps + OpenAPI + Prompt multi-scenario support
- ✅ Open specifications + 100 tutorial articles

We call it: SACC (Spec and Context as Code)

---

## Conclusion: The Beginning of an Era

This isn't "just another YAML DSL," but an upgrade in cognitive approach.

The essence of engineering is semantic alignment.

What we aim to do is provide a language that's both human-writable and AI-understandable, letting semantics no longer hide behind code, but become the first layer driving everything.

This is Semantic-Driven Development (Semantic-first Engineering).

---

*This is the first article in the SACC series, with subsequent articles diving deep into various practices and application scenarios of semantic-driven development.* 