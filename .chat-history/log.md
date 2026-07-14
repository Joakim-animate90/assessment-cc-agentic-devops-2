# Chat History Log

---
- timestamp: "2026-07-14"
- user_prompt: "(session start — no explicit task; IDE had .github/workflows/ci.yaml open)"
- assistant_response_summary: "Read for prior context; no existing log found. Created .chat-history/log.md. Asked user what they'd like to do (review ci.yaml or other work)."
- files_affected: ".chat-history/log.md"

---
- timestamp: "2026-07-14"
- user_prompt: "Provided a CI summary that didn't match ci.yaml; then: use Node 14 and run tests in a Docker container to avoid incompatibility issues."
- assistant_response_summary: "Verified ci.yaml: user's summary mismatched actual (triggers, node 14 not 15, build needs [lint] only, cache-key bug 'deps-' vs 'node-modules-'). Flagged package.json engines require node >=15 (only a warning). Edited test job to run inside container image node:14, removed redundant setup-node, kept node-modules cache restore + CI:true. Noted unfixed build cache-key mismatch and build->test dependency gap."
- files_affected: ".github/workflows/ci.yaml, .chat-history/log.md"

---
- timestamp: "2026-07-14"
- user_prompt: "Audit CI, explain defects, fix workflow, validate locally. Then: use the repo Dockerfile as source of truth for tests."
- assistant_response_summary: "Full audit via Docker. Found: (D1) Node 14 impossible because .npmrc engine-strict=true + engines require node>=15/npm>=7; (D2) eslintConfig extends plugin:prettier/recommended but eslint-plugin-prettier missing AND prettier3 incompatible with eslint7's plugin version -> removed the extends; (D3) build cache key deps- vs node-modules-; (D4) build needs [lint] only; (D5) no install fallback; plus test needs REACT_APP_API_URL; plus 5 files unformatted. Fixes in ci.yaml: node 15 all jobs + test container node:15, build needs [lint,test], cache key fixed, npm install cache-miss fallback added, REACT_APP_API_URL added to test job, npm install (matching Dockerfile). package.json: removed plugin:prettier/recommended. Reformatted 5 src files via prettier --write (docker cp back, no bind mount). Validated end-to-end via Dockerfile build stage on node:15-alpine: install/build/lint/prettier/test all exit 0, 11/11 tests pass. Nothing committed."
- files_affected: ".github/workflows/ci.yaml, package.json, src/App.js, src/IPv4Addr.js, src/SubnetNumbersInput.js, src/index.js, src/lib/ipv4.js, .chat-history/log.md"

---
- timestamp: "2026-07-14"
- user_prompt: "Restated CI summary and asked to also add triggers."
- assistant_response_summary: "Updated ci.yaml 'on:' block: push branches now main, devel, stage, feature/**, bugfix/**; pull_request now main, devel, stage. Reminded user the other summary bullets (build needs [lint,test], npm install in every job, --watchAll=false, build cache-key bug) remain unapplied."
- files_affected: ".github/workflows/ci.yaml, .chat-history/log.md"
