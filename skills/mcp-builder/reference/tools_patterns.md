# PATs - Patterns for Agentic Tools
# Design patterns for building quality tools for AI agents
# https://arcade.dev/patterns

## GUIDING PRINCIPLE

"Your agents are only as good as your tools."

Tools are how agents interact with the world. When tools are well-designed, 
orchestration stays simple and agents behave predictably. When tools are sloppy, 
the orchestration layer has to compensate - and it never does it well. 
These patterns capture what works, so you don't have to figure it out the hard way.

---

## THREE AXES OF TOOL CLASSIFICATION

Every tool exists at coordinates across these dimensions.

### AXIS 1: MATURITY
How sophisticated is the tool's implementation?

1. ATOMIC - Single-operation tools that do one thing. Direct API wrappers.
2. ENHANCED - Built-in validation, defaults, error handling. Production-ready.
3. COMPOSITE - Bundle multiple operations into coherent workflows. Task-oriented.
4. ORCHESTRATED - Coordinate other tools, manage state across calls, handle complex flows.

### AXIS 2: INTEGRATION TYPE
What kind of system does the tool connect to?

1. API - REST, GraphQL, gRPC services.
2. DATABASE - SQL, NoSQL, vector stores. Direct data access.
3. FILE_SYSTEM - Local files, cloud storage, document processing.
4. SYSTEM - Shell commands, OS operations, infrastructure control.

### AXIS 3: ACCESS PATTERN
How does the tool execute and return results?

1. SYNCHRONOUS - Request-response. Agent waits for completion.
2. ASYNCHRONOUS - Fire and poll. Long-running operations with job IDs.
3. STREAMING - Incremental delivery. Results arrive as they're ready.
4. EVENT_DRIVEN - Push-based. Tool notifies when something happens.

---

## CROSS-CUTTING CONCERNS

These principles apply regardless of which axis or pattern you're working with.

### MACHINE EXPERIENCE
"Design for the LLM, not the human."
Tool descriptions, parameter names, and error messages should be optimized for 
machine comprehension. Clear, unambiguous, structured.

### TOOL DAGs
"Hint at what comes next."
Tools should suggest related operations and dependencies. Help agents build 
workflows without trial and error.

### ERROR-GUIDED RECOVERY
"Errors should teach, not just fail."
When things go wrong, provide actionable guidance. Suggest retries, alternatives, 
or parameter adjustments.

### SECURITY BOUNDARIES
"Prompts express intent, code enforces rules."
Never trust the agent to enforce security. Authorization, validation, and audit 
happen in the tool layer.

---

## PATTERN CATEGORIES

### 1. TOOL (4 patterns) - "What kind of tool is this?"

The atomic unit. What IS a tool? The fundamental building blocks of agent tooling.

1. TOOL
   The atomic callable unit that an agent can invoke to perform work.
   - Named function with typed parameters and return type
   - Clear description for LLM comprehension
   - Documented side effects

2. QUERY_TOOL
   A read-only tool that retrieves data without side effects.
   - Safe to retry
   - Results can be cached
   - Parallelizable

3. COMMAND_TOOL
   A tool that performs actions with side effects.
   - Modifies state or triggers external actions
   - May require confirmation for destructive operations
   - Document irreversibility clearly

4. DISCOVERY_TOOL
   A tool that reveals available operations, schema, or capabilities.
   - list_tables(), describe_schema(), get_capabilities()
   - Essential for schema-on-read scenarios

---

### 2. TOOL INTERFACE (7 patterns) - "How do agents call this tool?"

The contract between agent and tool. How agents see, understand, and call tools.

5. TOOL_DESCRIPTION
   Write descriptions optimized for LLM comprehension, not human reading.
   - Comprehensive: Everything the LLM needs to know
   - Include examples of valid inputs
   - Link to prerequisites and follow-ups

6. CONSTRAINED_INPUT
   Use enums, ranges, and validation to limit inputs to valid values.
   - Enums instead of free-form strings
   - Ranges for numbers (min/max)
   - Patterns for format validation

7. SMART_DEFAULTS
   Reduce required parameters by providing sensible defaults.
   - Context-aware defaults (current user, time)
   - Default to most common case
   - Document defaults explicitly

8. NATURAL_IDENTIFIER
   Accept human-friendly identifiers and resolve them internally.
   - Accept email, username, display name
   - Resolve to system ID internally
   - Handle ambiguity with suggestions

9. MUTUAL_EXCLUSIVITY
   Enforce "exactly one of X or Y" parameter constraints.
   - Document valid combinations
   - Validate early with clear errors
   - Provide examples

10. PERFORMANCE_HINT
    Guide agents toward efficient usage patterns.
    - "Prefer conversation_id (faster)"
    - "For batch operations, use batch_get"
    - Warn about expensive operations

11. PARAMETER_COERCION
    Accept flexible input formats and normalize internally.
    - Dates: ISO, relative, natural language
    - Numbers: string or numeric
    - Lists: single item or array

---

