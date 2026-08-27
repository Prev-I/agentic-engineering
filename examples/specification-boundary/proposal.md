# Proposal: Add Health Check Endpoint

## What
Add an HTTP health check endpoint that reports service readiness.

## Why
Container orchestrators need a reliable signal to determine whether the service
is ready to receive traffic. The current deployment relies on process presence,
which does not distinguish between a running process and a healthy one.

## Scope
- One new endpoint: `GET /health`
- Returns 200 when ready, 503 when not
- Checks database connectivity and cache availability

## Out of Scope
- Detailed diagnostics endpoint
- Authentication on the health endpoint
- Custom health check intervals
