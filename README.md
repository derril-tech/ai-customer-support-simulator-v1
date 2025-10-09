# AI Customer Support Simulator

A multi-agent training & evaluation platform that simulates customer support interactions (chat, email, and voice) with realistic customers, live policies, and knowledge bases—then auto-scores responses, coaches the agent, and exports QA reports/macros to your help desk.

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

---

Built with ❤️ by Derril Filemon 
using Next.js, NestJS, FastAPI, CrewAI, and modern web technologies.