### 3. TOOL DISCOVERY (5 patterns) - "How do agents find this tool?"

Navigation and selection in a tool ecosystem. How agents find the right tool.

12. TOOL_REGISTRY
    Provide a catalog of available tools with their capabilities.
    - List all tools with descriptions
    - Categorize by domain or function
    - Include authentication requirements

13. SCHEMA_EXPLORER
    Progressively reveal structure through layered discovery.
    - Layer 1: list_tables() - What exists
    - Layer 2: describe_table() - Field details
    - Layer 3: sample_data() - See examples

14. DEPENDENCY_HINT
    Embed "call X before Y" guidance in tool descriptions.
    - Prerequisites: "If you don't have X, call Y first"
    - Follow-ups: "After this, you might want Z"
    - Include in error messages too

15. CAPABILITY_MATCHING
    Find tools by intent or capability, not just name.
    - Semantic search across descriptions
    - Capability tags
    - Action mapping

16. HEALTH_CHECK
    Verify tool availability before relying on it.
    - Fast availability probe
    - Backend status
    - Degraded mode indication

---

### 4. TOOL COMPOSITION (6 patterns) - "How do tools combine?"

Building complex operations from simple tools. How tools combine and chain.

17. ABSTRACTION_LADDER
    Provide tools at multiple levels of granularity.
    - Low: create_file(path, bytes)
    - Mid: create_document(title, content)
    - High: draft_report(topic)

18. TASK_BUNDLE
    Combine multiple operations into a single tool.
    - dm_user() = search + resolve + send
    - Handle intermediate errors
    - Name after task, not steps

19. BATCH_OPERATION
    Process multiple items in a single tool call.
    - Accept arrays
    - Return per-item results
    - Handle partial failures

20. OPERATION_MODE
    Provide different modes for different access patterns.
    - Explore: Read-only, safe
    - Preview: Show what would happen
    - Execute: Actually perform

21. TOOL_CHAIN
    Explicitly define sequences of tool calls.
    - Ordered step definitions
    - Data passing between steps
    - Checkpoints for resume

22. SCATTER_GATHER_TOOL
    Fan out to multiple sources, then combine results.
    - Query in parallel
    - Gather and merge results
    - Handle partial failures

---

### 5. TOOL EXECUTION (6 patterns) - "How does this tool execute?"

The internal processing patterns. How tools do their actual work.

23. SYNCHRONOUS_EXECUTION
    Immediate request-response execution.
    - Returns result in same call
    - Bounded time (seconds)
    - Most tools should be synchronous

24. ASYNC_JOB
    Handle long-running operations with job IDs and polling.
    - Start: Return job_id immediately
    - Poll: check_job_status()
    - Result: get_job_result()

25. IDEMPOTENT_OPERATION
    Make operations safe to retry with identical results.
    - Accept idempotency key
    - Cache and return same result
    - Document guarantee

26. TRANSACTIONAL_BOUNDARY
    Ensure all-or-nothing execution for multi-step operations.
    - All steps succeed or all roll back
    - Data integrity maintained

27. COMPENSATION_HANDLER
    Undo completed steps when a multi-step operation fails.
    - Track completed steps
    - Define compensating actions
    - Execute in reverse order

28. TIMEOUT_BOUNDARY
    Define maximum execution time with graceful termination.
    - Set appropriate limits
    - Return partial results if possible
    - Clear timeout errors

---

### 6. TOOL OUTPUT (7 patterns) - "How does this tool return results?"

Communicating results back to agents. How tools return useful information.

29. RESPONSE_SHAPER
    Transform raw API responses into agent-friendly formats.
    - Flatten nested structures
    - Select relevant fields only
    - Rename for clarity

30. TOKEN_EFFICIENT_RESPONSE
    Minimize response size while preserving essential information.
    - Essential fields only
    - Truncate long text
    - Count, don't list

31. PAGINATED_RESULT
    Handle large result sets with cursor-based pagination.
    - Cursor-based (not page numbers)
    - has_more flag
    - Include next-page hint

32. PROGRESSIVE_DETAIL
    Return summary by default, full detail on request.
    - Summary mode (default)
    - Full mode on request
    - Selective expansion flags

33. NEXT_ACTION_HINT
    Suggest what the agent should do next.
    - Suggested tools with parameters
    - Required data for next step
    - Alternative paths

34. GUI_URL
    Include links to view or edit results in a web interface.
    - View URL for the resource
    - Edit URL if applicable
    - Dashboard links

35. PARTIAL_SUCCESS
    Report mixed success/failure results for batch operations.
    - Per-item status
    - Summary statistics
    - Retry guidance for failures

---

### 7. TOOL CONTEXT (5 patterns) - "How is state managed?"

The MCP-native category. Stateful connections and context management.

36. IDENTITY_ANCHOR
    Establish user identity and context at the start of a session.
    - who_am_i() returns user context
    - User ID, roles, permissions
    - Team and org context

