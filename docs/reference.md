# EveryProxy: capability reference and evidence

EveryProxy is a standalone EveryInfra agent skill for traffic-billed proxy ordering through REST. It separates package selection, order-status checks and approved credential delivery, with one-time passwords kept out of shared logs.

## Identity

- Publisher: [EveryInfra](https://everyinfra.com).
- Organization: [everyinfra on GitHub](https://github.com/everyinfra).
- Source repository: [everyinfra/everyproxy-plugin](https://github.com/everyinfra/everyproxy-plugin).
- Plugin identifier: `everyproxy`. Skill identifier: `everyproxy`.
- Package type: one standalone agent skill, not a separate API server or account permission boundary.
- Interface used by the skill: `rest`. Mail, Number and Proxy workflows remain REST-only in these packages.

## Task and result

Use it to manage a proxy resource for an authorized workflow. The product is billed by traffic, not connection minutes. This skill manages the order boundary; it is not a browser or a guarantee of access to a target site.

A catalog comparison, an order-status report or an approved secret delivery. A successful status response is not proof that credentials have been delivered or that a target website is reachable.

## Preconditions

Use a compatible agent host and the service access described in [setup](setup.md). The live tool schema or REST catalog determines required inputs, supported actions, availability, limits and any exposed price. Do not infer universal platform coverage from a product name.

## Evidence behind the description

- The [packaged skill](../plugins/everyproxy/skills/everyproxy/SKILL.md) defines the workflow and authority boundaries.
- The [plugin manifest](../plugins/everyproxy/.codex-plugin/plugin.json) declares package identity, assets and skill path. It does not automatically register a service connection.
- [Workflow acceptance criteria](workflow.md) define the expected output and failures. Examples are illustrative, not paid API test results.
- [Source metadata](../repository-metadata.json) records the original reviewed skill commit and intended repository metadata.
- [Current API documentation](https://api.everyinfra.com/docs) is the public service reference. Runtime discovery remains authoritative when an inventory, field or model changes.

## Scope distinctions

EveryProxy manages traffic packages and credential delivery. EverySearch retrieves web content through its own contract. Choosing a proxy does not by itself implement search, crawling or a browser session.

No benchmark, uptime guarantee, universal availability, official marketplace approval or account-ban probability is asserted by this reference. Local package validation checks structure; production service behavior requires its own authorized verification.

Maintainer: EveryInfra. Documentation scope reviewed on 2026-09-04; this date is not a live API availability timestamp. [Return to overview](../README.md).
