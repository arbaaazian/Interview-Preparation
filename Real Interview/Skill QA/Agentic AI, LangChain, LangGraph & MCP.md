# LangChain, LangGraph and Agentic AI – 48 Interview Questions and Answers

*Arranged from fundamentals to advanced concepts, with simple explanations, practical examples and scenario-based answers.*

**Label format:** (V.Imp) = Very Important · (Imp) = Important · [Scenario Based] = scenario question

---

## 1. Agentic AI Fundamentals

### 1. What is the difference between AI and an agent? (V.Imp)

**Answer:**

AI is the broader field of building systems that perform tasks that normally require human intelligence, such as understanding language, recognizing images, making predictions, and generating content.

An AI agent is a system that uses an AI model to work toward a goal. It can observe the current situation, decide what action to take, use tools, and continue until it reaches a stopping condition.

**Example:**
- A normal AI chatbot answers: “What meetings do I have today?”
- An AI agent can check your calendar, identify free time, and create a meeting after receiving permission.

**Key difference:** AI describes the intelligence or capability. An agent describes a system that uses that capability to take goal-directed actions. Not every AI application is an agent.

### 2. What are agents in Agentic AI? (V.Imp)

**Answer:**

An agent is a software component that receives a goal, reasons about the next step, and takes actions to achieve that goal. Depending on its design, it may use an LLM, tools, memory, and a state-management mechanism.

A typical agent loop is:

1. Receive a task.
2. Understand the task and available context.
3. Decide whether to answer directly or call a tool.
4. Execute the selected action.
5. Inspect the result.
6. Continue, ask for clarification, or finish.

**Example:** A travel agent might search flights, compare options, check hotel availability, and prepare an itinerary. Booking or payment should require appropriate authorization.

### 3. What are tools in Agentic AI? (V.Imp)

**Answer:**

Tools are functions or external capabilities that an agent can invoke to perform tasks it cannot or should not perform using text generation alone.

Examples include:
- Search the web.
- Query a database.
- Read a file.
- Send an email.
- Call an API.
- Run a calculation.

A tool generally has a name, description, input schema, and implementation. The description and schema help the model understand when and how to use it.

**Example:** An employee-support agent may call `get_leave_balance(employee_id)` to retrieve a balance from an HR system. The actual function performs the database/API operation; the LLM does not magically access the database by itself.

### 4. What is the difference between an AI agent and a workflow, and when would you use a multi-agent system? [Scenario Based] (V.Imp)

**Answer:**

A workflow follows a predefined sequence or graph of steps. An agent can choose actions dynamically based on the task, context, and tool results.

| Workflow | Agent |
|---|---|
| Steps are largely predefined | Chooses among available actions |
| More predictable | More flexible |
| Easier to test and control | Needs stronger guardrails and evaluation |
| Good for repeatable processes | Good for tasks with variable paths |

**When to use a multi-agent system:**

Use multiple agents when a task has clearly separable responsibilities, specialized tools or prompts, and meaningful benefits from independent work. For example, a research agent gathers evidence, an analysis agent evaluates it, and a report agent prepares the result.

Do not add agents just to make the architecture look advanced. A single agent or deterministic workflow is often simpler, cheaper, and easier to maintain.

### 5. What are Agentic AI frameworks? (V.Imp)

**Answer:**

Agentic AI frameworks are software frameworks that help developers build applications in which AI models can reason over tasks, call tools, maintain state, coordinate steps, and sometimes collaborate with other agents.

Examples include:
- **LangChain:** Components for model integration, prompts, tools, retrieval, and agent applications.
- **LangGraph:** Graph-based orchestration for stateful workflows and agents, including cycles and persistence.
- **AutoGen:** A framework for building applications involving conversations among agents.
- **CrewAI:** A framework that organizes agents around roles, tasks, and crews.
- **Semantic Kernel:** SDK for integrating AI models with functions, plugins, and application services.

Framework capabilities and APIs evolve, so in an interview explain the design concepts as well as the version you have used.

### 6. What is Model-Context-Protocol (MCP)?

**Answer:**

MCP stands for **Model Context Protocol**. It is an open protocol that standardizes how AI applications connect to external tools and context sources.

An MCP architecture commonly has:
- **Host:** The AI application, such as an assistant or IDE.
- **Client:** The component inside the host that maintains a connection to an MCP server.
- **Server:** Exposes capabilities such as tools, resources, and prompts.

**Example:** An AI coding assistant can connect to an MCP server that exposes approved repository-search tools. The assistant discovers the tool schema and requests an operation through the protocol.

**Important:** MCP standardizes the connection and capability-exchange pattern. It does not itself make a model intelligent, guarantee that a tool is safe, or replace authorization. The host and server still need access controls and validation.

---

## 2. LangChain Fundamentals, Runnables and Memory

