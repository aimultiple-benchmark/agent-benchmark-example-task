# Task 6: Helpdesk Ticket System (Web)

## What to Build

A customer support ticket system with two user types:

**Customer:** Submit support tickets, view own tickets, add replies to own tickets

**Agent:** View all tickets, assign tickets to self, reply to tickets, change ticket status

---

## Core Requirements

### Ticket Flow
1. Customer creates a ticket with subject and description
2. Ticket starts in "open" status
3. Agent views all open tickets and assigns one to themselves
4. Agent and customer exchange replies on the ticket
5. Agent updates ticket status through the workflow

### Critical: Status Workflow
```
open -> in_progress (when agent assigns/starts working)
in_progress -> waiting_on_customer (agent needs info)
waiting_on_customer -> in_progress (customer replies)
in_progress -> resolved (agent resolves)
resolved -> reopened (customer reopens)
reopened -> in_progress
```
Invalid transitions must return 400. Only agents can set status to `resolved`. Only customers can set status to `reopened`.

### Critical: Data Isolation
- Customers can only see their own tickets
- Agents can see all tickets
- A customer requesting another customer's ticket must get 404 (not 403, to avoid leaking existence)

---

## Data Entities

- **User**: email, password, name, role (customer/agent)
- **Ticket**: customer_id, agent_id (nullable), subject, description, status, priority (low/medium/high), created_at, updated_at
- **Reply**: ticket_id, user_id, content, created_at

---

## Technical Constraints

For backend, use FastAPI + SQLite
For frontend, use React, vue, or Svelte+ Vite
For auth, use JWT token-based

### Run Commands (must work exactly)
```bash
# Backend
cd backend && pip install -r requirements.txt && uvicorn main:app --reload --port 8000

# Frontend
cd frontend && npm install && npm run dev
```

---

## Acceptance Criteria

- Backend starts without errors
- Frontend starts and connects to backend
- Customer can create a ticket
- Customer sees only their own tickets
- Agent sees all tickets
- Agent can assign a ticket to themselves
- Both can add replies (shown in chronological order)
- Status transitions follow the workflow (invalid → 400)
- Customer cannot change status to "resolved" → 403
- Agent cannot change status to "reopened" → 403
- Customer requesting another's ticket gets 404
- Ticket list supports filtering by status
- Priority can be set on creation and updated by agent
- Ticket detail shows full reply thread
- API docs available at /docs

---

## Seed Data

Create on first run:
- Agent: `agent@example.com` / `agent123`
- Customer: `customer@example.com` / `customer123`
- 3-4 sample tickets in various statuses with replies

---

## Deliverables

```
/backend
  main.py
  requirements.txt
  (organize remaining files as you see fit)

/frontend
  package.json
  (organize remaining files as you see fit)

README.md (setup instructions)
```
