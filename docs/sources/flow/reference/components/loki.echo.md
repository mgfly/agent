---
aliases:
- /docs/grafana-cloud/agent/flow/reference/components/loki.echo/
- /docs/grafana-cloud/monitor-infrastructure/agent/flow/reference/components/loki.echo/
- /docs/grafana-cloud/monitor-infrastructure/integrations/agent/flow/reference/components/loki.echo/
- /docs/grafana-cloud/send-data/agent/flow/reference/components/loki.echo/
canonical: https://grafana.com/docs/agent/latest/flow/reference/components/loki.echo/
description: Learn about loki.echo
title: loki.echo
---

# loki.echo

{{< docs/shared lookup="flow/stability/beta.md" source="agent" version="<AGENT_VERSION>" >}}

`loki.echo` receives log entries from other `loki` components and prints them
to the process' standard output (stdout).

Multiple `loki.echo` components can be specified by giving them
different labels.

## Usage

```river
loki.echo "LABEL" {}
```

## Arguments

`loki.echo` accepts no arguments.

## Exported fields

The following fields are exported and can be referenced by other components:

Name | Type | Description
---- | ---- | -----------
`receiver` | `LogsReceiver` | A value that other components can use to send log entries to.

## Component health

`loki.echo` is only reported as unhealthy if given an invalid configuration.

## Debug information

`loki.echo` does not expose any component-specific debug information.

## Example

This example creates a pipeline that reads log files from `/var/log` and
prints log lines to echo:

```river
local.file_match "varlog" {
  path_targets = [{
    __path__ = "/var/log/*log",
    job      = "varlog",
  }]
}

loki.source.file "logs" {
  targets    = local.file_match.varlog.targets
  forward_to = [loki.echo.example.receiver]
}

loki.echo "example" { }
```

<!-- START GENERATED COMPATIBLE COMPONENTS -->

## Compatible components

`loki.echo` has exports that can be consumed by the following components:

- Components that consume [Loki `LogsReceiver`](../../compatibility/#loki-logsreceiver-consumers)

{{< admonition type="note" >}}
Connecting some components may not be sensible or components may require further configuration to make the connection work correctly.
Refer to the linked documentation for more details.
{{< /admonition >}}

<!-- END GENERATED COMPATIBLE COMPONENTS -->