### 7. What are runnables and language runnables in LangChain? (V.Imp)

**Answer:**

A **Runnable** is a standard interface in LangChain for a component that takes an input and produces an output. Models, prompts, retrievers, and many processing components can be used as runnables.

Common runnable operations include:
- `invoke()` – run one input.
- `batch()` – run multiple inputs.
- `stream()` – stream output when supported.
- `ainvoke()` – asynchronous invocation.
- `pipe()` or the `|` operator – connect components into a sequence.

**Example:**

```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import ChatOpenAI

prompt = ChatPromptTemplate.from_template(
    "Explain {topic} in simple terms."
)
model = ChatOpenAI(model="gpt-4o-mini")

chain = prompt | model
result = chain.invoke({"topic": "RAG"})
print(result.content)
```

The prompt formats the input, then the model receives the formatted messages.

The phrase “language runnable” is not generally a separate core category parallel to Runnable. In LangChain discussions, it may refer informally to a runnable language-model component, such as a chat model. In an interview, clarify the specific API or class being discussed.

### 8. What is the difference between LangChain and LangGraph? (V.Imp)

**Answer:**

LangChain provides building blocks for LLM applications, such as chat models, prompts, retrievers, tools, and agent abstractions.

LangGraph is focused on orchestrating stateful applications as graphs. It is useful when execution needs explicit nodes, transitions, branching, loops, checkpoints, or human approval.

| LangChain | LangGraph |
|---|---|
| Component and application framework | Graph-based orchestration framework |
| Useful for chains, model calls, retrieval and tools | Useful for stateful, multi-step and cyclic workflows |
| Often used to compose components | Explicitly models nodes, edges and shared state |
| Can be used independently | Can use LangChain models and tools |

**Example:** A simple document Q&A chain can be built with LangChain. A support workflow that classifies a request, retrieves policy, asks for human approval, retries on failure, and then sends a response is a good fit for LangGraph.

### 9. What is LangGraph, and how have you used it? (V.Imp)

**Answer:**

LangGraph is a graph-based orchestration framework for building stateful LLM workflows and agents. A graph consists of:
- **State:** Shared data passed through the workflow.
- **Nodes:** Functions that perform work.
- **Edges:** Rules that determine which node runs next.
- **Reducers:** Rules for combining updates to state fields.
- **Checkpoints:** Persisted snapshots that support recovery and continuation.

**Example interview answer:**

“I would use LangGraph when my application needs multiple steps, conditional routing, shared state, retries, or human approval. For example, in an interview-assessment workflow, one node can extract the candidate response, another can evaluate it, and another can prepare feedback. Conditional edges can route low-confidence evaluations for review. A checkpointer can preserve execution state so an interrupted workflow can continue.”

Only claim a specific implementation if you actually built it; otherwise present it as a design you would implement.

### 10. What are the types of memory in LangChain with Python? (V.Imp)

**Answer:**

Older LangChain versions provided memory classes such as:
- `ConversationBufferMemory` – stores the full conversation history.
- `ConversationBufferWindowMemory` – keeps a recent window of messages.
- `ConversationSummaryMemory` – uses a summary to represent earlier conversation.
- `ConversationSummaryBufferMemory` – combines a recent message buffer with a summary.
- `VectorStoreRetrieverMemory` – retrieves relevant past information from a vector store.

**Important version note:** These are legacy memory abstractions and may be deprecated or unsuitable in newer LangChain patterns. Modern applications commonly manage message history explicitly, use message-history integrations, or use LangGraph state and persistence.

**How to choose:**
- Short conversations: keep recent messages.
- Long conversations: summarize older turns.
- Searchable long-term knowledge: retrieve relevant stored memories.
- Stateful agent workflows: use graph state and a checkpointer.

Memory should be scoped to the correct user/session, protected as sensitive data, and have retention/deletion rules.

### 11. How do you manage memory and conversation history in multi-turn chatbot interactions? [Scenario Based]

**Answer:**

I separate short-term conversation context from long-term memory.

1. Give each conversation a session or thread ID.
2. Store recent messages for continuity.
3. If the conversation becomes long, summarize older turns.
4. Retrieve only relevant long-term information when needed.
5. Keep user-specific data isolated by authorization checks.
6. Set retention limits and provide a way to correct or delete stored information.

**Example:** If a user first says, “I am building a chatbot with FastAPI,” and later asks, “How should I deploy it?”, the system can use the relevant earlier context instead of asking the user to repeat it.

Do not blindly send the entire history to the model. It increases token cost and may introduce irrelevant or stale context.

### 12. How do you set or manage memory between two AI agents? (V.Imp)

**Answer:**

I would not assume that two agents automatically share memory. I would define an explicit shared state or memory service.

For example:
- Agent A receives a task and produces a structured summary.
- The summary is written to shared state.
- Agent B reads the required fields from that state.
- Each agent can also have private state if needed.

