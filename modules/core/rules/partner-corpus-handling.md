---
priority: critical
---

Design-partner document corpora are private data held under commercial agreement. They routinely
contain PII — contractor home and mobile numbers, unredacted campaign-finance filings, named
individuals in rosters and minutes — and per-file PII flags generally do not exist, so **no
individual file can be certified safe to publish**.

**`xberg-io/test_documents` is a PUBLIC repository.** Never commit partner-corpus bytes into it, and
do not assume a repo is private because it holds test fixtures. Check
(`gh repo view <repo> --json visibility`) before proposing to commit anything derived from partner
data, and say so explicitly when asking a human to approve such a commit — an approval given
without that fact is not an informed one.

**Share by content hash, not by copy.** Corpus objects live content-addressed in the private bucket:

```sh
gcloud storage cp gs://xberg-internal-test-documents/objects/<sha256> ./<local-name>
```

A sha256 identifies the exact file, lets another agent fetch it with credentials it already has,
and publishes nothing new. `scripts/fetch_corpus.py --auth` uses Application Default Credentials for
private buckets; without `--auth` it uses anonymous HTTPS and will not serve them. Keep anything
materialised out of git.

**No corpus filename and no personal name belongs in any created file** — not in findings docs, not
in commit messages, not in code comments, not in a message to another agent. Filenames frequently
embed both document titles and people's names. Refer to documents by an opaque id derived from the
path (`sha1(relpath)[:10]`) alongside the sha256 when the recipient needs to fetch it. When quoting
extracted text as evidence, prefer headings, numbers, or boilerplate, and redact any personal name
as `[NAME]`.

Run an exact-bytes leak check over everything a corpus-derived run writes, not a text grep — a
filename can be embedded inside a generated PDF's metadata where a text search will not find it.
