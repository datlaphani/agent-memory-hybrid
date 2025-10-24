# Agent Memory Workflow Guide

## When to Save Memories
- After fixing bugs, refactors, or design decisions

## How to Save Memories
- Use the function `save_agent_memory()` with all fields: project_id, file_path, user, memory_type, content

## When to Retrieve Memories
- Always query `get_agent_memories()` before making changes

## Safety Protocols
- Do not overwrite; always append new records
- Flag uncertain cases for later review

## Example Usage
save_agent_memory("projectA", "src/payments.py", "agent", "bug", "Resolved payment error with new ENV config.")
