# AI Customer Support Simulator

A multi-agent training & evaluation platform that simulates customer support interactions (chat, email, and voice) with realistic customers, live policies, and knowledge bases—then auto-scores responses, coaches the agent, and exports QA reports/macros to your help desk.

## What's new (2025-10)

- Voice simulator: smoother ASR with pause-based flush, codec fallbacks, and client WAV fallback for Deepgram compatibility
- Text-to-Speech (TTS) via TTSOpenAI with webhook-based delivery and automatic playback of customer replies
- Auto-send toggle and manual Send for voice input; “Customer is writing…” typing indicator on Voice
- End Session + View Live Scoring added to Chat, Voice, and Email; Chat page auto-starts the session after scenario selection
- Live scoring via WebSocket with progressive updates and final scoring summary [[memory:10052027]]
- Footer links: GitHub and LinkedIn

## ✨ What this application is

The AI Customer Support Simulator is a practical sandbox where support reps and AI agents practice handling realistic customer interactions across chat, email, and voice. It enforces policies and knowledge-base citations in real time, scores every interaction against a rubric, and generates actionable coaching plus exportable macros and reports. Think of it as a flight simulator for customer support.

## 💡 Benefits

- **Faster onboarding**: Ramp new reps with realistic scenarios and immediate feedback.
- **Consistent QA**: Objective, rubric-based scoring with transparent rationales.
- **Policy compliance**: Live guardrails and KB-citation enforcement reduce risk.
- **Skill development**: Coach mode nudges best practices while preserving autonomy.
- **Operational insights**: Analytics highlight knowledge gaps and process friction.
- **Reusable outputs**: Export macros, KB delta proposals, and LMS packages.

## 🧭 Where this can be applied

- **Rep onboarding and upskilling** (Tier 1–3)
- **Vendor/BPO quality management**
- **Continuous training after policy or product changes**
- **Regulated industries** (enforcement + auditable scoring)
- **AI agent evaluation** before production rollout
- **LMS content generation** (SCORM/xAPI packages)

## 🤖 AI Agents Framework Choice: CrewAI

This simulator uses **CrewAI** to orchestrate multiple specialized agents (customer, triage, knowledge, compliance, coaching, supervisor, scribe, scoring). CrewAI provides a clear mental model (roles → tasks → tools) and predictable execution that maps well to customer support workflows.

### 🔬 Framework comparison (HTML matrix)

<table>
  <thead>
    <tr>
      <th>Framework</th>
      <th>Orchestration Style</th>
      <th>Ideal Use</th>
      <th>Key Strengths</th>
      <th>Trade‑offs</th>
      <th>Maturity & Ecosystem</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>CrewAI</strong></td>
      <td>Role/task-based multi‑agent crews with tools</td>
      <td>Team-of-agents with clear responsibilities</td>
      <td>Simple mental model, good ergonomics, Python-native, active community</td>
      <td>Less graph/state-machine expressiveness than LangGraph</td>
      <td>Growing, strong examples and tutorials</td>
    </tr>
    <tr>
      <td><strong>LangGraph</strong> (LangChain)</td>
      <td>Graph/DAG state machines</td>
      <td>Deterministic flows, retries, tool calling at scale</td>
      <td>Robust control, checkpoints, observability integrations</td>
      <td>Higher complexity; steeper learning curve</td>
      <td>Very mature ecosystem, enterprise adoption</td>
    </tr>
    <tr>
      <td><strong>AutoGen</strong></td>
      <td>Conversable agents with messaging loops</td>
      <td>Research, agent-to-agent collaboration patterns</td>
      <td>Flexible dialogues, multi-turn coordination</td>
      <td>Can be chat-loop heavy; more glue code for guardrails</td>
      <td>Active, research-friendly community</td>
    </tr>
    <tr>
      <td><strong>Haystack Agents</strong></td>
      <td>Pipeline-first with agent capabilities</td>
      <td>RAG-focused applications</td>
      <td>Solid RAG primitives, search-first design</td>
      <td>Less opinionated multi-agent orchestration</td>
      <td>Mature in RAG; smaller agent footprint</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="6" style="text-align:left; font-size: 0.9em;">
        Notes: All frameworks are capable; the best choice depends on control requirements, team expertise, and ecosystem fit.
      </td>
    </tr>
  </tfoot>
  </table>

### ✅ Why CrewAI for this project

- **Maps to support orgs**: Roles (triage, compliance, coach, supervisor) mirror real teams.
- **Readable & maintainable**: Clear roles/tasks keep complex behavior understandable.
- **Rapid iteration**: Quick to compose new agents/tools for experiments.
- **Python everywhere**: Works smoothly with FastAPI, Celery, and analytics stack.
- **Enough structure, not too rigid**: Balances control with developer velocity.

## 🚀 Quick Start

### Prerequisites

- Docker & Docker Compose
- Node.js 18+ 
- Python 3.11+
- Git

### Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd ai-customer-support-simulator
   ```

2. **Install dependencies**
   ```bash
   npm run setup
   ```

3. **Configure environment**
   ```bash
   cp env.example .env
   # Edit .env with your configuration
   ```

4. **Start development environment**
   ```bash
   npm run dev
   ```

5. **Access the application**
   - Frontend: http://localhost:3000
   - API Gateway: http://localhost:3001
   - API Docs: http://localhost:3001/api
   - Orchestrator: http://localhost:8000
   - MinIO Console: http://localhost:9001

### Voice setup (ASR + TTS)

- Open the app and use the API Key modal to enter your keys (OpenAI, Deepgram, TTS)
- Or set the required env vars in `.env` (see Configuration below)
- Microphone permission is required in the browser for Voice

Notes:
- ASR uses Deepgram (best with `audio/webm;codecs=opus` or `audio/ogg;codecs=opus`). The app falls back to `audio/webm` and client-side `WAV PCM 16k mono` if needed.
- TTS uses TTSOpenAI with webhook completion; audio is pushed back to the session and auto-plays on customer replies.

## 🏗️ Architecture

### Monorepo Structure

```
ai-customer-support-simulator/
├── apps/
│   ├── frontend/          # Next.js 14 + React 18
│   ├── gateway/           # NestJS API Gateway
│   ├── orchestrator/      # FastAPI + CrewAI
│   └── workers/           # Python Celery Workers
├── packages/
│   └── sdk/              # Shared TypeScript SDK
└── docker-compose.dev.yml
```

### Services

- **Frontend**: Next.js 14 with TypeScript, Tailwind CSS, shadcn/ui
- **API Gateway**: NestJS with OpenAPI 3.1, RBAC, rate limiting
- **Orchestrator**: FastAPI with CrewAI multi-agent orchestration
- **Workers**: Python Celery workers for background tasks
- **Database**: PostgreSQL with pgvector for embeddings
- **Cache**: Redis for session state and queues
- **Event Bus**: NATS for real-time events
- **Storage**: MinIO (S3-compatible) for files and reports

### Multi-Agent System

The platform uses CrewAI to orchestrate multiple specialized agents:

- **Customer**: Persona-driven customer simulation
- **Triage**: Issue categorization and routing
- **Knowledge Manager**: KB search and citation enforcement
- **Compliance Officer**: Policy enforcement and redaction
- **De-escalation Coach**: Sentiment monitoring and coaching
- **Supervisor**: Escalation decisions and overrides
- **Session Scribe**: Transcript and case summary
- **Scoring Analyst**: Rubric evaluation and feedback

## 🎯 Features

### Training & Simulation
- **Scenario Builder**: Create realistic customer scenarios
- **Customer Personas**: Configurable customer types and behaviors
- **Real-time Policy Guardrails**: Live compliance and KB enforcement
- **Multi-channel Support**: Chat, email, and voice interactions

### Voice channel specifics
- **Smoother ASR**: Client-side utterance buffering with ~0.9–1.2s pause-based flush, 13s max utterance, 4s MediaRecorder timeslice
- **Codec fallbacks**: `audio/webm;codecs=opus` → `audio/ogg;codecs=opus` → `audio/webm` → client-side `WAV (PCM 16k mono)` for Deepgram
- **Auto-send toggle**: Choose between auto-send after pause or manual Send for voice transcripts
- **Typing indicator**: “Customer is writing…” on Voice; disappears precisely when the customer message arrives
- **TTS replies**: Customer replies synthesized via TTSOpenAI and auto-played in the UI

### Assessment & Coaching
- **Auto-scoring**: Rubric-based evaluation with explanations
- **Coach Mode**: Live hints and suggestions
- **Macro Generator**: Automated response template creation
- **KB Delta Proposals**: Knowledge base improvement suggestions

### Integrations & Export
- **Help Desk**: Zendesk, Intercom, Salesforce Service Cloud
- **LMS**: SCORM/xAPI package export
- **Analytics**: Progress tracking and performance dashboards

### UI & UX
- **Session controls**: End Session + View Live Scoring on Chat, Voice, and Email pages
- **Chat page**: “Start This Scenario” now auto-starts the session; View Live Scoring sits next to End Session
- **Email page**: First trainee message auto-transitions the session to RUNNING so Send works as expected
- **Footer**: GitHub and LinkedIn links
- **Mobile**: Responsive layouts with Tailwind; controls wrap on smaller screens


---

Built with ❤️ using Next.js, NestJS, FastAPI, CrewAI, and modern web technologies.
