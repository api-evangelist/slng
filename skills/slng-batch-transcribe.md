---
name: Batch transcribe audio with SLNG
description: Submit audio for asynchronous transcription, poll the job, and download the transcript files.
api: openapi/slng-batch-openapi.json
operations: [batchJobsCreate, batchJobsGet, batchJobsList, batchJobsFiles, batchJobsDelete]
---

# Batch transcribe audio with SLNG

Asynchronous transcription for large or many files. Base URL: `https://api.batch.slng.ai`.

## Auth
`Authorization: Bearer <SLNG_API_KEY>`.

## Steps
1. Submit a job: `POST /v1/batch/jobs` (`batchJobsCreate`). Three input modes:
   - `multipart/form-data` file upload,
   - JSON with a public `input_url`,
   - JSON presigned-S3 upload (`mode: "presign"` returns `200` with an upload URL;
     the job is created on a follow-up call).
   Supply a `transcription_config`; optionally a custom alphanumeric `job_id`
   (auto-generated if omitted) and `metadata`.
2. Non-presign submissions return `202 Accepted` with `status: QUEUED`.
3. Poll: `GET /v1/batch/jobs/{jobId}` (`batchJobsGet`) until complete; list all with
   `GET /v1/batch/jobs` (`batchJobsList`).
4. Download outputs: `GET /v1/batch/jobs/{jobId}/files` (`batchJobsFiles`).
5. Clean up: `DELETE /v1/batch/jobs/{jobId}` (`batchJobsDelete`).

## Notes
The client-supplied `job_id` acts as a stable handle for the job. Errors use the
`{ "error": ... }` envelope.
