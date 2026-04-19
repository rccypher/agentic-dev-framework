# Discovery Process

A structured conversation guide for surfacing what needs to be built, what the boundaries are, and what could go wrong — designed for users who may have no software development background.

**When to use:** At the start of any Standard or Complex task (step 2), and any time during development when scope, requirements, or priorities feel unclear.

**How it works:** The agent asks questions in plain language, using practical metaphors. The user doesn't need to know technical terms — the agent translates answers into technical requirements. The goal is to get the user to paint a vivid picture of what success looks like, what failure looks like, and where the edges are.

---

## Phase 1: The Big Picture

*Get the user to describe the destination before discussing the road.*

### What are we building?

> "Describe what you want to exist when we're done — as if you're explaining it to a friend. Don't worry about how it works. Just tell me what it does."

Follow-ups if the answer is vague:
- "If this were a tool sitting on your desk, what would you use it for on a typical day?"
- "Can you walk me through a specific moment where you'd use this? Start from 'I open it up and...'"
- "If someone else used this without you explaining it, what would they immediately understand it does?"

### Why does this matter?

> "What problem does this solve? What's the pain today without it?"

Follow-ups:
- "What are you doing right now to work around this problem?"
- "How often does this problem come up — daily, weekly, occasionally?"
- "If we never build this, what happens? What stays broken?"

*Why this matters to the process: If the user can't articulate the pain, the feature may not be needed. If the pain is infrequent, the complexity budget should be low.*

### Who is this for?

> "Who uses this besides you? Or is it just for you?"

Follow-ups:
- "Would anyone else ever touch this, even accidentally?"
- "Does anyone else need to understand the output, even if they don't use it directly?"

---

## Phase 2: What Does Success Look Like?

*Get concrete, observable outcomes — not abstract goals.*

### The snapshot test

> "Imagine we've built this and it works perfectly. You use it for the first time. Describe exactly what you see, step by step."

Follow-ups:
- "What does the output look like? Can you sketch it in words — like describing a screenshot?"
- "What information is shown? What's NOT shown?"
- "How long should this take to run? Instant? A few seconds? Minutes?"

### The 'done' definition

> "How will you know this is finished? What specific thing will you be able to do that you can't do right now?"

Follow-ups:
- "Is there a specific test we could run where, if it works, you'd say 'yes, this is exactly right'?"
- "What's the minimum version that would still be useful to you? Like, if we could only build half of it, which half matters most?"

*Why this matters: The answers become acceptance criteria. "I can see X and do Y" translates directly into testable requirements.*

---

## Phase 3: Where Are the Edges?

*Find the boundaries before building into a corner.*

### What this is NOT

> "Sometimes it's easier to say what something ISN'T. Is there anything you want to make sure we DON'T build? Anything that might seem related but should be left out?"

Follow-ups:
- "Are there features that sound nice but aren't important right now?"
- "Is there a simpler version of this that would be 'good enough' for now?"

### The neighbors

> "Does this need to work with anything else you already use? Or is it standalone?"

Follow-ups:
- "Does it need to read data from somewhere? Write data to somewhere?"
- "Think of it like a new appliance in your kitchen — does it need to plug into a specific outlet, or does it run on batteries?"

### The limits

> "Are there any hard rules? Things that absolutely must or must not happen?"

Follow-ups:
- "Is there private or sensitive information involved? Passwords, personal data, financial information?"
- "Are there any legal or policy rules we need to respect?"
- "Is there a budget limit — either money or time — we need to stay within?"

*Why this matters: These answers become scope boundaries and non-goals in the spec. Undiscovered limits cause the most expensive rework.*

---

## Phase 4: What Could Go Wrong?

*Surface risks before they become surprises. Use the pre-mortem technique in plain language.*

### The pre-mortem

> "Let's play a game. Imagine it's a month from now and this project has completely failed. It's broken, nobody uses it, and we wasted all that effort. What went wrong?"

Follow-ups:
- "What's the most likely reason it would fail?"
- "Is there anything about this that makes you nervous or uncertain?"
- "Has anything like this been tried before? What happened?"

### The edge cases

> "Think about the weird situations. What happens when things aren't normal?"

Use metaphors appropriate to the domain:
- "What if the internet goes down while this is running? Like a power outage in the middle of cooking."
- "What if someone gives it bad information? Like putting diesel in a gasoline car."
- "What if it gets used way more than expected? Like a restaurant that suddenly has a line out the door."

### The day-two problem

> "Okay, it works on day one. What about day 100? What changes over time that might break it?"

