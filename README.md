# Enterprise LLM Broker Matching & Routing Engine

An enterprise-grade, compliance-driven AI routing pipeline that consumes unstructured user search intents and matches them with verified mortgage and financial brokers. This system bridges production cloud data lakes with advanced LLM reasoning and real-time observability tracking.

## 🏗️ System Architecture & Workflow
The engine operates across four distinct engineering boundaries:
1. **The Ingestion & Data Engineering Layer:** Extracts records from relational database tables/local data structures, normalizes messy string blocks, and enforces data quality constraints (e.g., stripping NMLS IDs).
2. **Compliance & Guardrail Logic:** Programmatically filters candidate broker arrays against strict geographic licensing restrictions before payload handoff, optimizing latency and reducing token consumption by up to 80%.
3. **LLM Reasoning Layer (Claude Sonnet 5):** Leverages advanced contextual prompts, securely skipping native model execution reasoning (Thinking Blocks) to isolate deterministic final routing responses.
4. **Telemetry & Production Observability (Langfuse):** Complete OpenTelemetry wrapping across every sub-span execution to monitor cost, prompt latency, input/output token allocation, and routing accuracy.

## 🛠️ Tech Stack
- **Languages:** Python (Pandas, OpenTelemetry)
- **AI Engine:** Anthropic API (Claude 3.5 Sonnet Engine)
- **Observability Framework:** Langfuse Cloud Tracking
- **Data Architecture Foundations:** Optimized for Databricks Lakehouse Environments & Local Workspaces

## 🚀 Core Implementation Preview
```python
# Open trace context for production monitoring
with langfuse.start_as_current_observation(name="enterprise-claude-match") as trace:
    # Broadcast session parameters to all sub-tasks
    with propagate_attributes(session_id=session_id, metadata={"target_state": target_state}):
        # Pre-filter compliance licensing via programmatic constraints
        state_candidates = valid_brokers[valid_brokers['state_licenses'].str.contains(target_state)]
        
        # Execute isolated generation call
        with langfuse.start_as_current_observation(as_type="generation", name="claude-broker-match") as gen:
            # Code securely bypasses raw thinking blocks and extracts targeted text output
            output_text = next(block.text for block in response.content if hasattr(block, "text"))
