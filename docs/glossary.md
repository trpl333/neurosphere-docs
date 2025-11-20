# NeuroSphereAI Glossary (Draft)

This file will contain the official glossary of all terms used across the NeuroSphere platform.

📘 NeuroSphereAI Global Glossary

Last updated: 2025-11-20
Authoritative source for terminology used across: ChatStack, AI-Memory, Orchestrator, Admin Panel, and Voice AI.

# 1. Core Concepts
Tenant

A business entity using the NeuroSphereAI platform. Each tenant has isolated data, phone numbers, memory storage, configuration, and admin access.
Example: Peterson Insurance.

Customer

Synonym for Tenant used in the ChatStack database schema. Internally represents the same concept.

Customer User

A client or prospect of the tenant.
This is the end user calling in or receiving calls — not the tenant’s staff.
Identified by phone number.

Caller

Any inbound phone user contacting the tenant’s phone number.
If recognized, mapped to a Customer User profile in AI-Memory.

Agent

A human staff member of the tenant (e.g., licensed insurance agent).
May receive transferred calls from the voice AI.

Admin User

A privileged login user of the tenant’s Admin Panel.

Session

A single conversational episode — a call, chat, or AI-driven workflow.
Tracks state, memory usage, and interactions.

Interaction

Any meaningful exchange between the user and the system — message, function call, call segment, or memory update.

# 2. NeuroSphereAI System Entities
AI-Memory

The NeuroSphereAI subsystem responsible for long-term, structured, recallable memory for each caller and tenant.

Memory Object

A structured item stored in AI-Memory such as facts, preferences, relationships, identity attributes, or call summaries.

Memory Slice

A filtered collection of memory objects retrieved for a specific call or prompt.

Personality Sliders

Configurable attributes controlling tone, pacing, empathy, assertiveness, humor, and conversational style.

Prompt Template

Reusable structured prompts used by orchestrator or LLM. May embed memory slices, context, system instructions, and tenant configuration.

Manifest

A versioned JSON document describing glossary terms, endpoints, ports, and system-level definitions. Future anchor for cross-service synchronization.

Knowledge Base Document

Tenant-provided or admin-provided reference material ingested into the system for improved accuracy and retrieval.

Event Trigger

Any system-level event (call answered, lead created, transfer triggered, memory updated) that may initiate automation.

# 3. Voice & Telephony
Inbound Call

A call received through a tenant’s Twilio number routed into ChatStack Nginx → Orchestrator → Voice AI pipeline.

Outbound Call

A proactive call initiated by NeuroSphereAI (e.g., follow-up, quote request, renewal, campaign).

Webhook Request

Real-time Twilio event (media stream, status callback, SMS webhook) forwarded into NeuroSphereAI.

Twilio Number

The tenant-owned or platform-provisioned phone number used for inbound/outbound voice flows.

Transfer Rule

A configuration rule that transfers a call to a human agent when certain conditions are met (e.g., keyword match “agent,” policy type, or caller identity).

Screening Rule

Determines whether a call should be live-transferred, dropped, filtered, or redirected based on tenant logic.

Human Takeover

When a call transitions from AI to a human agent—triggered manually (caller request) or automatically (rule-based).

Call Summary

AI-generated summary of a completed call stored in AI-Memory.

Transcript Chunk

A segment of the real-time audio transcription, used for memory updates, processing, and LLM context.

# 4. Application Layers (Backend Architecture)
Orchestrator

The FastAPI service managing LLM calls, memory retrieval, prompt construction, audio routing, and conversation logic.

Admin Panel

The Flask web application providing configuration interface, greeting templates, transfer rules, personality sliders, and knowledge base upload.

AI-Memory Worker

A service handling write/read operations to the tenant-specific memory store.

Media Relay

Handles real-time audio streaming between Twilio, LLM, and TTS engines.

LLM Engine

The model endpoint (RunPod, local GPU, or cloud) powering the conversational responses.
Examples: Qwen2-7B, Falcon-H1-34B.

Nginx Reverse Proxy

Entry point for all HTTP/HTTPS traffic. Routes admin panel, orchestrator, and docs domains.

Container

A Docker runtime environment containing an isolated service (nginx, web, orchestrator, ai-memory).

Service

A logical component inside ChatStack, orchestrator, or AI-Memory running inside a container.

# 5. Data & Integrations
Identity Object

Stores who the caller is — name, phone number, email, and any known profile traits.

Preference Object

Caller preferences such as communication mode, preferred agent, language, quote type, or requested follow-up.

Relationship Object

Tagged connections between entities (e.g., Caller ↔ Policy, Spouse ↔ Insured).

Fact Object

Discrete data points gathered from calls (address, vehicle year, deductible, coverage tier).

Endpoint Token

Authentication token enabling communication with services (AI-Memory, Orchestrator, Admin).

Tenant ID

Unique identifier for each tenant in ChatStack and AI-Memory.

User ID

Phone-number–normalized identifier for caller memory storage.

Phone Hash

SHA-256 normalized representation of a phone number for indexing and privacy.

Memory Stream

The sequence of memory objects recorded throughout a call or session.

# 6. Domains & URLs
Public Docs Domain

https://docs.neurospherevoiceai.com — hosts all documentation for tenants and internal devs.

API Base URL

Tenant-specific base URL for API access.
Example: https://app.neurospherevoiceai.com/api/customers/{tenant_id}/

Admin Panel URL

Web interface for tenant settings and configuration.

Webhook URL

Endpoint Twilio uses to send calls, audio, SMS, and callback information.

Tenant Routing

Logic determining which tenant a call or API request belongs to based on request path, number, or headers.

# 7. Deployment Concepts
Environment

Can be prod, dev, or local.
Controls logging level, LLM temperature, and routing behavior.

Container Image

The built Docker image for each service (nginx, orchestrator, web, ai-memory).

Volume / Bind Mount

File system mapping from host → container.
Example: /opt/docs → /var/www/docs

Symlink

A pointer path linking one directory to another.
Used to map neurosphere-docs → /opt/docs.

Update Script (update.sh)

Automates pulling GitHub changes, recreating containers, refreshing nginx, and clearing build cache.

Health Check

Periodic status check for orchestrator, AI-Memory, and admin services.

# 8. Logging & Monitoring
Request ID

Unique identifier assigned to each inbound/outbound request for tracing.

Call ID

Unique identifier for a phone call session, used across Twilio, ChatStack, and AI-Memory.

Memory Index

Ordered or timestamped list of memory objects for a caller.

Event Log

Chronological record of system events, actions, and transitions.

Error Boundary

Containment mechanism ensuring errors in one layer do not cascade into others.
