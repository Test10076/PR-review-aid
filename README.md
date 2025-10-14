# mkdocs → n8n sandbox

This branch (`ci/staging`) builds a small MkDocs site and uploads the rendered
`site/` folder to an n8n webhook in chunked parts.

## How to run
1) Add the secret `N8N_WEBHOOK_URL_STAGING` in this repo (Settings → Secrets → Actions).
2) Push a change to this branch or use "Run workflow" in GitHub Actions.
3) n8n should receive parts with headers:
   - `X-Upload-Id`, `X-Total-Parts`, `X-Part-Index`, `X-Filename`, `X-Zip-Sha256`.

After reassembly (in n8n), unzip and run your QA flow on the output directory.
