# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository (`sl-reminders`) is a lightweight automation system for scheduled Discord notifications related to the Seelen UI project. It contains no source code - only GitHub Actions workflows that run on scheduled intervals to post reminder messages to a Discord channel via webhooks.

## Architecture

The system consists of two main components:

### Workflows (`.github/workflows/`)
Four independent GitHub Actions workflows that send scheduled Discord messages:

1. **discord_server_boost.yml** - Reminds community members to boost the Discord server
   - Schedule: Wednesdays at 12:00 UTC, Saturdays at 10:00 UTC
   - Message: `messages/server_boost.md`

2. **discord_store_review.yml** - Encourages users to review Seelen UI on the Microsoft Store
   - Schedule: Tuesdays at 12:00 UTC, Fridays at 10:00 UTC
   - Message: `messages/store_review.md`

3. **discord_donation.yml** - Promotes donation options to support development
   - Schedule: Mondays at 12:00 UTC, Thursdays at 10:00 UTC
   - Message: `messages/donation.md`

4. **discord_socials.yml** - Promotes Seelen UI's social media presence
   - Schedule: Sundays at 14:00 UTC
   - Message: `messages/socials.md`

All workflows use the `tsickert/discord-webhook@v7.0.0` action and require a `DISCORD_WEBHOOK` secret to be configured in the repository settings.

### Messages (`messages/`)
Markdown files containing the Discord embed descriptions for each reminder type. These files can be edited and previewed directly in any Markdown editor.

## Common Operations

**Manually trigger any workflow:**
```bash
# Use GitHub CLI to manually dispatch any workflow
gh workflow run discord_server_boost.yml
gh workflow run discord_store_review.yml
gh workflow run discord_donation.yml
gh workflow run discord_socials.yml
```

**View workflow status:**
```bash
gh run list
gh run view <run-id>
```

## Editing Messages

Message content is stored in Markdown files in the `messages/` directory:
- `messages/server_boost.md` - Server boost reminder
- `messages/store_review.md` - Store review request
- `messages/donation.md` - Donation support message
- `messages/socials.md` - Social media promotion

To edit a message, simply modify the corresponding `.md` file. Changes will be reflected in the next scheduled run or manual trigger.

## Workflow Customization

Each workflow follows the same structure:
- Checkout repository to access message files
- Read message content from `messages/*.md`
- Send to Discord via webhook with customized embed settings

When modifying workflows:
- Test using `workflow_dispatch` before changing schedules
- Ensure `DISCORD_WEBHOOK` secret is properly configured
- Avatar URLs point to the Seelen-UI repository's static assets
- Embed colors use hex format (e.g., `0xf47fff` for server boost purple)
- Message files must be in `messages/` directory and referenced correctly