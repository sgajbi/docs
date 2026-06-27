# Versioning, Tags, Changelogs, and Release Notes

## Purpose

This file explains how versioning and release documentation support traceability and controlled delivery.

A release should be traceable from version to tag to commit to artifact to deployment evidence.

---

## Versioning

Common versioning patterns:

- semantic versioning
- date-based versioning
- build number
- commit SHA
- release candidate suffix
- environment promotion tag

Choose based on product and deployment model.

---

## Tags

Tags should identify release points.

A release tag should map to:

- commit SHA
- artifact/image digest
- release notes
- test evidence
- security scan status
- deployment record
- rollback target

---

## Changelog

A changelog should summarize notable changes.

Categories:

- added
- changed
- fixed
- deprecated
- removed
- security
- migration
- operational

---

## Release Notes

Release notes should include:

- release version
- date
- scope
- major changes
- API/contract changes
- migration notes
- known issues
- rollback notes
- support impact
- evidence links

---

## Versioning Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Release not tagged | Hard to trace. |
| Tag points to wrong commit | Audit and rollback risk. |
| Changelog generated from memory | Missing changes. |
| Version not in running service | Runtime diagnosis hard. |
| Artifact not tied to version | Deployment ambiguity. |
| Breaking change hidden in patch note | Consumer risk. |

---

## Review Checklist

- Is versioning strategy defined?
- Are release tags used?
- Is tag tied to artifact?
- Does running service expose version?
- Is changelog updated?
- Are API/contract changes noted?
- Are migrations noted?
- Are known issues documented?
- Is rollback target clear?

---

## Summary

Versioning and release notes are not admin work.

They are traceability tools for support, audit, and rollback.
