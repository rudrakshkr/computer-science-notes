# System Design - Delivery Framework

> A structured approach to solving system design interviews efficiently.

---

# Why a Delivery Framework?

System design interviews are time-limited.

Without a structure, candidates often:
- Jump into architecture too early.
- Spend too much time on one component.
- Forget important requirements.
- Never finish the design.

A delivery framework helps you:
- Stay organized.
- Communicate clearly.
- Manage time.
- Deliver a complete system.

---

# Interview Timeline

| Phase | Time |
|--------|------|
| Requirements | ~5 min |
| Core Entities | ~5 min |
| API / High-Level Design | ~10-15 min |
| Deep Dives | ~15 min |
| Wrap Up | Remaining time |

---

# 1. Requirements (~5 min)

Start by understanding the problem before designing anything.

## Functional Requirements

Define what the system should do.

Ask questions like:

- What features are required?
- Who are the users?
- What actions can users perform?

Examples:

- Users can upload videos.
- Users can send messages.
- Users can like posts.

---

## Non-Functional Requirements

Understand system constraints.

Ask about:

- Scale
- Latency
- Availability
- Consistency
- Durability

Examples:

- 10M Daily Active Users?
- Read-heavy or write-heavy?
- Global system?
- Real-time updates?

---

## Out of Scope

Not every feature needs to be designed.

Explicitly state what you're ignoring.

Example:

> Notifications are out of scope for this interview.

This keeps the interview focused.

---

# 2. Core Entities (~5 min)

Identify the main data objects.

Examples:

Instagram

- User
- Post
- Comment
- Like

Uber

- Rider
- Driver
- Trip

Don't overcomplicate the schema.

Only include fields relevant to the discussion.

---

# 3. API Design & High-Level Design (~10–15 min)

Start with APIs before drawing architecture.

Example:

```http
POST /posts

GET /posts/{id}

POST /like
```

APIs help define how components interact.

---

## Draw High-Level Components

Typical components:

- Client
- Load Balancer
- Application Servers
- Cache
- Database
- Message Queue
- Object Storage
- CDN

Only introduce components when they solve a real problem.

Avoid adding technologies just because you know them.

---

## Explain Data Flow

Walk through one request step by step.

Example:

Client
↓

Load Balancer
↓

Application Server
↓

Cache

↓

Database

↓

Response

Always explain what happens at each step.

---

# 4. Deep Dives (~15 min)

This is where interviewers evaluate your depth.

Possible discussion topics:

- Database choice
- Caching strategy
- Scaling
- Replication
- Partitioning
- Consistency
- Rate limiting
- Message queues
- Fault tolerance

Don't deep dive into everything.

Choose the most important bottlenecks.

If the interviewer guides you elsewhere, follow them.

---

# 5. Wrap Up

Before the interview ends:

- Summarize your design.
- Mention trade-offs.
- Discuss future improvements if time permits.

Example:

> "This design prioritizes availability over strong consistency. If the scale grows significantly, I would introduce sharding and asynchronous processing."

---

# General Tips

- Clarify before designing.
- Think aloud.
- Build incrementally.
- Justify every design decision.
- Mention trade-offs.
- Don't optimize too early.
- Finish a working design before adding complexity.

---

# Common Interview Mistakes

- Starting architecture immediately.
- Ignoring requirements.
- Spending too much time on one topic.
- Adding unnecessary technologies.
- Never finishing the design.
- Not explaining trade-offs.

---

# 1-Minute Revision

- Clarify requirements first.
- Separate functional and non-functional requirements.
- Define core entities.
- Design APIs before architecture.
- Explain data flow.
- Deep dive into the important bottlenecks.
- Always discuss trade-offs.
- Deliver a complete design before optimizing.