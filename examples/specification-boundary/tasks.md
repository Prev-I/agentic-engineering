# Tasks: Health Check Endpoint

## Task 1: Create health check module
- Create `src/health.ts` with a `checkHealth()` function
- Implement database connectivity check with 2-second timeout
- Implement cache connectivity check with 1-second timeout
- Return structured result with per-check status

## Task 2: Add HTTP endpoint
- Register `GET /health` route
- Call `checkHealth()` and map result to 200/503 response
- Format response body as specified JSON

## Task 3: Add tests
- Unit test: healthy when both checks pass
- Unit test: unhealthy when database check fails
- Unit test: unhealthy when cache check fails
- Unit test: response completes within timeout
- Integration test: endpoint returns expected status codes
