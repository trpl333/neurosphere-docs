# NeuroSphereAI Glossary (Draft)

This file will contain the official glossary of all terms used across the NeuroSphere platform.

# 📘 NeuroSphereAI Global Glossary
_Last updated: 2025-11-20_

This glossary defines all terminology used across the NeuroSphereAI ecosystem including ChatStack, AI-Memory, Orchestrator, Admin Panel, Voice AI, Telephony, Deployment Infrastructure, and API Services.

---

# 1. Core Concepts

### **Tenant**
A business entity using the NeuroSphereAI platform. Each tenant has isolated data, phone numbers, memory storage, configuration, and admin access.  
*Example: Peterson Insurance.*

### **Customer**
The internal database term equivalent to *Tenant*. Used throughout ChatStack.

### **Customer User**
A client or prospect of the tenant (e.g., policyholder, lead, caller).  
Not the tenant’s staff. Identified primarily by phone number.

### **Caller**
Any inbound phone user contacting a tenant phone number. If recognized, mapped to a Customer User profile in AI-Memory.

### **Agent**
A human staff member of the tenant (e.g., licensed insurance agent).  
May receive transferred calls from the Voice AI.

### **Admin User**
A tenant staff member with access to the Admin Panel for configuration and management.

### **Session**
A single conversational episode — call, chat, or automated workflow.  
Tracks state, transcription, memory usage, and interaction flow.

### **Interaction**
Any meaningful exchange between the user and the system — spoken message, LLM response, function call, memory update, or prompt cycle.

---

# 2. NeuroSphereAI System Entities

### **AI-Memory**
Subsystem responsible for long-term, structured, recallable memory for each caller and tenant.

### **Memory Object**
A structured item stored in AI-Memory — facts, preferences, relationships, identity attributes, call summaries, and more.

### **Memory Slice**
A filtered set of memory objects retrieved for a specific prompt or call.

### **Personality Sliders**
Configurable attributes controlling tone, empathy, pacing, confidence, humor, and conversational style for the Voice AI.

### **Prompt Template**
Reusable instructions that define how the LLM performs tasks.  
Templates can embed memory slices, call context, tenant configuration, business logic, and constraints.

### **Manifest**
A versioned JSON document describing glossary terms, endpoints, ports, and platform-level definitions. Future anchor for cross-system synchronization.

### **Knowledge Base Document**
Tenant- or admin-provided reference materials (PDF, text, Markdown) ingested by the system for retrieval and context.

### **Event Trigger**
A system event (call started, lead created, memory updated, transfer invoked) that initiates logic or automation.

---

# 3. Voice & Telephony

### **Inbound Call**
A call received through a tenant’s Twilio number and routed to ChatStack → Orchestrator → Voice AI pipeline.

### **Outbound Call**
A proactive call placed by NeuroSphereAI (e.g., follow-up, quote request, renewal, campaigns).

### **Webhook Request**
Real-time HTTP request from Twilio carrying audio streams, call events, SMS messages, or status callbacks.

### **Twilio Number**
Tenant-owned or platform-provisioned phone number used for inbound and outbound interactions.

### **Transfer Rule**
Configuration that hands off the call to a human agent based on keywords (“agent,” “representative”), intent, policy type, or caller identity.

### **Screening Rule**
Logic that determines whether a call should be transferred, blocked, routed to voicemail, or assigned to the Voice AI.

### **Human Takeover**
When a call transitions from AI → Human agent.  
Triggered by caller request or rule-based logic.

### **Call Summary**
AI-generated summary stored in AI-Memory after a call completes.

### **Transcript Chunk**
A segment of real-time audio transcription used as input to the LLM and AI-Memory.

---

# 4. Application Layers (Backend Architecture)

### **Orchestrator**
FastAPI backend service managing LLM calls, prompt construction, memory retrieval, streaming audio routing, and conversation logic.

### **Admin Panel**
Flask-based interface for greetings, transfer rules, sliders, configuration, and knowledge base management.

### **AI-Memory Worker**
Service that writes and retrieves memory objects for each caller and tenant.

### **Media Relay**
Handles real-time audio streaming between Twilio, LLM, and text-to-speech (TTS) engine.

### **LLM Engine**
Model endpoint that generates conversational responses.  
Examples: *Qwen2-7B*, *Falcon-H1-34B*.

### **Nginx Reverse Proxy**
Entry point for all web and API traffic.  
Routes Admin Panel, Orchestrator, Docs, and other services.

### **Container**
A Docker runtime environment for an isolated service (nginx, web, orchestrator, ai-memory).

### **Service**
A logical microservice within the pla