In LangGraph, nodes can read from and return updates to a common graph state. A reducer controls how updates to particular fields are combined.

**Design principle:** Share only the information the next agent needs. Use structured fields, stable identifiers, and access controls rather than passing an uncontrolled transcript to every agent.

### 13. How do you manage conversation history to avoid context loss? [Scenario Based]

**Answer:**

I use a combination of persistent message history, summarization, and retrieval.

- Persist messages using a session/thread identifier.
- Keep the most recent turns available to the model.
- Summarize older turns while preserving decisions, constraints, and unresolved tasks.
- Retrieve relevant historical details when a new query needs them.
- Store important facts separately from raw conversation logs.
- Test that summaries preserve critical facts and do not invent details.

In LangGraph, a checkpointer can persist graph state between invocations. The thread ID identifies the conversation whose state should be loaded.

---

## 3. LangGraph Architecture and State Management

### 14. What is a state graph, and how does it work? (V.Imp)

**Answer:**

A state graph represents an application as nodes connected by edges, with a shared state object flowing through the graph.

- A node receives the current state.
- It performs a task.
- It returns a partial state update.
- An edge determines the next node.
- The process continues until an end condition is reached.

**Example:**

```text
START
  |
Classify Request
  |
  +---- question ----> Retrieve Context
  |                       |
  |                    Generate Answer
  |                       |
  +---- complaint ----> Human Review
                          |
                         END
```

The graph makes the workflow explicit and easier to inspect, test, and control.

### 15. What is the difference between a state graph, message graph, and a prebuilt create_react_agent? (V.Imp)

**Answer:**

These terms refer to different LangGraph APIs or patterns, and the exact API availability depends on the installed version.

- **StateGraph:** A general graph builder. You define the state schema, nodes, and edges. It is suitable for custom workflows.
- **Message-oriented graph/pattern:** Organizes state around messages and message updates. In LangGraph, message state is commonly implemented using `MessagesState` or an equivalent state schema with the `add_messages` reducer. “MessageGraph” appeared in older APIs and should not be assumed to be the current recommended API.
- **`create_react_agent`:** A prebuilt agent pattern that repeatedly lets a model decide whether to call a tool, executes the tool, and returns the result to the model until it finishes. The import path and recommended replacement can vary by LangGraph version.

**When to use:**
- Choose `StateGraph` for custom control and business logic.
- Choose message-oriented state when conversation messages are the main state.
- Choose a prebuilt ReAct agent when its model-tool loop fits the requirement and you do not need to build that loop yourself.

### 16. What is a cycle, and how does it differ from a DAG in LangChain? (V.Imp)

**Answer:**

A **DAG** (Directed Acyclic Graph) has directed edges and no cycles. Each path moves forward without returning to an earlier node.

A **cycle** is a path that returns to a previously visited node.

In LangGraph, cycles are useful for iterative behavior, such as:
- Agent → Tool → Agent.
- Draft → Review → Revise → Review.
- Generate answer → Evaluate → Retry.

A traditional one-pass chain is usually acyclic. LangGraph supports cycles, but the graph must have a suitable stopping condition or recursion limit to avoid running indefinitely.

### 17. What are reducers, and what happens when parallel branches write to the same field without one?

**Answer:**

A reducer defines how updates to a state field are combined.

Suppose two parallel nodes both update `results`. A reducer can append both results to a list rather than treating the updates as conflicting writes.

**Example:**

```python
import operator
from typing import Annotated
from typing_extensions import TypedDict

class State(TypedDict):
    results: Annotated[list, operator.add]
```

Here, `operator.add` combines list updates by concatenation.

If parallel branches write to the same state key without a suitable reducer, LangGraph may raise an update conflict or invalid concurrent update error rather than knowing which value to keep. The exact behavior depends on the state schema and runtime version.

**Remember:** Use reducers for fields that need merging. For fields where only one node should write, design the graph to avoid concurrent writes.

### 18. How are conversations passed between different states or agents in LangGraph? (V.Imp)

**Answer:**

Conversation data is passed through the graph's shared state. A common design uses a `messages` field with a message reducer such as `add_messages`.

Each node receives the current state and returns an update. The next node then sees the updated state.

**Conceptual example:**

```python
def agent_a(state):
    return {"messages": [("assistant", "I found the relevant policy.")]}

def agent_b(state):
    # Reads the messages already present in state
    return {"summary": "Policy retrieved and ready for review."}
```

For separate agents, you can pass the complete message history, a filtered subset, or a structured handoff object. Passing only relevant context reduces token use and limits accidental information sharing.

### 19. How does a state graph work and interact with messages or events?

**Answer:**

The graph's state is the data model for the workflow. Messages are one possible part of that state, while events may represent external inputs or runtime signals.

