# Documentation Checklists and Engineering Lead Playbook

## Purpose

This file provides practical checklists for documentation, KT, architecture governance, and knowledge management.

Use these checklists during PR reviews, architecture reviews, KT planning, repo audits, and knowledge-base maintenance.

---

## README Checklist

- [ ] Purpose clear.
- [ ] Responsibilities listed.
- [ ] Non-responsibilities listed.
- [ ] Current status honest.
- [ ] Architecture overview included.
- [ ] Local commands current.
- [ ] CI/validation commands current.
- [ ] Docs map included.
- [ ] Operations links included.
- [ ] Owner/contact clear.

---

## Architecture Doc Checklist

- [ ] Audience clear.
- [ ] Scope clear.
- [ ] Current/future state separated.
- [ ] Service boundaries clear.
- [ ] Data flows shown.
- [ ] Runtime view included where needed.
- [ ] Security boundaries included.
- [ ] Failure modes included.
- [ ] Decisions linked.
- [ ] Evidence linked.

---

## ADR/RFC Checklist

- [ ] Context clear.
- [ ] Decision or proposal explicit.
- [ ] Alternatives considered.
- [ ] Consequences documented.
- [ ] Security impact included.
- [ ] Operational impact included.
- [ ] Implementation evidence linked.
- [ ] Status current.
- [ ] Review/supersession path defined.

---

## API and Contract Docs Checklist

- [ ] Owner clear.
- [ ] Route/schema/event documented.
- [ ] Examples current and synthetic.
- [ ] Errors documented.
- [ ] Versioning documented.
- [ ] Idempotency/pagination documented.
- [ ] Supportability documented.
- [ ] Access rules documented.
- [ ] Tests/evidence linked.
- [ ] Deprecation status clear.

---

## Runbook Checklist

- [ ] Symptoms listed.
- [ ] Impact explained.
- [ ] Dashboards linked.
- [ ] Alerts linked.
- [ ] First checks safe.
- [ ] Diagnosis steps clear.
- [ ] Mitigation clear.
- [ ] Rollback/replay included.
- [ ] Escalation path included.
- [ ] Data-safety notes included.

---

## Supported Feature Checklist

- [ ] Feature name clear.
- [ ] Status explicit.
- [ ] Owner listed.
- [ ] Supported inputs listed.
- [ ] Unsupported inputs listed.
- [ ] APIs/UI surfaces listed.
- [ ] Tests linked.
- [ ] Runtime evidence linked.
- [ ] Known gaps listed.
- [ ] Last reviewed date present.

---

## KT and Onboarding Checklist

- [ ] Role-based learning paths exist.
- [ ] New joiner setup documented.
- [ ] Architecture map available.
- [ ] Domain glossary available.
- [ ] Hands-on exercises included.
- [ ] Runbook introduction included.
- [ ] PR/review workflow included.
- [ ] Security rules included.
- [ ] Progress measurable.
- [ ] Feedback loop exists.

---

## Documentation Security Checklist

- [ ] Classification clear.
- [ ] Examples synthetic.
- [ ] Screenshots safe.
- [ ] Secrets absent.
- [ ] Internal paths removed where needed.
- [ ] Raw logs absent.
- [ ] Prompt/model outputs governed.
- [ ] Evidence filtered.
- [ ] Sensitive-content checks run.
- [ ] Audience appropriate.

---

## Knowledge Maintenance Checklist

- [ ] Owner defined.
- [ ] Lifecycle status defined.
- [ ] Last reviewed date present.
- [ ] Review cadence defined.
- [ ] Links valid.
- [ ] Duplicates removed.
- [ ] Deprecated docs labelled.
- [ ] Archived docs separated.
- [ ] Index updated.
- [ ] User feedback reviewed.

---

## Engineering Lead Operating Rhythm

| Cadence | Activity |
|---|---|
| Every PR | Check docs impact and supported claims. |
| Weekly | Review stale docs, broken links, and KT gaps. |
| Monthly | Review runbooks, ADRs, and architecture docs. |
| Quarterly | Review knowledge taxonomy and maturity roadmap. |
| After incident | Update runbooks, architecture notes, and evidence maps. |
| After onboarding | Update KT paths based on new joiner feedback. |

---

## Summary

Documentation leadership is engineering leadership.

A team that keeps knowledge clear, current, and evidence-backed moves faster with less risk.
