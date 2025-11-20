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
A logical microservice within the platform running inside one or more containers.

---

# 5. Data & Integrations

### **Identity Object**
Stores caller identity: name, phone number, email, and profile traits.

### **Preference Object**
Caller preferences (communication method, language, preferred agent, quote type, follow-up preferences).

### **Relationship Object**
Entity relationships (e.g., Caller ↔ Policy, Spouse ↔ Insured).

### **Fact Object**
Discrete, structured data extracted during calls (address, vehicle year, coverage tier, deductible preferences).

### **Endpoint Token**
Authentication token for secure API communication (AI-Memory, Orchestrator, Admin).

### **Tenant ID**
Unique identifier for each tenant across all services.

### **User ID**
Normalized representation of a phone number used for memory storage in AI-Memory.

### **Phone Hash**
SHA-256 hashed phone number for index and privacy.

### **Memory Stream**
Chronological list of memory objects for a caller or session.

---

# 6. Domains & URLs

### **Public Docs Domain**
`https://docs.neurospherevoiceai.com` — hosts platform documentation.

### **API Base URL**
Tenant-specific base for API access.  
Example:  
`https://app.neurospherevoiceai.com/api/customers/{tenant_id}/`

### **Admin Panel URL**
The configuration interface for each tenant.

### **Webhook URL**
HTTP endpoint Twilio uses for streaming calls, status callbacks, and SMS events.

### **Tenant Routing**
Logic that determines which tenant a call or request belongs to.

---

# 7. Deployment Concepts

### **Environment**
Execution mode: `prod`, `dev`, or `local`.  
Impacts logging, LLM temperature, routing, and behaviors.

### **Container Image**
The Docker image for a service (nginx, orchestrator, web, or ai-memory).

### **Volume / Bind Mount**
File system mapping host → container.  
Example: `/opt/docs → /var/www/docs`.

### **Symlink**
Symbolic link that allows multiple paths to reference the same directory.  
Used for mapping neurosphere-docs → `/opt/docs`.

### **Update Script (`update.sh`)**
Automates:
- Pulling latest ChatStack + AI-Memory code  
- Rebuilding orchestrator/web  
- Restarting nginx  
- Cleaning Docker images  

### **Health Check**
Periodic service validation for orchestrator, AI-Memory, and admin panel.

---

# 8. Logging & Monitoring

### **Request ID**
Unique ID assigned to inbound/outbound requests for debugging and traceability.

### **Call ID**
Unique identifier for a phone call session.

### **Memory Index**
Ordered list of memory objects associated with a caller.

### **Event Log**
Chronological sequence of system events.

### **Error Boundary**
Architecture component ensuring one subsystem’s error does not crash others.

---

# ⭐ END OF GLOSSARY