For example:
1. A user message enters the graph.
2. A node classifies it and updates state.
3. A conditional edge selects the next node.
4. A tool node may execute an API call.
5. The tool result is added to state.
6. The response node uses the updated state to answer.

The graph controls execution; state carries information. Messages are not the graph itself.

### 20. What is a checkpoint in programming and machine learning? (Imp)

**Answer:**

A checkpoint is a saved snapshot of a system's state at a particular point in execution or training.

In machine learning training, a checkpoint may contain model weights, optimizer state, epoch/step information, and other training metadata. It allows training to resume or a model to be restored.

In a workflow, a checkpoint may contain the current state, execution position, and related metadata so processing can continue after interruption.

### 21. What is a checkpoint in the context of LangChain or AI pipelines? (V.Imp)

**Answer:**

In LangGraph, a checkpoint is a persisted snapshot of graph state associated with a thread. A checkpointer saves state at graph execution steps.

It can support:
- Resuming an interrupted workflow.
- Inspecting previous states.
- Human-in-the-loop approval.
- Time-travel debugging or replay patterns, depending on the implementation.
- Maintaining conversation state across invocations.

A typical setup uses a checkpointer and a stable `thread_id` in the graph's configurable settings. The exact checkpointer implementation may be in-memory, SQLite, PostgreSQL, or another supported backend.

**Important distinction:** A checkpoint is not simply a copy of the LLM's hidden internal reasoning. It stores application/graph state that the workflow has explicitly represented.

### 22. How can checkpoints be implemented in LangChain workflows? (V.Imp)

**Answer:**

For a LangGraph workflow, compile the graph with a checkpointer and invoke it using a stable thread ID.

**Illustrative example (API details may vary by installed version):**

```python
from typing_extensions import TypedDict
from langgraph.graph import StateGraph, START, END
from langgraph.checkpoint.memory import MemorySaver

class State(TypedDict):
    question: str
    answer: str

def answer_node(state: State):
    return {"answer": f"Answer for: {state['question']}"}

builder = StateGraph(State)
builder.add_node("answer", answer_node)
builder.add_edge(START, "answer")
builder.add_edge("answer", END)

checkpointer = MemorySaver()
graph = builder.compile(checkpointer=checkpointer)

config = {"configurable": {"thread_id": "conversation-123"}}
result = graph.invoke({"question": "What is RAG?"}, config=config)
```

For production, use a durable supported checkpointer and appropriate database configuration rather than an in-memory saver, because in-memory state is lost when the process stops.

### 23. How can you resume execution in a LangChain workflow after a node failure? (V.Imp)

**Answer:**

I would design recovery at both the node and workflow levels.

1. Persist graph state using a checkpointer.
2. Make external operations idempotent where possible.
3. Add bounded retries for transient errors.
4. Record error details and the failed operation.
5. Resume from the saved thread state after fixing the issue.
6. Use human approval for operations that should not be retried automatically.

A checkpoint does not automatically make every failed external side effect safe to repeat. For example, blindly retrying an email or payment operation could send or charge twice. Use idempotency keys or operation-status checks.

### 24. How can fallback be implemented in LangChain or LangGraph workflows? (V.Imp)

**Answer:**

Fallback means switching to an alternative when the preferred component fails or is unavailable.

In LangChain, runnable fallbacks can be configured for supported runnable components. Conceptually:

```python
primary_model = ...
backup_model = ...

model_with_fallback = primary_model.with_fallbacks(
    [backup_model]
)
```

In LangGraph, fallback can also be modeled explicitly:
- Call the primary model/API.
- Catch or classify the failure.
- Route to a backup model or alternate node.
- Record which path was used.

**Example:** If the primary LLM provider times out, retry with a bounded policy and then route to a backup provider. Preserve compatible prompts, output schemas, and evaluation checks.

### 25. How do agents handle communication failures in LangChain? (V.Imp)

**Answer:**

I handle failures at the communication boundary rather than assuming every agent or tool call succeeds.

- Apply connection and read timeouts.
- Retry transient failures with exponential backoff and jitter.
- Validate request and response schemas.
- Use correlation IDs and structured logs.
- Set retry limits and circuit breakers where appropriate.
- Return a clear failure state when recovery is not possible.
- Use idempotency for operations with side effects.
- Route critical or ambiguous cases to a human.

For multi-agent communication, I also define what happens when an agent returns an empty, malformed, or incomplete result.

### 26. How do you manage state sharing between agents in a multi-agent workflow? [Scenario Based] (V.Imp)

**Answer:**

I define a shared state schema with explicit fields and ownership rules.

For example:

```python
class WorkflowState(TypedDict):
    user_request: str
    research_result: dict
    analysis_result: dict
    final_response: str
    errors: list
```

The research agent updates `research_result`; the analysis agent reads it and updates `analysis_result`; the response agent prepares `final_response`.

