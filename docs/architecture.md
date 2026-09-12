# Architecture

## 1. New booking request

```text
Form Trigger
  ↓
Prepare and Validate Request
  ↓
IF Validation OK
  ├── false → Save Validation Error → Send Error Email
  ↓ true
Read Rooms ─────┐
                ├→ Merge / Wait → Find Available Room
Read Bookings ──┘                  ↓
                              IF Room Found
                         ┌─────────┴─────────┐
                       true                false
                         ↓                    ↓
                  Create Booking      Save Not Available
                         ↓                    ↓
                 Confirmation Email      No Room Email
```

## 2. Availability algorithm

The workflow reads both the room catalog and existing bookings before selecting a room.

A booking blocks a room when:

- `room_id` is the same;
- the booking status is active;
- requested and existing date intervals overlap.

Non-blocking statuses:

```text
cancelled
canceled
error
not_available
```

Overlap rule:

```text
existing_start < requested_end
AND
existing_end > requested_start
```

Checkout day is reusable. A booking ending on `2026-09-16` does not block another booking starting on `2026-09-16`.

## 3. Daily report

```text
Schedule Trigger
   ├→ Read Bookings ─┐
   └→ Read Rooms ────┤
                     ↓
                 Merge Data
                     ↓
              Build HTML Report
                     ↓
                  Gmail
```

The report calculates occupancy from bookings active on the report date instead of relying on a permanent `reserved` flag in the room catalog.

## 4. Production considerations

This repository intentionally uses Google Sheets because the project demonstrates n8n orchestration and business logic.

For a higher-load production system, replace Sheets with PostgreSQL or another transactional database and make availability check + booking creation atomic. Additional production improvements could include cancellation workflow, payment provider integration, idempotency, room inventory locking, retries and centralized error handling.