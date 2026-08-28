# StoreBackend Example

This example demonstrates the most important Deep Agents persistence distinction: the checkpointer retains conversation state for one `thread_id`, while the store retains a file across threads. The namespace must be stable and must come from trusted runtime context in a multi-tenant service.

```python
from deepagents import create_deep_agent
from deepagents.backends.store import StoreBackend
from deepagents.backends.utils import create_file_data
from langgraph.checkpoint.memory import MemorySaver
from langgraph.store.memory import InMemoryStore


def namespace_for_user(user_id: str) -> tuple[str, str]:
    if not user_id or "/" in user_id:
        raise ValueError("user_id must be a non-empty safe identifier")
    return ("memories", user_id)


store = InMemoryStore()
user_id = "demo-user"
store.put(
    namespace_for_user(user_id),
    "AGENTS.md",
    create_file_data("# Project memory\nUse concise answers.\n"),
)

agent = create_deep_agent(
    model=model,
    backend=StoreBackend(
        store=store,
        namespace=lambda runtime: namespace_for_user(user_id),
    ),
    memory=["/projects/AGENTS.md"],
    store=store,
    checkpointer=MemorySaver(),
)

result = agent.invoke(
    {"messages": [{"role": "user", "content": "Summarize your memory."}]},
    config={"configurable": {"thread_id": "store-demo-thread-1"}},
)
print(result["messages"][-1].content)
```

Production notes:

- Replace `InMemoryStore` and `MemorySaver` with durable implementations when restart recovery matters.
- Derive `user_id` from authenticated request context; do not accept it from model output or an untrusted prompt.
- Define retention, encryption, deletion, namespace migration, and concurrent-write behavior.
- Test that two users cannot read each other's files and that a second thread can read the same user's durable memory.
- Catch and classify store failures; do not claim memory was written unless the store confirms the write.