I would:
1. Share structured outputs rather than unfiltered internal transcripts.
2. Define which agent can update each field.
3. Use reducers for parallel updates.
4. Validate data at each handoff.
5. Keep private or sensitive data restricted.
6. Persist state when workflow recovery is required.

### 27. What is the difference between a supervisor swarm and a hierarchical multi-agent system in LangGraph? (V.Imp)

**Answer:**

The terminology can vary across libraries and examples, but the broad architectural distinction is:

- **Supervisor pattern:** A central supervisor receives the task, delegates to specialist agents, reviews their results, and decides what should happen next.
- **Swarm pattern:** Agents can hand off control to one another more dynamically, often based on which specialist is best suited to continue.
- **Hierarchical multi-agent system:** A top-level agent or manager delegates to sub-managers or groups, which may coordinate their own specialist agents.

**Example:**
- Supervisor: one manager assigns research and writing tasks and combines the results.
- Swarm: a support agent hands off to billing, which may hand off to refunds.
- Hierarchical: a project manager delegates to research and engineering leads, who coordinate their own teams.

Choose based on control requirements, task complexity, observability, and how predictable the handoffs need to be. Do not treat these names as universally fixed LangGraph node types.

### 28. How do you manage multi-agent workflows and interactions in LangGraph? [Scenario Based] (V.Imp)

**Answer:**

I start by identifying responsibilities and defining the state shared by the agents.

A possible workflow is:

```text
START
  |
Supervisor
  |
  +---- Research Agent ----+
  |                        |
  +---- Data Agent --------+--> Supervisor Review
                                   |
                           +-------+-------+
                           |               |
                         Revise          Finalize
                           |               |
                       Specialist        END
```

I would define:
- Each agent's role and allowed tools.
- Input/output schemas for handoffs.
- Shared state and reducers.
- Conditional routing rules.
- Retry and timeout policies.
- Checkpointing and human approval.
- Tracing, logs, and evaluation metrics.

I would begin with the smallest number of agents that clearly separates responsibilities.

---

## 4. Multi-Agent Design and Practical Scenarios

### 29. Are the same agents responsible for both email sending and scoring, or are they separate? (V.Imp)

**Answer:**

They should generally be separate responsibilities, even if they are coordinated in one workflow.

For example:
- **Scoring/evaluation component:** Reviews the interview response against a rubric and produces a structured score with evidence.
- **Email component:** Prepares and sends the result or notification using an approved template.

The scoring component should not send emails directly. The email component should not invent or change the score. A workflow or supervisor can pass the validated result from one component to the other.

Before sending, the system should check recipient, content, permissions, and whether human approval is required.

### 30. What language and frameworks are your agents built with? (V.Imp)

**Answer:**

A suitable interview answer for a Python-based system is:

“I build the agent backend in Python. I use LangChain for model, prompt, and tool integration, and LangGraph when I need stateful orchestration, conditional routing, or multi-agent coordination. FastAPI can expose the workflow through REST APIs. I use Pydantic for request and response validation, and a database such as MySQL or MongoDB for application data. The exact stack depends on the system requirements.”

Only list frameworks you have actually used in your project. If your agents are implemented using a different language or framework, state that accurately.

### 31. How do you pass information between different agents in your system? (V.Imp)

**Answer:**

I pass information through explicit handoffs. In a LangGraph application, agents can read and update shared graph state. In a service-based architecture, they may exchange validated messages through APIs or a message broker.

I prefer structured data such as:

```json
{
  "task_id": "task-101",
  "status": "completed",
  "summary": "Three relevant documents were found.",
  "evidence_ids": ["doc-12", "doc-18"]
}
```

This makes handoffs easier to validate, log, retry, and test than passing vague natural-language instructions alone.

### 32. Can you name the agents in LangChain? (V.Imp)

**Answer:**

LangChain does not define one fixed universal list of agent names. Developers create agents for their application and give them roles.

Common application-level examples are:
- Research agent.
- Retrieval/RAG agent.
- SQL/database agent.
- Coding agent.
- Data-analysis agent.
- Planning agent.
- Reviewer/critic agent.
- Email or notification agent.
- Supervisor/coordinator agent.

LangChain provides agent-building abstractions and integrations; the names and responsibilities are usually designed by the application developer.

### 33. How do agents communicate with each other in LangChain? (V.Imp)

**Answer:**

There is no single automatic communication channel shared by all LangChain agents. Communication is designed by the developer.

Common approaches include:
1. **Sequential handoff:** One chain or agent's output becomes the next agent's input.
2. **Shared state:** Agents read and update a common state, often orchestrated with LangGraph.
3. **Tool/API calls:** One agent invokes a tool or service that provides another capability.
4. **Message broker:** Independent services exchange messages through systems such as Kafka or RabbitMQ.

