# Medivium — Reference Architecture

AI-integrated patient self-service, CRM and electronic health record (EHR) platform for clinics, in active production.
Built by [AndreX IT](https://andrexit.com.br) under the "Healthcare Technology Concierge" model.

> This repository documents the architecture and engineering decisions only. No client code, data or credentials are published.

## Problem
Clinics lose time and revenue on manual WhatsApp scheduling, repeated prescription requests and scattered patient records.

## Result
- Prescription issuance time reduced by **more than 50%** in production use.
- Patients self-serve scheduling, confirmations and requests over WhatsApp, 24/7.
- Staff work from a single CRM + EHR instead of chat threads and spreadsheets.

## Architecture
```
Patient (WhatsApp)
      |
Messaging gateway (Evolution API)
      |
AI agent (LLM, guarded flows + human handoff)  <->  n8n workflows
      |
Core API (Node.js / TypeScript)
      |-- CRM (patients, funnel, reminders)
      |-- EHR (records, prescriptions)
      |-- Scheduling (confirm / reschedule / cancel buttons)
      |
PostgreSQL (multi-tenant)   |   Redis + BullMQ (queues)   |   AWS
```

## Key decisions
| Decision | Why |
|---|---|
| Guarded conversation flows around the LLM | Predictable behavior in a healthcare context; free text only where it adds value |
| Human handoff on any uncertainty | Safety and trust |
| Multi-tenant PostgreSQL | One platform, many clinics, isolated data |
| Queue-based reminders (BullMQ) | Reliable delivery and retries |
| Monitoring with Telegram alerts | Small team, fast incident response |

## Stack
Node.js, TypeScript, Next.js, PostgreSQL, Redis, BullMQ, Docker, AWS, n8n, WhatsApp (Evolution API), LLM integration.

## Related work
- WhatsApp AI agent for a salon: one main flow plus 7 sub-flows (service catalog, booking, reminders with confirm / reschedule / cancel buttons), integrated with the Trinks scheduling system.
- Security and resource monitoring stack across three production servers.

## Author
André Pisetta — CIO & Founder, AndreX IT · andre@andrexit.com.br · [LinkedIn](https://www.linkedin.com/in/andrep1994a2025)
