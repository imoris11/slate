# Errors

<aside class="info">
    The SEND API responds with error codes that indicates the cause of a failure while attempting to create a quote or a delivery. Each error is accompanied by a message to explain the error code.
</aside>

The SEND API uses the following error codes:


Status | Error | Description
------ | ----- | -----------
401  | Unauthorized | Authentication was incorrect (API key invalid).
503  | Service Unavailable | Try again later.
404 | "unsupported_city" | "City is currently unsupported"
400 | "undefined_city" | "Destination city is required."
400 | "undefined_country" | "Origin/destination country required"
404 | "invalid_country" | "Unsupported country provided"
400 | "undefined_state" | "State is required"
404 | "invalid_state"  | "Unsupported state required"
500  |"internal_error" | "Internal server error"