For reliable communication, use structured schemas, task IDs, timeouts, validation, and clear ownership of state.

### 34. Can you write a line graph code with two agents: a summarizer and a reviewer? (V.Imp)

**Answer:**

The following is a simple sequential LangGraph example: the summarizer creates a summary, and the reviewer checks it.

```python
from typing_extensions import TypedDict
from langgraph.graph import StateGraph, START, END

class State(TypedDict):
    text: str
    summary: str
    review: str

def summarizer(state: State):
    # Replace this placeholder with an LLM call.
    summary = state["text"][:200]
    return {"summary": summary}

def reviewer(state: State):
    # Replace this placeholder with an LLM review.
    review = (
        "Summary created. Check that it preserves the key facts: "
        + state["summary"]
    )
    return {"review": review}

builder = StateGraph(State)
builder.add_node("summarizer", summarizer)
builder.add_node("reviewer", reviewer)

builder.add_edge(START, "summarizer")
builder.add_edge("summarizer", "reviewer")
builder.add_edge("reviewer", END)

graph = builder.compile()

result = graph.invoke({
    "text": "LangGraph is used to build stateful workflows..."
})
print(result["summary"])
print(result["review"])
```

This is a simple two-node workflow. To make the reviewer genuinely evaluate quality, replace the placeholder with an LLM call and define a structured review schema.

### 35. How does an agent determine which function or tool to call? (V.Imp)

**Answer:**

The model receives the available tool names, descriptions, and input schemas. Based on the user's request and the current context, it decides whether a tool is needed and, if so, selects one and generates its arguments.

The application then validates the tool call and executes the actual function. The tool result is returned to the model, which can use it to continue or produce the final answer.

**Example:** If the user asks for their leave balance, the model may select `get_leave_balance`. If the user asks for a general explanation of annual leave, it may answer without calling a tool.

Tool selection is not a security boundary. The application must independently enforce permissions and validate arguments.

### 36. What agentic pattern and workflow did you use? (V.Imp)

**Answer:**

A good answer should describe the actual architecture rather than only naming a pattern.

For example:

“I use a supervisor-style workflow when a task needs multiple specialized capabilities. The supervisor classifies the request and routes it to a retrieval, evaluation, or notification component. Each component returns a structured result, and the supervisor decides whether another step is needed or whether the workflow can finish.”

A typical flow is:

```text
User Request
     |
Supervisor / Router
     |
     +---- Retrieval Agent
     |
     +---- Evaluation Agent
     |
     +---- Action Agent
               |
          Final Response
```

Mention the actual pattern you implemented. If you used a simple sequential workflow rather than autonomous agents, describe it as a workflow.

### 37. How would you implement reinforcement learning in an AI workflow based on user feedback? [Scenario Based]

**Answer:**

First, I would distinguish collecting feedback from actually training a reinforcement-learning policy. A thumbs-up/down signal alone does not automatically create reinforcement learning.

A practical process is:

1. Collect explicit user feedback and relevant task outcomes.
2. Link feedback to the prompt/model version and workflow trace.
3. Remove sensitive data and validate feedback quality.
4. Build a reliable reward signal or preference dataset.
5. Evaluate whether supervised fine-tuning, preference optimization, or reinforcement learning is appropriate.
6. Train or optimize offline first.
7. Compare the new model against a baseline using a held-out evaluation set.
8. Deploy gradually with monitoring and rollback.

For an agent, feedback can also improve deterministic routing rules, tool descriptions, or retrieval quality without changing model weights. Use human review for high-impact decisions.

### 38. How would you structure objectives, safety bounds, and evidence gathering for a procurement negotiation agent communicating by email? [Scenario Based] (V.Imp)

**Answer:**

I would treat this as a constrained workflow, not give the agent unrestricted authority to negotiate and commit.

**1. Objectives**
- Stay within the approved budget.
- Compare supplier price, quality, delivery, warranty, and terms.
- Gather enough evidence to recommend an option.
- Achieve the target terms defined by the procurement team.

**2. Safety bounds**
- Define maximum spend and approved negotiation ranges.
- Do not accept legal terms or sign contracts autonomously.
- Do not disclose confidential supplier or company information.
- Require human approval before sending binding offers, accepting terms, or placing orders.
- Restrict the agent to approved suppliers and communication channels.

**3. Evidence gathering**
- Retrieve approved procurement policies and purchase requirements.
- Collect supplier quotes and email history.
- Verify dates, quantities, currency, taxes, delivery, and warranty.
- Track the source of each claim.

**4. Workflow**
```text
Request
  |
Retrieve Policy and Supplier Evidence
  |
Compare Offers
  |
Draft Negotiation Email
  |
Human Approval
  |
Send Email
  |
Record Supplier Reply
  |
Re-evaluate and Escalate if Needed
```

