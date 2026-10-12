---
name: chainsaw
description: Write, lint, and debug Kyverno Chainsaw tests. Use when writing or fixing a Chainsaw test.
---

# Chainsaw

The installed `chainsaw` binary is the source of truth for syntax and version.

## Source priority

Use sources in this order:

1. Installed `chainsaw` binary.
2. Schemas generated from the installed binary.
3. Existing tests in the current repository.
4. Official Chainsaw examples.
5. Official Kyverno Chainsaw tests.
6. Other official documentation.

Take syntax from a source that matches the installed version.

Official repositories:

* Chainsaw: https://github.com/kyverno/chainsaw
* Chainsaw examples: https://github.com/kyverno/chainsaw/tree/main/testdata/e2e/examples
* Kyverno: https://github.com/kyverno/kyverno
* Kyverno Chainsaw tests: https://github.com/kyverno/kyverno/tree/main/test/conformance/chainsaw

Use a third-party example only when the user asks for one.

## Version and schemas

Run these commands to anchor the version and generate schemas:

```bash
chainsaw version
chainsaw export schemas <directory>
```

Schemas go in the generated directory, not in this skill. Schemas must match the installed version.

An example from the `main` branch may not fit the installed version. When the installed version is known, take examples from the matching release.

## Create a test

1. Run `chainsaw version`.
2. Inspect existing tests in the repository.
3. Identify the behavior to test.
4. Find a similar official example.
5. Generate schemas when a field is unclear.
6. Copy the closest valid pattern.
7. Write the minimal test that verifies the behavior.

For generic Chainsaw behavior, use official Chainsaw examples. For Kyverno behavior, use official Kyverno conformance tests.

The minimal test uses only the resources, steps, waits, bindings, commands, and assertions it needs. Take field names and locations from the schema or a working example.

## Validate locally

After each change, lint the test:

```bash
chainsaw lint test -f <file>
```

Lint a configuration with:

```bash
chainsaw lint configuration -f <file>
```

Resolve every lint error before finishing. Linting needs no cluster.

## Run against a cluster

Before you run `chainsaw test` or any `kubectl` command, read [`cluster.md`](cluster.md). It holds the confirmation rules, the command allow-list, and the remediation protocol.

## Failure analysis

Classify the failure before changing the test. Fix the actual cause.

Types:

* YAML syntax error.
* Chainsaw schema error.
* Chainsaw syntax error.
* Kubernetes resource error.
* Timing error.
* Assertion error.
* Binding error.
* Kyverno policy error.
* Cluster or environment error.

Keep the expected behavior and the assertions fixed. Change the test to make it pass only when the expected behavior is wrong.

When cleanup fails, report the exact Chainsaw error. Classify it as a cluster or environment issue. State a specific cause only when verified.

## Completion criteria

A test is complete when:

1. It uses syntax from a source matching the installed version.
2. `chainsaw lint test -f <file>` succeeds.
3. `chainsaw test` succeeds when the user allows cluster access and the cluster is available.
4. It verifies the requested behavior.
5. Only the logic the test needs remains.

Report the state precisely: linted, executed, passed, failed, or not executed. Report a test as passed only when it ran. If the cluster is unavailable, report the error.
