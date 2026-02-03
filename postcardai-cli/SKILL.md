---
name: postcardai-cli
description: Send AI-generated postcards via the Postcard AI CLI. Use when users want to send mail, manage contacts, create designs, or work with the Postcard AI platform.
license: MIT
metadata:
  author: Postcard AI
  version: "1.0"
  type: utility
  mode: assistive
---

# Postcard AI CLI: AI-Powered Direct Mail

You help users send AI-generated postcards and manage direct mail campaigns through the Postcard AI CLI. Your role is to translate user intent into CLI commands, handle authentication, and guide users through the mailing workflow.

## Core Principle

**The CLI enables programmatic postcard sending with AI-generated designs. Every action should verify authentication first, then execute the appropriate command.**

## Installation

```bash
npm install -g @postcardaidev/cli
```

## Quick Reference

### Authentication (Required First)
```bash
postcardai login              # Authenticate via browser
postcardai whoami             # Check current auth status
postcardai whoami --verify    # Verify credentials with API
postcardai logout             # Remove stored credentials
```

### Send a Postcard (Fastest Path)
```bash
postcardai send \
  --to "John Doe, 123 Main St, Austin, TX 78701" \
  --prompt "A beautiful sunset over mountains with 'Wish you were here!' text"
```

### Check Balance & Account
```bash
postcardai balance            # Show credit balance
postcardai account            # Show account details and usage
postcardai transactions       # Show transaction history
```

## Complete Command Reference

### Authentication Commands

| Command | Description |
|---------|-------------|
| `postcardai login` | Authenticate via device code flow (opens browser) |
| `postcardai login --no-browser` | Authenticate without opening browser |
| `postcardai logout` | Remove stored credentials |
| `postcardai whoami` | Show current authentication status |
| `postcardai whoami --verify` | Verify credentials are still valid |

### Quick Send

The `send` command is the fastest way to send a single postcard:

```bash
postcardai send \
  --to "Name, Street, City, STATE ZIP" \
  --prompt "Design prompt for AI generation" \
  [--size 4x6|5x7|6x9|6x11] \
  [--name "Campaign name"] \
  [--preview]  # Preview before sending
```

**Address format:** `"John Doe, 123 Main St, Austin, TX 78701"`

### Contact Management

```bash
# List contacts
postcardai contacts list [-l limit] [-o offset] [-s search] [--status active|inactive|bounced|do_not_mail]

# Get contact details
postcardai contacts get <id>

# Create a contact
postcardai contacts create \
  --street "123 Main St" \
  --city "Austin" \
  --state "TX" \
  --zip "78701" \
  [--first-name "John"] \
  [--last-name "Doe"] \
  [--unit "Apt 4"]

# Delete a contact
postcardai contacts delete <id>

# View contact history
postcardai contacts history <id>

# Match a person by email, phone, or property address
postcardai contacts match [--email email] [--phone phone] [--street street] [--city city] [--state state] [--zip zip]

# Batch import contacts from JSON file
postcardai contacts batch <file.json> [--no-skip-invalid]

# Batch match contacts from JSON file
postcardai contacts match-batch <file.json> [--create-missing]
```

### List Management

```bash
# List all lists
postcardai lists list [-l limit] [-o offset]

# Get list details
postcardai lists get <id>

# Create a list
postcardai lists create --name "My List" [--description "Description"]

# Delete a list
postcardai lists delete <id>

# Add contacts to a list
postcardai lists add-contacts <listId> <contactId1> <contactId2> ...
```

### Design Management

```bash
# List designs
postcardai designs list [-l limit] [-o offset] [-s search] [--status draft|generating|active|completed|archived] [--published]

# Get design details
postcardai designs get <id>

# Create a design
postcardai designs create \
  --prompt "Design prompt" \
  [--name "Design name"] \
  [--size 4x6|5x7|6x9|6x11] \
  [--orientation landscape|portrait]

# Iterate on a design (generate new version)
postcardai designs iterate <id> [--prompt "Refinement prompt"]

# Publish a design iteration
postcardai designs publish <id> --iteration <number>
```

### Mailing Management

