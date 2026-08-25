---
priority: critical
---

The dominant defect shape here is a check that passes because it examined nothing. Before trusting
a green result: make it fail on purpose once, to prove it is wired to what you think it is wired
to. Read the count or the diff, not the word "ok" — a `0 files`, an empty match set, and a real
pass can render identically. Check the artifact actually in effect at runtime — generated output,
committed content, a pushed ref — not just the source you edited; a source-only grep can be clean
while the rendered or fetched result still carries the bug, and an uncommitted scratch file can
report clean regardless of content. A gate scoped to a path or a diff is blind to any change that
doesn't touch those paths — confirm the filter matches the shape it exists to catch.
