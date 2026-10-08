---

name: chainsaw
description: Create and validate Kyverno Chainsaw tests. Use this skill when creating, modifying, reviewing, debugging, or validating Chainsaw tests or Kyverno tests that use Chainsaw.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Chainsaw

Use this skill to create correct Kyverno Chainsaw tests.

## Core rules

* Do not invent Chainsaw syntax.
* Use syntax supported by the installed Chainsaw version.
* Use existing working tests as examples.
* Prefer the installed Chainsaw binary as the source of truth.
* Keep tests small.
* Use the smallest number of resources, steps, assertions, and waits needed.
* Do not change the expected behavior only to make a test pass.
* Fix validation errors before finishing.

## Source priority

Use sources in this order:

1. Installed Chainsaw binary.
2. Schemas generated from the installed binary.
3. Existing tests in the current repository.
4. Official Chainsaw examples.
5. Official Kyverno Chainsaw tests.
6. Other official documentation.

Do not use forks, mirrors, or third-party examples unless the user asks for them.

Official repositories:

* Chainsaw: https://github.com/kyverno/chainsaw
* Chainsaw examples: https://github.com/kyverno/chainsaw/tree/main/testdata/e2e/examples
* Kyverno: https://github.com/kyverno/kyverno
* Kyverno Chainsaw tests: https://github.com/kyverno/kyverno/tree/main/test/conformance/chainsaw

## Version

Always check the installed version before creating or modifying a Chainsaw test:

```bash
chainsaw version
```

Do not assume that examples from `main` work with the installed version.

When possible, use examples from the matching Chainsaw release.

## Schemas

Generate schemas from the installed Chainsaw binary when schemas are needed:

```bash
chainsaw export schemas <directory>
```

Do not copy Chainsaw schemas into this skill.

Generated schemas must match the installed Chainsaw version.

## Create a test

Before creating a test:

1. Run `chainsaw version`.
2. Inspect existing tests in the repository.
3. Identify the behavior to test.
4. Find a similar official example.
5. Generate schemas if needed.
6. Reuse the closest valid pattern.
7. Create the smallest test that verifies the behavior.

For generic Chainsaw behavior, prefer official Chainsaw examples.

For Kyverno behavior, prefer official Kyverno Chainsaw conformance tests.

## YAML rules

When writing YAML:

* Use valid YAML.
* Use the API version supported by the installed Chainsaw version.
* Use fields supported by the installed Chainsaw version.
* Follow the structure used by working examples.
* Keep indentation consistent.
* Do not add fields that are not required.
* Do not add unnecessary steps.
* Do not add unnecessary waits.
* Do not add unnecessary bindings.
* Do not add unnecessary commands.
* Do not add unnecessary assertions.
* Do not add unnecessary resources.

Do not guess field names or field locations.

If unsure about a field, check the generated schema or an official example.

## Local validation

Always validate a test after creating or modifying it:

```bash
chainsaw lint test -f <file>
```

Validate a configuration with:

```bash
chainsaw lint configuration -f <file>
```

Fix all lint errors before finishing.

Local validation does not require access to a Kubernetes cluster.

## Kubernetes cluster access

Never assume that the current Kubernetes context is the correct cluster.

Do not access a Kubernetes cluster without user confirmation.

### Commands allowed without confirmation

The agent may run these commands without confirmation:

```bash
chainsaw version
chainsaw lint test -f <file>
chainsaw lint configuration -f <file>
chainsaw export schemas <directory>
kubectl config current-context
kubectl config get-contexts
```

These commands do not modify Kubernetes resources.

### Commands requiring confirmation

Ask for confirmation before running:

```bash
chainsaw test ...
kubectl get ...
kubectl apply ...
kubectl create ...
kubectl delete ...
kubectl patch ...
kubectl edit ...
```

Also ask for confirmation before running any other command that can access, create, modify, or delete Kubernetes resources.

Do not bypass this rule by combining a cluster command with another command.

### Before `chainsaw test`

First validate the test locally.

Then ask the user for confirmation.

Tell the user:

* which command will run;
* that it will access the current Kubernetes cluster;
* that the test may create, modify, or delete Kubernetes resources;
* that the current Kubernetes context must point to the intended cluster.

Example:

> `chainsaw test ./tests/namespace-label` will access the current Kubernetes cluster and may create or delete test resources. The current context is `<context>`. Confirm that this is the intended cluster before I run the test.

Do not run the command until the user confirms.

## Test execution

Run tests only after user confirmation:

```bash
chainsaw test <path>
```

If the user does not confirm, do not run the test.

If the cluster is unavailable, report the error.

Do not claim that a test passed if it was not executed.

If only lint was run, state:

> The test was linted but not executed.

## Failure analysis

Classify failures before changing the test.

Possible failure types:

* YAML syntax error.
* Chainsaw schema error.
* Chainsaw syntax error.
* Kubernetes resource error.
* Timing error.
* Assertion error.
* Binding error.
* Kyverno policy error.
* Cluster or environment error.

Fix the actual cause.

Do not weaken assertions or remove test behavior only to hide a failure.

## Completion criteria

A Chainsaw test is complete when:

1. It uses syntax supported by the installed Chainsaw version.
2. `chainsaw lint test -f <file>` succeeds.
3. `chainsaw test` succeeds when the user allows cluster access and the cluster is available.
4. The test verifies the requested behavior.
5. No unnecessary test logic remains.

If the test was not executed, say so clearly.

## Language and output

Use short and direct sentences.

Use one instruction per sentence.

Avoid vague words such as:

* probably
* maybe
* generally
* usually
* should be fine

State what is known.

State what is not known.

Do not hide assumptions.

When a command requires confirmation, stop and ask for confirmation.

When reporting validation, distinguish clearly between:

* linted;
* executed;
* passed;
* failed;
* not executed.
