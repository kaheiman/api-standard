# MIME Types

## Request Format

Request messages with body **MUST** support the 'application/json' (JSON) format.

Request messages **MAY** also support the 'multipart/form-data' format for file upload, and `application/json-patch+json` for advanced patching.

## Response Format

All response messages **MUST** support the 'application/json' format.
