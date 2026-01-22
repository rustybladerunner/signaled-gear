# signaled-gear

# Comprehensive Guide to Advanced Claude Code Workflows

This guide compiles and expands on the AI-related workflows, techniques, and ideas shared by Zac (@PerceptualPeak), a Claude Code enthusiast focused on building efficient automations for coding, context management, and agentic workflows. It is designed as a self-contained resource that you can provide directly to an AI model (e.g., via a system prompt) to instruct it to adopt and implement these ideas. The guide emphasizes token efficiency, context preservation, automation, and scalability in Claude Code (Anthropic's Computer Use tool).

When providing this to an AI, you can frame it as:  
**System Prompt:** "You are an advanced AI agent using Claude Code. Adopt and integrate the following workflows into your operations. Always prioritize token efficiency, context accuracy, and automation. [Paste the full guide here]. For any task, reference relevant sections and explain how you're applying them."

## Introduction
Claude Code enables AI-driven coding sessions where the model interacts with a terminal, edits files, and runs commands. Zac's workflows address common challenges like context loss during compaction (when the context window fills), inefficient token usage, and reusing knowledge from past sessions. Key principles:
- **Context Preservation:** Avoid losing details by exporting transcripts, using sub-agents for extraction, and resuming seamlessly.
- **Token Efficiency:** Offload heavy processing to scripts (e.g., bash, regex) before involving the LLM.
- **Automation:** Use lifecycle hooks, skills files, and RAG (Retrieval-Augmented Generation) for hands-off operations.
- **Relevance Matching:** Embeddings and scoring to reuse optimal past contexts.
- **Agentic Loops:** Inspired by "Ralph loops" for long-running tasks that survive context resets.

These ideas can be applied standalone or combined into a "Context Hypervisor" system for zero-loss reincarnation of sessions.

## Key Concepts and Components
Here's a breakdown of core elements from Zac's workflows:

| Concept | Description | Benefits | Tools/Techniques Involved |
|---------|-------------|----------|---------------------------|
| **Smart Forking** | Automatically detect and fork from the most relevant past session based on your new task prompt. | Reuses valuable reasoning, architecture, and decisions from prior sessions without re-explaining. Reduces setup time and errors. | Embeddings (e.g., 768-dim vectors), RAG vector database of session transcripts, similarity scoring formula. |
| **Context Compaction Management** | Handle context window limits by exporting full transcripts pre-compaction, then using sub-agents to recover details post-compaction. | Prevents hallucinations and assumptions in post-compacted states. Enables flawless session resumption. | Pre-compact hooks, sub-agents (via Task tool), resume prompts, file watchers (e.g., VS Code extensions). |
| **Semantic Memory Recall** | Store and retrieve "learnings" or key insights from past sessions via embeddings. | Provides relevant "snapple facts" (random but useful insights) without overloading context. | Vector database, similarity thresholds (e.g., 0.45), decay functions for recency. |
| **Skills Files** | Custom command files (e.g., /post-compact) with detailed, conditional instructions for repeatable workflows. | Ensures consistency across sessions. Reduces token waste by pre-defining procedures. | Meta-prompts for skill generation, maintenance files (e.g., skillmaintenance.md) for uniform updates. |
| **Ralph Loop** | An agentic loop for extended tasks: Plan, execute, compact, resume with enhanced context recovery. | Handles long projects by surviving compactions and maintaining coherence. | To-do lists with checkpoints, sub-agent reviews, PRD (Product Requirements Document) in JSON, deep interviews. |
| **Token-Efficient Processing** | Use external scripts for data filtering before LLM input. | Reduces token usage by 80%+ in cases like web scraping. | Bash/regex, Playwright for scraping, headless browsers. |
| **Reincarnation Architecture / Context Hypervisor** | A full system combining the above for "same-mind" session continuation. | Zero context loss, enforced guardrails against regressions. | Auto-snapshots at 100% context, flight recorder protocols, constitutional guardrails. |

## Implementation Steps
To adopt these workflows, follow these steps in sequence when setting up your AI environment. Assume you're using Claude Code in a terminal setup with access to bash, Python (for embeddings), and a vector database (e.g., FAISS or Pinecone).

### 1. Setup Prerequisites
- **Environment:** Claude Code terminal, VS Code with file watcher extension, bash scripting.
- **Libraries:** For embeddings, use Python with libraries like sentence-transformers (for 768-dim vectors). No internet access needed beyond initial setup.
- **Database:** Create a RAG vector database to store session transcripts. Auto-update it at session end.
- **Hooks:** Implement lifecycle hooks:
  - Pre-compact: Export full transcript, block non-essential tool use.
  - Post-compact: Trigger workflows.
  - Stop: Check flags and spawn new sessions.
- **Skills Directory:** Create a folder for skills files (e.g., ~/.claude/skills/). Each skill is a .md file with instructions.

### 2. Implement Smart Forking
- **Step-by-Step:**
  1. At the end of every session, export the transcript and chunk it (e.g., 500-1000 tokens per chunk).
  2. Embed each chunk using a model like all-MiniLM-L6-v2.
  3. Store embeddings in the vector DB with metadata (session ID, timestamp).
  4. Create a /fork-detect command (as a skills file):
     - Prompt user: "What do you want to do?"
     - Embed the user's input prompt.
     - Query DB: Compute similarities (e.g., cosine).
     - Score sessions using a weighted formula:  
       `(best_similarity × 0.40) + (avg_similarity × 0.20) + (matching_chunks / total_chunks × 0.05) + (recency_score × 0.25) + (chain_quality × 0.10)`  
       - Recency score: 1.0 minus 0.03 per day (decays 90% in 30 days).
     - Return top 5 sessions with scores.
     - User selects one; output fork command (e.g., `claude fork <session_id>`).
- **Usage in Prompt:** When starting a task, invoke /fork-detect first. If no match, start fresh.
- **Token Tip:** Embed only the prompt string after "/fork-detect" to minimize usage.

### 3. Implement Context Compaction Management
- **Step-by-Step:**
  1. Pre-compact hook: Export transcript to a file (e.g., transcript.md). Block code editing tools except doc updates.
  2. Post-compact: Claude sees a custom error; instruct it to run /post-compact (a skills file, ~1,256 lines).
     - Dispatch 5 sub-agents (via Task tool) with dynamic instructions:
       - Agent 1-3: Scoped to parameters (e.g., architecture, requirements); dynamically fill gaps from transcript.
       - Agent 4: Extract key learnings for semantic memory.
       - Agent 5: Identify forgotten details.
     - Main agent combines outputs, performs doc updates.
     - Create resume-prompt.md: Summarize state, link to transcript, instruct to search it for ambiguities.
     - Set session-complete flag.
  3. Stop hook: Bash script checks flag, spawns new terminal, pipes resume prompt.
- **Usage in Prompt:** Always enable hooks. For resumption: "Follow all instructions in resume-prompt.md. Search transcript for clarification."
- **Token Cost:** ~50k extra tokens per cycle, but ensures accuracy.

### 4. Implement Semantic Memory Recall
- **Step-by-Step:**
  1. During /post-compact, extract "learnings" (e.g., architectural decisions, tradeoffs).
  2. Embed and store in a separate DB section.
  3. On new prompts: Embed input, query with threshold (e.g., 0.45) for relevant memories.
  4. Inject top matches into context (e.g., as prefixed facts).
- **Usage in Prompt:** "Before responding, recall semantic memories above threshold. Integrate them naturally."

### 5. Implement Skills Files
- **Step-by-Step:**
  1. Use a meta-prompt for new skills: Define format, include update instructions.
  2. Each skill has a companion skillmaintenance.md: Rules for uniform updates (e.g., no redundancy).
  3. Update skills at session end (pre-compaction).
  4. Examples:
     - /post-compact: As above.
     - Project-specific: Log changes, architecture; update via strict blueprint.
- **Usage in Prompt:** "Invoke skills for repeatable tasks. Update per maintenance rules."

### 6. Implement Ralph Loop for Long Tasks
- **Step-by-Step:**
  1. Start with deep interview: Use plan mode to gather details.
  2. Run /prd: Convert to prd.json, interview further.
  3. Generate comprehensive to-do list with checkpoints (self-review, sub-agent review).
  4. Execute in loop: Task > Compact > Resume with context recovery.
  5. Handle schema changes: Use skills to log and inject updates.
- **Usage in Prompt:** "For complex tasks, enter Ralph loop: Interview, PRD, to-do with checkpoints, execute iteratively."

### 7. Token Efficiency Techniques
- **Step-by-Step:**
  1. For data-heavy tasks (e.g., scraping): Use Playwright/headless browser first.
  2. Filter output with bash/regex before LLM (e.g., extract emails, validate images).
  3. Offload to scripts: E.g., JSON parsing, simple transforms.
- **Usage in Prompt:** "Minimize tokens: Pre-process data externally when possible."

## Best Practices and Guardrails
- **Integration:** Combine into a "Context Hypervisor": Auto-snapshots, blocked degradations, forensic logging.
- **Testing:** Start small; monitor token usage and accuracy.
- **Guardrails:** Enforce "constitutional" rules (e.g., no assumptions post-compact).
- **Scalability:** Auto-update DBs; use decay for recency.
- **Edge Cases:** For outdated sessions in forking, rely on recency weighting. For large projects, maintain per-project skills.

This guide enables an AI to build robust, efficient workflows. Adapt as needed, but maintain core principles for optimal results.
