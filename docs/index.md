# Sandbox Docs

Welcome! This site exists to test a GitHub Actions workflow that:
1) builds MkDocs in CI, and  
2) ships the rendered `site/` to an n8n webhook in chunked parts for QA.

---

## How this works

- **Build trigger:** Push to the branch (e.g., `ci/staging`) or run the workflow manually.
- **Build flags:** `mkdocs build --strict -q --clean`  
  - `--strict`: fail on warnings  
  - `-q`: quiet logs  
  - `--clean`: remove old artifacts
- **Artifact shipped:** `site.zip` containing this static site and a `build_manifest.json`.

> The receiver (n8n) reassembles chunks, verifies a SHA256, unzips, and runs QA.

---

## What n8n receives 

- **Files:** The complete `site/` directory as `site.zip`
- **Headers/fields:** `upload_id`, `total_parts`, `part_index`, `filename`, `zip_sha256`
- **Manifest (inside the zip):** `build_manifest.json` with:
  - source repo, branch, commit  
  - MkDocs version  
  - UTC build timestamp  
  - tree hash (SHA256 across all files)

---

## Quick checks

- The page you’re reading confirms MkDocs rendered HTML.
- To force a rebuild, edit this file and push a commit.
- In n8n, verify:
  - all parts arrived (0..N-1),
  - the final ZIP hash matches `zip_sha256`,
  - unzip completed without errors.

---

## Troubleshooting

- **Build failed on warnings:** Fix broken links/headings or disable strict mode (not recommended).
- **No upload to n8n:** Ensure `N8N_WEBHOOK_URL_STAGING` is set as an Actions secret.
- **413 / payload too large:** Lower `CHUNK_SIZE_MB` in the workflow.
- **Hash mismatch:** Confirm the receiver concatenates parts in numeric order.

---

## FAQ

**Q: Can I view build metadata?**  
A: Yes—open `build_manifest.json` in the extracted site folder on the n8n side.

**Q: How do I test errors?**  
A: Add a broken link (e.g., `[Missing](/nope/)`) and confirm your QA flow flags it.

---

## Next steps

- Replace this page with your actual project docs.
- Point the workflow to your production n8n webhook.
- Optional: attach a QA report to the zip (e.g., `qa_report.json`) so n8n can display results.


## Next steps

- Replace this page with your actual project docs.
- Point the workflow to your production n8n webhook.
- Optional: attach a QA report to the zip (e.g., `qa_report.json`) so n8n can display results.