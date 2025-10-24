# CloudSQL Agent Memory Workflow Guide

## When to Store a Memory
- After fixing a bug, log the bug type, location, root cause, and solution
- After a design decision, capture the reason, affected modules, and risks
- On identifying recurring errors, store error details and steps taken

## Storing a Memory
- Use the function `save_agent_memory()` with:
    - project_id
    - file_path
    - user
    - memory_type (“bug”, “design_decision”, etc.)
    - content (concise, plain English)

## Retrieving a Memory
- Before code changes, review recent memories for that file/module
- Before fixing a bug, check for similar past issues in the same area
- Use `get_agent_memories(project_id, file_path, memory_type)` to fetch context

## Safety Protocols
- Never overwrite existing memories; add new records with updated info
- Flag uncertainties in content so humans can review later

## Example:
