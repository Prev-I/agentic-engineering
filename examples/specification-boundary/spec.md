# Specification: Health Check Endpoint

## Requirements

1. The service exposes `GET /health` on the existing HTTP port.
2. A 200 response means the service is ready to receive traffic.
3. A 503 response means the service is not ready.
4. The response body is JSON: `{"status": "healthy"}` or `{"status": "unhealthy", "checks": {...}}`.
5. The endpoint checks:
   - Database connection: a simple query completes within 2 seconds.
   - Cache connection: a ping completes within 1 second.
6. If any check fails, the overall status is unhealthy and the failing check is named.
7. The endpoint must respond within 5 seconds under all conditions.
8. No authentication is required.
