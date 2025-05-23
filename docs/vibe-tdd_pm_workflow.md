# Vibe-TDD Project Management Workflow

## Summary

The conversation outlined a comprehensive workflow designed specifically for Project Managers who do not have extensive coding skills but excel in managing projects and defining scope. The goal is to effectively leverage AI-driven coding assistants (e.g., Cursor AI, Windsurf) using a test-driven development (TDD) methodology adapted for vibe coding—named **Prompt-Oriented Test Development (POTD)**.

## What is POTD?

**Prompt-Oriented Test Development** is a structured, repeatable approach where Project Managers use clearly defined, structured prompts to guide AI-driven development agents. By defining tests early as part of the project's scope, these tests act as guardrails for AI-generated implementations, ensuring alignment with project goals and business logic.

## Benefits

* **Clear and precise scope definition** from the project's outset.
* **Automated correctness checks** against predefined tests.
* **Reduced ambiguity and development drift**.
* **Repeatability and scalability** of AI interactions.
* **Ease of management and documentation**, enabling transparent collaboration.

## Workflow Phases

### Phase 1: Initial Scope Definition

1. Clearly define project requirements and business logic in plain, structured English.
2. Formulate these requirements into explicit scenarios or functional test cases.

Example:

| Requirement                  | Functional Scenario                         | Test Case (Plain English)                 |
| ---------------------------- | ------------------------------------------- | ----------------------------------------- |
| Allow users to create a task | User clicks "Create" and enters a task name | When task "Buy Milk" is entered, it saves |

### Phase 2: AI Test Generation

1. Use structured prompts to instruct AI agents to generate technical, executable test cases from the scenarios.
2. Iteratively refine the AI-generated tests to ensure clarity and accuracy.

Example Prompt:

* "Create test cases for secure login functionality."
* "Define test descriptions for edge cases of password resets."

### Phase 3: Feedback & Refinement Loop

1. Prompt AI agents to implement functionalities strictly guided by generated tests.
2. Continuously validate and refine AI-generated code through test results, ensuring alignment with project intent.

Example Prompt:

* "Implement the login functionality to pass the generated login tests."

### Phase 4: Modularization & Repeatability

1. Save structured prompts clearly in project documentation.
2. Create reusable and labeled prompt templates.
3. Continuously improve prompt templates based on iterative project feedback.

Example Structure:

* `prompt-template_user-auth.md`
* `prompt-template_reset-password.md`

## Role Clarifications

* **Project Manager**: Clearly defines business goals, logic, and scope through structured English prompts.
* **AI Agent (Cursor AI, Windsurf)**: Translates prompts into executable test cases, iteratively refines implementations to pass tests.

## Conclusion

This methodology allows Project Managers to effectively manage software development projects by clearly communicating intentions to AI-driven coding agents, ensuring project outcomes closely align with defined business goals. It is particularly suitable for managers who excel in defining and maintaining project scope but who may lack deep coding experience.

**In essence**, this workflow enables a PM to act as a conductor, orchestrating project outcomes clearly and effectively without directly engaging in coding activities.