37. SESSION_CONTEXT
    Maintain state across multiple tool calls in a conversation.
    - Persist context across calls
    - Set working project/scope
    - Expire stale sessions

38. RESOURCE_REFERENCE
    Point to external data by URI instead of embedding it.
    - URI-based references
    - Resolvable by tools
    - Include content type

39. CONTEXT_INJECTION
    Automatically inject relevant context the agent didn't request.
    - User context (team, permissions)
    - Temporal context (time, timezone)
    - Allow explicit overrides

40. CONTEXT_BOUNDARY
    Define the scope or boundaries of tool operations.
    - Root paths for file access
    - Tenant scope for data
    - Permission scope limits

---

### 8. TOOL RESILIENCE (6 patterns) - "How does this tool handle failures?"

Recovery, retry, and degradation patterns. How tools handle failures gracefully.

41. RECOVERY_GUIDE
    Provide actionable error messages that tell agents how to fix the problem.
    - What went wrong
    - Why it failed
    - How to fix (specific steps)

42. ERROR_CLASSIFICATION
    Distinguish between retryable and permanent failures.
    - Retryable: transient, will succeed later
    - Permanent: won't work without changes
    - Auth required: need to re-authenticate

43. CONFIRMATION_REQUEST
    Request clarification when input is ambiguous.
    - Return matches with details
    - Ask for selection
    - Provide exact call for each option

44. FUZZY_MATCH_THRESHOLD
    Auto-accept high-confidence matches, confirm uncertain ones.
    - >90%: auto-accept
    - 50-90%: return options
    - <50%: ask for different input

45. GRACEFUL_DEGRADATION
    Return partial results when full operation isn't possible.
    - Return what worked
    - Note what failed
    - Suggest remediation

46. FALLBACK_TOOL
    Provide alternative tools when the primary is unavailable.
    - Define fallback order
    - Switch transparently or suggest
    - Indicate when fallback used

---

### 9. TOOL SECURITY (4 patterns) - "How is access controlled?"

Trust, authorization, and data protection. How access is controlled.

47. SECRET_INJECTION
    Inject credentials at runtime, never passing them through the LLM.
    - Context-based injection
    - Never accept secrets as parameters
    - Secure storage

48. PERMISSION_GATE
    Enforce access control in code, not prompts.
    - Check permissions before executing
    - Log all denials
    - "Prompts express intent; code enforces rules"

49. SCOPE_DECLARATION
    Declare required OAuth scopes per tool.
    - Required scopes documented
    - Pre-check before execution
    - Clear missing scope errors

50. AUDIT_TRAIL
    Log all tool invocations for security and debugging.
    - What: tool, parameters (redacted)
    - Who: user, session
    - When: timestamp
    - Result: success/failure, duration

---

### 10. COMPOSITIONAL (4 patterns) - "What patterns apply across the system?"

Cross-cutting patterns that span multiple categories. System-wide concerns.

51. TOOL_GATEWAY
    Provide a unified interface to multiple tool backends.
    - Single entry point
    - Route to correct backend
    - Aggregate discovery

52. TOOL_ADAPTER
    Wrap legacy APIs as agent-friendly tools.
    - Hide legacy complexity
    - Add LLM-friendly descriptions
    - Shape responses appropriately

53. CANONICAL_TOOL_MODEL
    Use standard data models across the tool ecosystem.
    - Define canonical schemas (User, Task, Event)
    - Map to/from canonical form
    - Consistent naming

54. TOOL_VERSIONING
    Support multiple tool versions coexisting.
    - Version in name or metadata
    - Parallel support during migration
    - Deprecation with migration guide

---

## QUICK REFERENCE

When building a tool, consider:

1. WHAT kind of tool? → Tool, Query Tool, Command Tool, Discovery Tool
2. HOW called? → Description, Constraints, Defaults, Natural IDs
3. HOW found? → Registry, Schema, Dependencies, Matching
4. HOW composed? → Abstraction levels, Bundles, Batches, Chains
5. HOW executed? → Sync, Async, Idempotent, Transactional
6. HOW returned? → Shape, Efficiency, Pagination, Hints
7. HOW contextual? → Identity, Session, Resources, Boundaries
8. HOW resilient? → Recovery, Classification, Degradation, Fallback
9. HOW secure? → Secrets, Permissions, Scopes, Audit
10. HOW integrated? → Gateway, Adapter, Canonical, Versioning

---

## THE PARADIGM SHIFT

| Aspect | Traditional (2003) | Agent Tools (2024+) |
|--------|-------------------|---------------------|
| Consumer | Applications | AI Agents (LLMs) |
| State Model | Stateless messages | Stateful sessions (MCP) |
| Routing | Predetermined flows | Agent-selected (non-deterministic) |
| Error Handling | Dead letter queues | Recovery guidance for retry |
| Documentation | Human-readable | Machine-optimized for LLM |
| Composition | ESB orchestration | Agent-driven tool chaining |
| Protocol | JMS, AMQP, HTTP | MCP, function calling |

---

# Generated from arcade.dev/patterns