```bash
# List mailings
postcardai mailings list [-l limit] [-o offset] [--status previewing|processing|active|paused|completed|cancelled]

# Get mailing details
postcardai mailings get <id>

# Send a mailing (after preview)
postcardai mailings send <id>

# Cancel a mailing
postcardai mailings cancel <id>

# Get preview images
postcardai mailings previews <id>

# List recipients and delivery status
postcardai mailings recipients <id> [-l limit] [-o offset] [--status pending|sent|in_transit|delivered|returned]

# Get mailing analytics
postcardai mailings analytics <id>
```

### Brand Management

```bash
# List brands
postcardai brands list

# Get brand details
postcardai brands get <id>

# Create a brand
postcardai brands create \
  --name "Brand Name" \
  --goal-type seller_leads \
  --goal-label "Get Seller Leads" \
  [--description "Brand description"] \
  [--voice "Brand voice"]

# Update a brand
postcardai brands update <id> \
  [--name "New Name"] \
  [--goal-type type] \
  [--goal-label label] \
  [--description text] \
  [--voice text] \
  [--brand-rules text] \
  [--goal-rules text] \
  [--logo-url url] \
  [--logo-description text] \
  [--logo-rules text] \
  [--always-include-logo | --no-always-include-logo] \
  [--headshot-url url] \
  [--headshot-description text] \
  [--always-include-headshot | --no-always-include-headshot]

# Delete a brand
postcardai brands delete <id>

# Set default brand
postcardai brands set-default <id>
```

### Return Address Management

```bash
# List return addresses
postcardai addresses list

# Get return address details
postcardai addresses get <id>

# Create a return address
postcardai addresses create \
  --name "Company Name" \
  --address1 "123 Main St" \
  --city "Austin" \
  --state "TX" \
  --zip "78701" \
  [--address2 "Suite 100"] \
  [--label "Main Office"] \
  [--default]

# Update a return address
postcardai addresses update <id> [--name name] [--address1 addr] [--address2 addr] [--city city] [--state state] [--zip zip] [--label label]

# Delete a return address
postcardai addresses delete <id>

# Set default return address
postcardai addresses set-default <id>
```

### Webhook Management

```bash
# List webhooks
postcardai webhooks list

# Get webhook details (includes secret)
postcardai webhooks get <id>

# Create a webhook
postcardai webhooks create --url "https://example.com/webhook" --events mailing.completed mailing.delivered

# Delete a webhook
postcardai webhooks delete <id>

# Rotate webhook secret
postcardai webhooks rotate-secret <id>

# Test a webhook
postcardai webhooks test <id>

# Enable/disable a webhook
postcardai webhooks enable <id>
postcardai webhooks disable <id>
```

### Credits & Billing

```bash
# Check credit balance
postcardai balance

# View transaction history
postcardai transactions [-l limit] [-o offset] [--type purchase|usage|refund|adjustment]

# Estimate mailing cost
postcardai estimate -r <recipients> [--size 4x6|6x9|6x11] [--steps <number>]
```

### Configuration

```bash
# Show all config
postcardai config get

# Get specific config value
postcardai config get <key>

# Set config value (editable keys: environment, apiEnvironment)
postcardai config set <key> <value>

# Show config file path
postcardai config path
```

**Configurable values:**
- `environment`: `live` or `test` (which Postcard AI environment to use)
- `apiEnvironment`: `local`, `staging`, or `production` (which API endpoint to target)

## Workflow Patterns

### Pattern 1: Quick One-Off Send

```bash
# 1. Login (one-time)
postcardai login

# 2. Send directly
postcardai send \
  --to "Jane Smith, 456 Oak Ave, Portland, OR 97201" \
  --prompt "Thank you card with flowers and elegant typography"
```

### Pattern 2: Preview Before Sending

```bash
# 1. Create with preview mode
postcardai send \
  --to "Jane Smith, 456 Oak Ave, Portland, OR 97201" \
  --prompt "Professional real estate just sold card" \
  --preview

# 2. Check previews
postcardai mailings previews <mailing-id>

# 3. Send when ready
postcardai mailings send <mailing-id>
```

### Pattern 3: Batch Campaign

```bash
# 1. Create contacts from JSON
postcardai contacts batch contacts.json

# 2. Create a list
postcardai lists create --name "Q1 Campaign"

# 3. Add contacts to list
postcardai lists add-contacts <list-id> <contact-id-1> <contact-id-2>

# 4. Create design
postcardai designs create --prompt "Spring sale announcement"

# 5. Check balance
postcardai balance

# 6. Estimate cost
postcardai estimate -r 100
```

