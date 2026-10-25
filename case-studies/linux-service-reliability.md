# Linux service reliability

**Focus:** Linux administration, incident diagnosis, maintenance, recovery

## Illustrative scenario

Consider an internal web service that becomes intermittently unavailable after a maintenance window. The process appears to be running, but users cannot complete requests. The task is to locate the failure and restore the user-facing service.

## Engineering goal

Restore useful service, preserve the evidence needed for diagnosis, and keep the recovery steps easy to follow. Use authorized administrative access and document each intervention.

## Conceptual service path

```text
User request -> Web entry point -> Application service -> Required dependency
                                         ^
                                         |
                              Authorized operator
                                    (diagnosis)
```

The service path helps separate application behavior from dependency failures.

## Diagnostic approach

1. Establish when the symptom started and which user actions fail. Compare that timeline with the maintenance record.
2. Separate process state from service behavior. A running process is one observation; a successful user request is a different one.
3. Check resource pressure, dependency availability, service permissions, and recent changes. Preserve relevant evidence in a restricted incident record.
4. Form a specific hypothesis and change one variable at a time. Avoid repeated restarts that obscure the original failure.
5. If the evidence points to a recent change, evaluate a rollback against the risk of a new repair. Record the decision and the recovery checkpoint.

## Design decisions and tradeoffs

| Decision | Reason | Cost or limitation |
| --- | --- | --- |
| Use a functional health check | It can detect failures beyond process startup | The check needs a narrow, safe definition of success |
| Keep a change record and rollback procedure | It makes incident decisions traceable | The procedure must stay aligned with the actual service |
| Capture evidence before intervention | It supports root-cause analysis | Evidence collection must not delay urgent containment |

## Validation plan

In a disposable lab, introduce a missing dependency, a permission mismatch, and resource pressure as separate exercises. For each exercise, observe the failed user request, identify the relevant signal, apply a reversible correction, and confirm the request succeeds again. Also verify that the service can recover after a normal restart.

**Acceptance criteria:** the functional check detects the failure; the recovery procedure restores the expected user action; the change and its reversal are documented.

## Putting the approach into practice

Map the service's dependencies, agree on the maintenance window, and define recovery criteria before making changes. Record what restored service and which checks confirmed the result.