Log every action and preserve an audit trail.

### 39. What agents were used in the Talent Talks platform, and how did they work together? (V.Imp)

**Answer:**

The exact agent names and architecture of Talent Talks are not specified in the question, so they should not be invented. A safe interview answer should use the components that actually exist in your implementation.

If describing an interview platform with AI-assisted interviewing, a possible responsibility breakdown is:
- **Interview orchestration:** Controls the interview stages and question flow.
- **Question-generation component:** Selects or generates the next question.
- **Response evaluation component:** Evaluates candidate answers against a rubric.
- **Proctoring component:** Processes signals or events from the monitoring system.
- **Report-generation component:** Prepares the final assessment.
- **Notification component:** Sends approved updates or reports.

Explain whether these were separate autonomous agents, ordinary backend services, or steps in one workflow. Do not call every service an agent unless it makes decisions or uses agent-like behavior.

### 40. How would you implement via casing in a multi-agent workflow? [Scenario Based] (V.Imp)

**Answer:**

The phrase “via casing” is unclear and may be a typo. It could mean **caching** or **case handling**. These are different topics.

If the intended question is **caching in a multi-agent workflow**, a suitable approach is:

1. Identify repeatable, read-heavy operations such as document retrieval or reference-data lookup.
2. Cache results using a key based on the normalized input, user/tenant scope, model or prompt version, and relevant filters.
3. Set a suitable time-to-live (TTL).
4. Invalidate the cache when source data or permissions change.
5. Avoid caching sensitive results across users.
6. Measure hit rate, latency, and stale-result incidents.

If “via casing” means something else, retain the exact wording from the interviewer and clarify it before answering.

### 41. Why choose LangChain with a multi-agent setup over Blanket? (V.Imp)

**Answer:**

“Blanket” is not clear in this question and may be a product or a typo. Do not claim a comparison without identifying the intended alternative.

If the intended comparison is **LangChain versus building a multi-agent system from scratch**, you can say:

“LangChain provides reusable integrations for models, prompts, tools, and retrieval. This reduces boilerplate and makes it easier to connect common components. For stateful orchestration and explicit handoffs, I can use LangGraph. However, I would still compare the framework against project requirements for control, observability, latency, deployment, and maintenance. A custom implementation may be better for a small or highly specialized workflow.”

### 42. How do you deploy an agent in a multi-agent AI system? [Scenario Based] (V.Imp)

**Answer:**

I would deploy the agent as part of a secure, observable application.

1. Package the agent/workflow and dependencies.
2. Expose it through an API, for example FastAPI.
3. Store secrets in a secure secret manager or environment configuration.
4. Configure model providers, tools, and databases.
5. Add authentication and authorization for tools.
6. Containerize the service if appropriate.
7. Deploy with health checks, resource limits, and scaling.
8. Add tracing, logs, metrics, and error reporting.
9. Test timeouts, retries, and failure paths.
10. Use versioned prompts and workflows with rollback support.

For multi-agent systems, agents can be separate services or nodes inside one orchestrated service. Separate deployment is useful when independent scaling or isolation is needed; it also adds network and operational complexity.

### 43. Can you explain the graph or decision tree used in your agent architecture? (V.Imp)

**Answer:**

I would explain the graph by walking through the entry point, decisions, specialist nodes, and stopping conditions.

Example:

```text
START
  |
Classify User Request
  |
  +---- Knowledge Question ----> Retrieval
  |                                  |
  |                              Generate Answer
  |                                  |
  +---- Evaluation Request ----> Evaluation
  |                                  |
  |                              Validate Result
  |                                  |
  +---- Action Request --------> Permission Check
                                     |
                              Human Approval?
                                /       \
                              Yes        No
                               |          |
                            Execute    Reject/Explain
                                \       /
                                  END
```

I would also explain what state each node reads and writes, what happens on failure, and how the workflow avoids unauthorized actions.

### 44. How do you handle conversation history to avoid context loss? [Scenario Based]

**Answer:**

I use persistent session history, recent-message windows, summaries, and retrieval of relevant past facts.

For each session:
- Use a stable conversation/thread ID.
- Persist messages and workflow state.
- Keep recent turns in the active prompt.
- Summarize older turns when necessary.
- Retrieve only relevant facts from long-term memory.
- Validate that summaries preserve decisions and constraints.
- Apply user-level access controls and retention rules.

In LangGraph, a checkpointer can persist graph state for a thread. It helps restore the workflow state, but it does not replace a deliberate memory and context-selection strategy.

### 45. How would you design an employee management system using FastAPI, Pydantic, LangGraph, MySQL, and RBAC? [Scenario Based] (V.Imp)

**Answer:**

I would keep core employee operations deterministic and use LangGraph only for tasks that benefit from AI orchestration.

**Architecture:**

