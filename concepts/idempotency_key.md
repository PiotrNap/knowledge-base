## Idempotency Key

**Summary:**
A unique identifier used to detect API requests which structure and payload haven't changed 
between seperate calls.

**Examples**
1. Cached response from an upload API endpoint:
- client sends a POST request with the file and idempotency-key attached
- server stores the information about the request shape and the returned response (file hash, idempotency-key, etc.)
- client makes another POST request with the exact same shape and idempotency-key
- server returns a cached response

2. Payment SDK hitting provider's API
- a payment request is submitted by the SDK client
- SDK stores the information about the given pay request
- when the SDK client makes another request for the same checkout, or if the UI page
was refreshed, the previous request payload (attached to an idempotency-key) will be used
to reconcile/continue the previous payment request cycle
