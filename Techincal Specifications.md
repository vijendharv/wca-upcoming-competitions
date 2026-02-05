# WCA Competition Lookup & Email Agent - Technical Specifications

## Overview
Agent that scrapes WCA competitions for Washington, Oregon, and British Columbia, then emails the results.

## Input Parameters
- **Regions**: `["Washington", "Oregon", "British Columbia"]`
- **Base URL**: `https://www.worldcubeassociation.org/competitions`
- **Email recipient(s)**: user-provided
- **Date range**: TODAY to +6 months (configurable)

## Data Extraction Requirements

### Source
- **Website**: https://www.worldcubeassociation.org/competitions
- **Method**: Web scraping or API (if available)
- **Filters**: Region-based search

### Data Fields Per Competition
```
competition_name: string
date_start: date
date_end: date
city: string
venue: string
address: string
state_province: string
country: string
registration_fee: float
currency: string (USD/CAD)
competitor_limit: integer
current_registrations: integer
registration_open_date: date
registration_close_date: date
registration_status: string (open/closed/full/waitlist)
events: array[string]
competition_url: string
organizers: array[string]
delegates: array[string]
```

## Processing Logic

### Filtering
```python
if competition.state_province in ["Washington", "Oregon", "British Columbia"]:
    if competition.date_start >= today():
        include_in_results = True
```

### Sorting
1. **Primary**: `date_start` (ascending)
2. **Secondary**: `state_province` (alphabetical)

## Email Output

### Email Headers
```
To: recipient_email
Subject: Upcoming WCA Competitions - WA/OR/BC - [Month Year]
From: agent_email
Content-Type: text/html; charset=utf-8
```

### Email Body Template
```html
<h1>Upcoming Cubing Competitions</h1>
<p>Found [COUNT] competitions in Washington, Oregon, and British Columbia</p>

<h2>WASHINGTON ([COUNT])</h2>
<!-- for each competition -->
<div style="border:1px solid #ccc; padding:10px; margin:10px 0;">
  <h3>[COMPETITION_NAME]</h3>
  <p><strong>Date:</strong> [DATE_START] - [DATE_END]</p>
  <p><strong>Location:</strong> [CITY], [STATE]</p>
  <p><strong>Venue:</strong> [VENUE]</p>
  <p><strong>Fee:</strong> $[FEE] [CURRENCY]</p>
  <p><strong>Competitors:</strong> [CURRENT]/[LIMIT]</p>
  <p><strong>Registration:</strong> [STATUS] (Opens: [OPEN_DATE], Closes: [CLOSE_DATE])</p>
  <p><strong>Events:</strong> [EVENT_LIST]</p>
  <p><a href="[URL]">View Competition Details</a></p>
</div>

<h2>OREGON ([COUNT])</h2>
<!-- repeat structure -->

<h2>BRITISH COLUMBIA ([COUNT])</h2>
<!-- repeat structure -->

<hr>
<p style="font-size:12px; color:#666;">
  Information current as of [TIMESTAMP]. 
  Please verify details at worldcubeassociation.org before registering.
</p>
```

## Technical Implementation

### Dependencies
```
requests
beautifulsoup4
smtplib (Python standard library)
email.mime (Python standard library)
datetime
```

### Core Functions
```python
scrape_competitions(regions, date_range)
parse_competition_data(html)
filter_competitions(data, regions, date_range)
sort_competitions(data)
format_email_html(competitions)
send_email(recipient, subject, body)
main(regions, recipient_email)
```

### Error Handling
- **Network errors**: Retry 3 times with exponential backoff
- **No competitions found**: Send email notification
- **Invalid email**: Validate before sending
- **Scraping failure**: Log error and alert admin

### Configuration File
```json
{
  "regions": ["Washington", "Oregon", "British Columbia"],
  "base_url": "https://www.worldcubeassociation.org/competitions",
  "email_from": "your-agent@example.com",
  "email_to": ["recipient@example.com"],
  "smtp_server": "smtp.gmail.com",
  "smtp_port": 587,
  "date_range_months": 6,
  "schedule": "weekly",
  "schedule_day": "Sunday",
  "schedule_time": "20:00"
}
```

## Execution Flow
1. Load configuration
2. Scrape WCA website for each region
3. Extract competition data
4. Filter by region and date
5. Sort results
6. Format HTML email
7. Send email to recipient(s)
8. Log results
9. Schedule next run

## API Alternative (If Available)
```
GET https://www.worldcubeassociation.org/api/v0/competitions

Parameters:
  - country_iso2: US, CA
  - state: Washington, Oregon, British Columbia
  - start: [TODAY]
  - end: [TODAY + 6 months]
```

## Rate Limiting
- Max 1 request per second
- Add 2-second delay between region searches
- Use caching for repeated requests within 1 hour

## Deployment Options
- Cron job (Linux/Mac)
- Task Scheduler (Windows)
- AWS Lambda + EventBridge
- GitHub Actions (scheduled workflow)
- Heroku Scheduler
