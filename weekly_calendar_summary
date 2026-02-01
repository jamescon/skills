# Weekly Calendar Summary Skill

## Purpose
Generate a comprehensive summary of calendar events for the current week or next week, organized by day with clear formatting.

## Instructions

### 1. Determine the Current Date and Week
- Always get the current date first to determine what "this week" and "next week" mean
- This week = Sunday through Saturday of the current week
- Next week = Sunday through Saturday of the following week
- If today is Sunday, "this week" starts today

### 2. Calculate Date Range
Based on the user's request:
- **"Current week" or "this week"**: Start from the most recent Sunday through the upcoming Saturday
- **"Next week"**: Start from the upcoming Sunday through the following Saturday

Example calculations:
- If today is Sunday Feb 1, 2026:
  - This week: Feb 1-7, 2026
  - Next week: Feb 8-14, 2026
- If today is Wednesday Feb 4, 2026:
  - This week: Feb 1-7, 2026
  - Next week: Feb 8-14, 2026

### 3. Retrieve Calendar Events
Use the Google Calendar connector with these parameters:
```
list_gcal_events with:
- calendar_id: "rik0o5qn88mbs1tciham6j9ctk@group.calendar.google.com" (Family calendar)
- time_min: [Start of week at 00:00:00 in Pacific timezone]
- time_max: [End of week at 23:59:59 in Pacific timezone]
- max_results: 50
```

Also check the primary calendar:
```
list_gcal_events with:
- calendar_id: "jamesmcon@gmail.com" (Primary calendar)
- Same time range as above
```

### 4. Format the Output
Present events in this structure:

```
## Week of [Start Date] - [End Date]

**[Day of Week], [Month Day]**
- [Time] - [Event Summary] ([Location if relevant])
- [Time] - [Event Summary]

**[Next Day]**
- [Time] - [Event Summary]
...
```

### 5. Formatting Guidelines
- Use 12-hour time format (e.g., "9:30 AM" not "09:30")
- Group events by day
- Include location for events that have one, especially medical appointments, sports games, or practices
- For all-day events, write "All day - [Event Summary]"
- If a day has no events, skip that day (don't list it)
- Add emoji for special occasions like birthdays (🎂)
- Provide a brief summary at the end highlighting key events (medical appointments, important deadlines, special occasions)

### 6. Timezone
- All events should be displayed in Pacific Time (America/Los_Angeles)
- Use RFC3339 format with timezone when calling the API

### 7. Error Handling
- If no events are found, inform the user that the week appears to be clear
- If the calendar returns an error, suggest checking calendar permissions

## Example Usage

**User:** "Summarize my week"
**Response:** 
- Determine today's date
- Calculate this week's Sunday-Saturday range
- Fetch events from both calendars
- Present organized summary

**User:** "What's on my calendar next week?"
**Response:**
- Determine today's date
- Calculate next week's Sunday-Saturday range
- Fetch events from both calendars
- Present organized summary

## Notes
- Always fetch from both the Family calendar and Primary calendar to ensure complete coverage
- Combine and sort events chronologically within each day
- Be concise but informative in event descriptions
