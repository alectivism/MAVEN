---
name: content-shipped
description: Log completed deliverables and shipped content to the content log. Triggers proactively when content is finalized, published, or delivered. Use when "content shipped", "published", "sent the email", "delivered", "completed the document", or any content completion event.
disable-model-invocation: false
allowed-tools:
  - Read
  - Edit
  - Write
---

# Content Shipped Tracker

## Trigger Detection
Activate when the user:
- Completes a document, email, or deliverable
- Publishes content (blog, newsletter, social post)
- Sends a communication
- Finalizes a presentation or report
- Ships any work product

## Logging Process

### 1. Capture Details
- **What:** Title/description of the content
- **Type:** Email, document, blog post, social post, presentation, report, other
- **Audience:** Who received/will see it
- **Date:** When it was shipped
- **Link:** URL or file path if applicable

### 2. Log to Content Record
Append to `content/log.md` (create if it doesn't exist):

```markdown
### [Date] — [Content Title]
- **Type:** [type]
- **Audience:** [audience]
- **Link:** [link if applicable]
```

### 3. Track Against Goals
Check `state/goals.md` for content-related goals. If this shipment advances a goal, note the progress.

## Guidelines
- Log proactively when content completion is detected
- Keep entries concise — one entry per shipment
- Don't log drafts — only shipped/finalized content
- Ask for confirmation before logging if the completion isn't explicit