### Pattern 4: Design Iteration

```bash
# 1. Create initial design
postcardai designs create --prompt "Modern minimalist postcard"

# 2. View result
postcardai designs get <design-id>

# 3. Iterate with refinement
postcardai designs iterate <design-id> --prompt "Make the colors warmer"

# 4. Publish preferred iteration
postcardai designs publish <design-id> --iteration 2
```

## Configuration Files

Credentials are stored in `~/.postcardai/config.json`:

```json
{
  "apiKey": "sk_live_...",
  "keyId": "key_...",
  "organizationId": "org_...",
  "organizationName": "My Company",
  "organizationSlug": "my-company",
  "environment": "live",
  "apiEnvironment": "production"
}
```

## Environment Variables

Override configuration with environment variables:

| Variable | Description |
|----------|-------------|
| `POSTCARDAI_API_URL` | Direct API URL override |
| `POSTCARDAI_ENVIRONMENT` | API environment: `local`, `staging`, `production` |

Priority: `POSTCARDAI_API_URL` > `POSTCARDAI_ENVIRONMENT` > config file > default (production)

## Postcard Sizes

| Size | Dimensions | Best For |
|------|------------|----------|
| `4x6` | 4" x 6" | Standard postcards, personal messages |
| `5x7` | 5" x 7" | Thank you cards, announcements |
| `6x9` | 6" x 9" | Marketing pieces, detailed designs |
| `6x11` | 6" x 11" | Large format marketing |

## Contact JSON Format

For batch import (`contacts batch`):

```json
[
  {
    "firstName": "John",
    "lastName": "Doe",
    "address1": "123 Main St",
    "address2": "Apt 4",
    "city": "Austin",
    "state": "TX",
    "zip": "78701"
  }
]
```

For batch match (`contacts match-batch`):

```json
[
  {
    "email": "john@example.com"
  },
  {
    "phone": "+15551234567"
  },
  {
    "propertyAddress": {
      "street": "123 Main St",
      "city": "Austin",
      "state": "TX",
      "zip": "78701"
    }
  }
]
```

## Anti-Patterns

### The Unauthenticated Command
**Pattern:** Running commands without checking auth first
**Problem:** Commands will fail with unclear errors
**Fix:** Always run `postcardai whoami` to verify auth before other commands

### The Unparseable Address
**Pattern:** Providing addresses in wrong format
**Problem:** Address parsing fails silently or creates invalid contacts
**Fix:** Use exact format: `"Name, Street, City, STATE ZIP"`

### The Forgotten Preview
**Pattern:** Sending mailings without preview for important campaigns
**Problem:** Design issues discovered after mailing is sent
**Fix:** Use `--preview` flag for important mailings, then review with `mailings previews`

## Diagnostic Process

When a user wants to send mail:

1. **Check authentication** - Run `postcardai whoami --verify`
2. **Identify the goal** - Quick send vs. campaign vs. design iteration
3. **Check prerequisites** - Balance, contacts, return address if needed
4. **Execute workflow** - Use appropriate pattern from above
5. **Verify result** - Check mailing status, preview if in preview mode

## What You Do NOT Do

- Never run commands that could spend credits without user confirmation
- Never assume authentication state without checking
- Never provide hardcoded API keys or credentials
- Never skip the preview step for large mailings without asking
- Never batch import contacts without confirming the file format

## Execution Strategy

### Sequential (Required Order)
1. Authentication must be verified before any API calls
2. Contact creation must complete before adding to lists
3. Design must be created before publishing
4. Mailing must be in preview mode before calling `mailings send`

### Parallelizable
- Fetching account info and balance can run in parallel
- Listing different resource types (contacts, designs, mailings) can parallelize
- Multiple contact lookups can run concurrently

## Context Management

### Approximate Token Footprint
- **Skill base:** ~3k tokens
- **With full command reference:** ~5k tokens

### When Context Gets Tight
- Prioritize: Authentication check, current command execution
- Defer: Full command reference, all options lists
- Drop: JSON format examples, workflow patterns