Follow-ups:
- "Does the data it uses change? Grow? Get updated by someone else?"
- "Will you need to change how it works later? How often?"
- "What happens when you're not available — does someone else need to maintain this?"

*Why this matters: Pre-mortem answers become test cases and documented assumptions. "What if X?" becomes a test that verifies X is handled.*

---

## Phase 5: Priorities and Tradeoffs

*Force rank when everything can't be first priority.*

### The three-legged stool

> "Every project balances three things: how much it does (scope), how quickly it's done (speed), and how polished it is (quality). You can usually optimize two of three. Which two matter most to you?"

Explain with a metaphor:
- "It's like moving to a new house. You can move fast, move everything, or pack carefully — but probably not all three. Which two would you pick?"

### The priority stack

> "If we run into a wall and can only deliver part of this, what's the most important part? And what could we drop?"

Follow-ups:
- "Imagine I can only give you three features from everything we discussed. Which three?"
- "Is there anything on your list that's actually a 'nice to have' disguised as a 'must have'?"

### The tradeoff test

> "Would you rather have something basic that works next week, or something polished that takes a month?"

*Why this matters: Priority answers drive complexity assessment and prevent scope creep. When the user has explicitly ranked priorities, the agent has permission to cut scope on lower-ranked items.*

---

## Phase 6: Confirmation

*Play it back and get explicit agreement.*

### The playback

> "Let me make sure I've got this right. Here's what I think we're building..."

Read back:
1. What it does (one sentence)
2. What success looks like (2-3 acceptance criteria)
3. What it does NOT do (scope boundaries)
4. What could go wrong (top 2-3 risks)
5. What matters most (priority ranking)

> "Did I get anything wrong? Is anything missing?"

### The commitment

> "Based on everything we've discussed, I'm going to create a written spec. That spec becomes our contract — if something isn't in it, it's not getting built. If something IS in it, it IS getting built. Are you comfortable with that?"

---

## Techniques for Non-Technical Users

### Use metaphors, not jargon

| Technical Concept | Plain Language Metaphor |
|------------------|----------------------|
| API | "A phone number your system can call to get information" |
| Database | "A filing cabinet that remembers everything" |
| Schema | "The labels on the filing cabinet drawers — what goes where" |
| Authentication | "The lock on the front door — who's allowed in" |
| Caching | "Keeping a photocopy on your desk so you don't walk to the filing cabinet every time" |
| Queue | "A line at the post office — requests wait their turn" |
| Latency | "How long you wait after pressing the button before something happens" |
| Rollback | "An undo button for the whole system" |
| Feature flag | "A light switch that lets us turn a feature on and off without rebuilding anything" |
| Dependency | "Something we rely on that we don't control — like the postal service" |
| Race condition | "Two people trying to edit the same document at the same time — who wins?" |
| Pipeline | "An assembly line where each station does one job and passes the result to the next" |

### Ask for stories, not specifications

Bad: "What are the input validation requirements?"
Good: "What kind of information will people type in? What would be a 'wrong' answer?"

Bad: "What's the error handling strategy?"
Good: "When something goes wrong, what should the user see? Should it just say 'oops' or explain what happened?"

Bad: "What are the performance SLAs?"
Good: "How long is too long to wait? Like, would you tap your foot after 2 seconds, or is 30 seconds fine?"

### Watch for disguised requirements

Users often say things that contain hidden requirements:

| User Says | Hidden Requirement |
|-----------|-------------------|
| "It should just work" | Reliability, error handling, graceful degradation |
| "Keep it simple" | Minimal UI, few options, sensible defaults |
| "Like [other product] but..." | Reference implementation exists; understand the delta |
| "I'll figure that out later" | Scope boundary — mark as out of scope, revisit later |
| "That would never happen" | It will happen — write a test for it |
| "It doesn't need to be perfect" | Define "good enough" explicitly or it will be redefined later |

### When the user doesn't know

Sometimes the honest answer is "I don't know." That's valuable information:

> "That's totally fine — let's mark that as an open question. We'll make our best guess, build it that way, and you can tell us if it's wrong when you see it. Sound good?"

Open questions go into the spec artifact. They are resolved during exploration or flagged for user decision during implementation.

---

## Quick Reference: Minimum Viable Discovery

For time-constrained situations, ask these five questions:

1. **"What does this do?"** → Becomes the problem statement
2. **"How will you know it works?"** → Becomes acceptance criteria
3. **"What should it NOT do?"** → Becomes scope boundaries / non-goals
4. **"What could go wrong?"** → Becomes pre-mortem / risk list
5. **"What matters most?"** → Becomes priority ranking
