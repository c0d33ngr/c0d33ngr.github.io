+++
title = 'N8n Github Issues Bounty Tracker'
date = 2025-09-13T05:56:13+01:00
author = 'c0d33ngr'
toc = true
draft = false
+++

# How I Built an Automated GitHub Bounty Tracker with n8n, Email, and WhatsApp Notifications

## Why I Needed This Automation

I was manually tracking GitHub bounty issues every day - searching for issues with bounty labels, copying URLs to spreadsheets, and trying to remember which ones I'd already seen. The process was repetitive and I kept missing new opportunities or tracking the same issues multiple times.

I decided to build an automated workflow using n8n that would monitor GitHub for bounty issues and notify me via email and WhatsApp when new ones appeared.

## Choosing n8n for the Workflow

I chose n8n because it offers a visual workflow builder that makes it easy to connect different APIs and services without writing complex code. The platform supports the integrations I needed: GitHub API, Google Sheets, email, and WhatsApp.

The workflow needed to:
- Search GitHub for issues with bounty labels
- Filter for recent issues (last 5 days)
- Store data in Google Sheets for tracking
- Send notifications via email and WhatsApp
- Keep the data updated with current issue status

## Building the Core Workflow

### Setting Up the GitHub Connection

I started by configuring the GitHub API connection in n8n. I created a personal access token in GitHub and added it to n8n's credentials manager. The workflow uses an HTTP Request node to query GitHub's search API with this query:

```
is:issue label:"💎 Bounty" state:open
```

I set up pagination to handle cases where there are more than 100 results, ensuring the workflow captures all available bounty issues.

### Processing the GitHub Data

After the API call, I used a Split Out node to process each issue individually instead of handling them as a batch. This allows the workflow to perform operations on each issue separately.

The data flows into a filter system that checks two conditions:
1. The issue was created within the last 5 days
2. The issue hasn't already been processed (by comparing against existing Google Sheets data)

### Google Sheets Integration

I created two Google Sheets to serve different purposes:

**public-bounty sheet** stores comprehensive data about all bounty issues:
- Issue Number
- Issue Name  
- Issue Repo URL
- Issue URL
- Issue Creation Date
- Issue State
- Issue Updated Date

**sent-issue sheet** tracks issues that were actually sent as notifications and includes additional details:
- Issue Number
- Issue Name
- Issue Repo URL
- Issue URL
- Issue Creation Date
- Number of Issue Comments
- Issue State
- Bounty Amount (extracted from labels)

The workflow uses Google Sheets nodes to read existing data, append new entries, and update changed information.

### Building the Notification System

For WhatsApp notifications, I connected n8n to the WhatsApp Business API. The message includes:
- Issue title
- Issue number
- Repository name
- Direct link to the issue
- Creation date

For email notifications, I created an HTML template that formats the information with GitHub-style styling. The email includes all the WhatsApp information plus additional details like comment count and bounty amount when available.

The HTML node generates a properly formatted email that gets sent through Gmail's API.

## Adding Data Management Features

### Duplicate Prevention

The workflow includes logic to prevent processing the same issue multiple times. Before adding new issues to the sheets or sending notifications, it checks against existing data in both Google Sheets.

### Status Updates

I added a secondary schedule trigger that runs every 6 hours to update existing records. This process:
1. Reads all existing issues from both sheets
2. Makes API calls to GitHub to get current status
3. Updates the sheets if the issue state or comment count has changed

This keeps the historical data accurate without requiring manual maintenance.

### Data Cleanup

The workflow automatically marks issues as "closed" in both spreadsheets when they're no longer open on GitHub. This prevents outdated information from cluttering the tracking sheets.

## Setting Up the Schedule Triggers

I configured two schedule triggers:

**Primary trigger (every hour)**: Searches for new bounty issues and sends notifications
**Secondary trigger (every 6 hours)**: Updates existing records with current GitHub status

This frequency ensures I get timely notifications for new opportunities while keeping the data current without overwhelming the GitHub API rate limits.

## Testing and Refinement

During testing, I discovered several issues that required refinement:

**Rate Limiting**: I had to ensure the workflow stayed within GitHub's API limits by spacing out requests appropriately.

**Data Validation**: Some issues had missing or malformed data, so I added error handling to process what was available rather than failing completely.

**Notification Filtering**: Initially, I was getting notifications for very old issues that happened to be updated recently. I refined the filtering logic to focus on creation date rather than update date.

## Results and Performance

The automated workflow now handles the entire bounty tracking process:
- Runs 24 times per day checking for new issues
- Maintains two comprehensive spreadsheets with 100% accuracy
- Delivers notifications within an hour of new bounty issues appearing
- Automatically maintains data integrity across all tracking systems

I went from spending 30-45 minutes daily on manual tracking to receiving curated notifications with zero manual effort.

## Key Technical Implementation Details

**GitHub API Integration**: Uses pagination and proper authentication to handle large result sets reliably.

**Google Sheets Management**: Implements upsert logic to prevent duplicates while ensuring all data stays current.

**Error Handling**: Includes fallback logic for missing data fields and API failures.

**Filtering Logic**: Multi-stage filtering prevents duplicate notifications and focuses on actionable items.

## Lessons Learned

Building this workflow taught me several important lessons about automation:

**Start Simple**: I began with just the GitHub search and built complexity gradually.

**Data Structure Matters**: Having two separate sheets with different purposes made the system much more manageable.

**Testing Is Critical**: Running the workflow manually many times helped identify edge cases before going live.

**Monitoring Is Essential**: I needed to watch the execution logs regularly during the first few weeks to catch issues early.

## Extending the System

The workflow foundation makes it easy to add new features:
- Additional notification channels (Slack, Discord)
- More sophisticated filtering based on repository reputation
- Integration with time tracking for bounty work
- Analytics dashboard for bounty trends

The modular design of n8n workflows means each of these additions would be separate nodes that integrate cleanly with the existing system.

## Why This Approach Works

This automation eliminates the repetitive manual work while providing better coverage than human monitoring could achieve. The system never forgets to check, never misses an update, and maintains perfect records of all activity.

For anyone dealing with similar repetitive data collection tasks, n8n provides an accessible way to build robust automation without extensive programming knowledge. The visual workflow design makes it easy to understand, modify, and extend the system as needs change.