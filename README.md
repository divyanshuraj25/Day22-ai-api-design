# Production-Grade AI API with FastAPI & Pydantic

A resilient, production-grade AI API built with **FastAPI** and **Pydantic v2**. This project demonstrates essential architectural patterns for serving AI models, including strict input validation, real-time token streaming, exponential backoff retries, request timeout safeguards, and standardized error schemas.

---

## 🚀 Key Features

- **Strict Input Validation (Pydantic v2):** 
  - Rejects inputs shorter than 5 characters or longer than 1000 characters.
  - Detects and filters out inputs containing only whitespace or non-alphanumeric special characters before sending them to the model pipeline.
- **Consistent Error Architecture:**
  - Standardized JSON error response across all endpoints containing `code`, `message`, and a unique `request_id` for traceability.
  - Handles `INPUT_INVALID` (HTTP 422/400), `RETRIEVAL_FAILURE` (HTTP 500), and `LLM_TIMEOUT` (HTTP 504 / Stream Error).
- **Streaming Responses (`StreamingResponse`):** 
  - Delivers real-time, token-by-token text streams to improve perceived latency and Time-To-First-Token (TTFT).
- **15-Second Hard Timeout:** 
  - Safeguard preventing requests from hanging indefinitely under heavy load or pipeline delays.
- **Resilience & Retry Logic:** 
  - Exponential backoff algorithm (`base_delay * 2^attempt`) catching transient rate limits (`RateLimitError`) and flaky network calls.

---

## 📐 Standardized Error Schema

Every error path adheres to the following contract:

```json
{
  "code": "ERROR_CODE",
  "message": "Human-readable description of what went wrong",
  "request_id": "7b20ffed-b6b3-481b-9336-02c93c39a957"
}
