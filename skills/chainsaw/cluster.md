# Cluster protocol

Read this file before running `chainsaw test` or any `kubectl` command.

## Confirmation

The current Kubernetes context is not assumed correct. Confirm the context with the user before cluster access. Run cluster commands only after the user confirms.

## Commands allowed without confirmation

These commands read local state or cluster metadata:

```bash
chainsaw version
chainsaw lint test -f <file>
chainsaw lint configuration -f <file>
chainsaw export schemas <directory>
kubectl config current-context
kubectl config get-contexts
```

## Commands requiring confirmation

Ask before running:

```bash
chainsaw test ...
kubectl get ...
kubectl apply ...
kubectl create ...
kubectl delete ...
kubectl patch ...
kubectl edit ...
```

Ask before any other command that reads, creates, modifies, or deletes Kubernetes resources. Run a cluster command on its own line. Do not combine it with another command.

## Before `chainsaw test`

1. Lint the test locally.
2. Ask the user for confirmation. Tell them:
   * which command will run;
   * that it accesses the current Kubernetes cluster;
   * that the test may create, modify, or delete resources;
   * that the current context must point to the intended cluster.
3. Run the command only after the user confirms.

If the user does not confirm, do not run the test.

Example:

> `chainsaw test ./tests/namespace-label` will access the current Kubernetes cluster and may create or delete test resources. The current context is `<context>`. Confirm that this is the intended cluster before I run the test.

## Remediation

Remediate only when the problem requires it. First explain the problem and the proposed command. Then ask for confirmation.

This applies to every command that deletes, patches, replaces, or modifies Kubernetes resources.