```text
Client / Frontend
       |
    FastAPI
       |
Authentication (JWT)
       |
RBAC Authorization
       |
  Service Layer
   /        \
MySQL     LangGraph
             |
       Approved Tools
```

**Main components:**
1. **FastAPI:** Exposes endpoints for employee, department, leave, and role operations.
2. **Pydantic:** Validates request and response schemas.
3. **MySQL:** Stores employees, departments, roles, permissions, and audit records.
4. **JWT authentication:** Identifies the logged-in user.
5. **RBAC:** Checks whether the user's role permits an action on a resource.
6. **LangGraph:** Handles optional AI workflows, such as answering policy questions or assisting with an employee request.
7. **Tools:** Expose only approved operations to the graph, with authorization enforced server-side.

**Example flow for viewing an employee:**
- User sends `GET /employees/123`.
- FastAPI validates the request and JWT.
- RBAC checks whether the user can view that employee.
- Service layer retrieves the record from MySQL.
- Pydantic validates the response.
- API returns the result.

**Important:** Do not let an LLM bypass RBAC or directly execute unrestricted SQL. Enforce permissions in the backend for every sensitive operation.

### 46. How do you handle failure in an agent within a LangChain multi-agent setup? [Scenario Based] (V.Imp)

**Answer:**

I use layered failure handling.

1. **Detect:** Catch exceptions, timeouts, invalid outputs, and tool errors.
2. **Classify:** Decide whether the error is transient, permanent, or caused by invalid input.
3. **Retry:** Retry transient failures with a limited exponential-backoff policy.
4. **Fallback:** Route to another model, tool, or specialist if appropriate.
5. **Validate:** Check the fallback result against the expected schema and quality rules.
6. **Escalate:** Ask for human review when the result is uncertain or the action is high-impact.
7. **Persist:** Save state and error details so the workflow can be inspected or resumed.
8. **Observe:** Record traces, latency, retry counts, and failure rates.

For side-effecting operations such as sending emails or creating payments, use idempotency keys and verify operation status before retrying.

---

## 5. Coding and Evaluation

### 47. Coding question: Write a function to return total calls per agent from a list of dictionaries. (V.Imp)

**Answer:**

Suppose the input contains one dictionary per call, with an `agent` field. We can use `collections.Counter`.

```python
from collections import Counter

def total_calls_per_agent(records):
    return dict(Counter(
        record["agent"]
        for record in records
        if "agent" in record
    ))

data = [
    {"agent": "researcher"},
    {"agent": "reviewer"},
    {"agent": "researcher"},
    {"agent": "summarizer"},
    {"agent": "reviewer"},
]

print(total_calls_per_agent(data))
```

**Output:**

```python
{
    "researcher": 2,
    "reviewer": 2,
    "summarizer": 1
}
```

**Complexity:** O(n) time, where n is the number of records, and O(k) extra space for k distinct agents.

If the actual input uses a different key, such as `agent_name`, update the field accordingly.

### 48. What evaluation metrics do you follow for AI or agent-based systems? (V.Imp)

**Answer:**

I evaluate an agent at several levels.

**1. Task quality**
- Task success rate.
- Answer correctness.
- Completeness and relevance.
- Human preference or review score.

**2. Tool and workflow quality**
- Tool-selection accuracy.
- Tool-call success rate.
- Invalid-argument rate.
- Workflow completion rate.
- Retry and fallback rate.
- Handoff accuracy between agents.

**3. Safety and reliability**
- Policy-violation rate.
- Unauthorized-action rate.
- Hallucination or unsupported-claim rate.
- Failure and timeout rate.

**4. Performance and cost**
- End-to-end latency, including p50 and p95.
- Token usage.
- Cost per completed task.
- Throughput.

**5. RAG-specific metrics, when retrieval is involved**
- Recall@K and Precision@K.
- MRR and NDCG.
- Faithfulness/groundedness.
- Citation accuracy.

I use a fixed evaluation dataset, automated tests, trace inspection, and human review. I compare changes against a baseline before deploying them.

---

## Quick Interview Preparation Tips

1. Explain the difference between an LLM, an agent, a tool, and a workflow.
2. Be comfortable explaining LangChain Runnable composition and tool calling.
3. For LangGraph, understand state, nodes, edges, conditional routing, reducers, cycles, and checkpoints.
4. Explain memory separately from checkpointing: memory is about useful context; checkpointing is about persisted workflow state.
5. In multi-agent designs, describe responsibilities, handoffs, shared state, and failure handling.
6. Do not call every backend service an agent. Explain whether it makes decisions or simply performs a deterministic operation.
7. For scenario questions, explain the requirement, architecture, state flow, failure handling, security, and evaluation.
8. Verify API names against the LangChain/LangGraph version used in the project because these libraries evolve.
9. Be honest about what you implemented versus what you would design.
