# Security Baseline – Delta vs Vanilla CrewAI

This document records every meaningful security control added to the default `crewai create crew` output.

## Implemented controls

1. **Prompts as versioned code**  
   Location: `prompts/*.md`  
   Why: Prompts are high-risk attack surface → must be reviewed, diffed, signed like any other code.

2. **Structured per-run audit logging (JSONL)**  
   Location: `src/secured_research_crew/callbacks.py` → `logs/runs/*.jsonl`  
   Why: Forensic visibility of every LLM thought, tool call, argument, observation.

3. **Explicit tool allow-list factory**  
   Location: `src/secured_research_crew/tools.py`  
   Why: Prevents accidental / malicious registration of dangerous tools.

4. **Hard iteration limit (max_iter=8)**  
   Location: agent creation in `crew.py`  
   Why: Rudimentary protection against resource exhaustion & infinite loops.

5. **Delegation explicitly disabled**  
   Location: `allow_delegation=False` on agents  
   Why: Reduces inter-agent privilege escalation surface in phase 1.

6. **Strict environment variable hygiene**  
   `.env.example` + `.gitignore` excludes `.env`  
   Why: Prevents credential leakage.

7. **Attack demonstration folder with evidence**  
   Location: `attacks/`  
   Why: Proves understanding of OWASP ASI01–03 failure modes.

## Planned future hardening (phases 2–6)

- Prompt signing & integrity checks
- Input/output guardrails (NeMo, Guardrails AI)
- Sandboxed tool execution (E2B)
- Runtime policy enforcement (OPA / Cedar)
- Automated red-teaming in CI (PyRIT)
- Agent registry & approval workflow
