---
name: meeting-reminder
description: "When a meeting is scheduled on Calendly, email the requester a reminder 24 hours before the meeting starts."
version: 1.0.0
author: Maton AI
license: MIT
metadata:
  maton:
    tags: [Calendly, Email, Reminders, Scheduling]
---

# Calendly Meeting Reminder

Watches Calendly for newly scheduled meetings and sends a polite email
reminder to the requester (the invitee) 24 hours before the meeting starts.

## What it does

1. **Trigger** — A new event is scheduled in Calendly (`invitee.created`).
2. **Wait** — Sleep until 24 hours before `event.start_time`. If the meeting
   is already less than 24 hours away when the booking comes in, send the
   reminder immediately.
3. **Send** — Email the invitee a friendly reminder with the meeting time,
   organizer name, location/conference link, and a one-click reschedule link.
4. **Skip** — If the meeting was canceled in the meantime
   (`invitee.canceled`), do not send the reminder.

## Connections required

- **Calendly** — to receive new-booking webhooks and read event details.
- **Email provider** (Gmail, Outlook, or any SMTP relay configured in
  Maton) — to send the reminder.

## Inputs

| Field            | Description                                                      | Default |
| ---------------- | ---------------------------------------------------------------- | ------- |
| `lead_time`      | How far ahead of the meeting to send the reminder.               | `24h`   |
| `from_address`   | Sender email address.                                            | Connected mailbox default |
| `subject`        | Email subject template. Supports `{{event_name}}`, `{{when}}`.   | `Reminder: {{event_name}} tomorrow` |
| `body_template`  | Markdown email body. Variables listed below.                     | See below |

### Available template variables

- `{{invitee_name}}` — The invitee's full name.
- `{{event_name}}` — Calendly event type name (e.g., "30 Minute Meeting").
- `{{start_time_local}}` — Localized start time string in the invitee's
  timezone (e.g., `Tuesday, May 28 at 2:00 PM EDT`).
- `{{organizer_name}}` — Calendly host name.
- `{{location}}` — Zoom link, Google Meet link, phone number, or address.
- `{{reschedule_url}}` — Calendly reschedule link.
- `{{cancel_url}}` — Calendly cancel link.

## Default email body

```
Hi {{invitee_name}},

Just a quick reminder that we have **{{event_name}}** with {{organizer_name}}
coming up at **{{start_time_local}}**.

Joining details:
{{location}}

If something has come up, you can reschedule or cancel here:
- Reschedule: {{reschedule_url}}
- Cancel: {{cancel_url}}

Looking forward to it!
```

## Setup

1. In Maton, connect **Calendly** and your **email provider**.
2. Create a task from this template.
3. (Optional) Adjust `lead_time`, `subject`, or `body_template`.
4. Activate the task. Maton will subscribe to your Calendly webhook
   automatically.

## Edge cases handled

- **Bookings inside the lead window** — If a meeting is scheduled less than
  `lead_time` before its start, the reminder is sent right away.
- **Cancellations** — If the invitee cancels before the reminder fires, the
  scheduled email is dropped.
- **Reschedules** — If the invitee reschedules, the original reminder is
  dropped and a new one is queued for the updated start time.
- **Timezones** — `start_time_local` uses the invitee's timezone as
  reported by Calendly so the reminder reads naturally on their end.

## Limitations

- Calendly's webhook fires on the host's behalf, so the host must have
  the appropriate Calendly plan tier that exposes webhook subscriptions.
- This template only emails the invitee. To also notify the host, fork
  the template and add a second send step.
