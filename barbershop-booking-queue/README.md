# Barbershop Booking & Queue System

An end-to-end automation built in n8n that handles walk-in booking, queue management, and wait-time estimation for a barbershop — no app required, entirely Telegram-based on the client side.

## What it does
- Client messages a Telegram bot to book a service (Shave, Fade, Kids Cut, etc.)
- Bot presents available barbers via inline buttons
- System checks if the client already exists in the database, creates a new record if not
- Creates a queue entry linked to the client, barber, and service
- Calculates live queue position and estimated wait time based on service durations ahead in the queue
- Replies to the client with their position and wait time

## Stack
- **n8n** — workflow orchestration
- **Airtable** — data layer (Clients, Barbers, Services, Queue, Transactions)
- **Telegram Bot API** — client-facing interface

## Technical notes
- Handled Telegram's single-trigger-per-bot limitation by combining Message and Callback Query as multiple trigger types on one Trigger node
- Queue position and wait time calculated via a custom Code node summing linked service durations
- Client lookup/creation branch built with an If node checking for existing Telegram Chat ID matches, merged back into a single flow via a Merge node

## Status
Core booking and queue management flow is fully functional end-to-end — booking, queue creation, and wait-time reply all tested live. Additional modules (automated reminders/rebooking, payment tracking) are in progress.
