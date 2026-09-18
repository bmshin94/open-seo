# OpenSEO (every-app/open-seo)

## 프로젝트 개요
매달 수십만 원씩 하던 비싼 해외 유료 검색엔진 최적화 도구를 내 손안에서 무료로 완벽히 대체하는 "오픈소스 검색엔진 최적화(SEO) 관제탑"
내 웹사이트의 검색 노출 순위, 키워드 경쟁력, 백링크 현황을 종합 분석하여 구글 1페이지 상위 노출 전략을 제시
마케팅 예산이 부족한 1인 창업가와 스타트업이 검색 트래픽을 폭발적으로 끌어모을 수 있도록 돕는 필수 나침반

## 핵심 특징 & 추천 분야
- 오픈소스SEO관제탑
- 검색엔진상위노출
- 키워드순위종합분석
- 비싼구독료완벽대체
- 스타트업트래픽성장

---
*이 문서는 오픈소스 큐레이터(Curator-Agent)에 의해 자동 생성된 가이드 문서입니다.*


---
## 기존 CLAUDE.md 내용

# Agent guidance

## Engineering principles

- Prefer simple, readable, flat code with minimal indirection.
- Search for existing implementations and installed libraries before creating new helpers or abstractions.
- Abstract when it prevents meaningful drift and makes the result simpler to maintain. Avoid speculative or one-use abstraction layers.
- Keep product data normalized and relationships explicit. Do not encode relational data in JSON or text merely to avoid joins.
- For new application-backed backend functionality, default to: TanStack server function → service → repository.
- Keep schema changes, queries, and mutations compatible with both SQLite and Postgres.
- Use idiomatic TypeScript. Use Zod to validate untrusted data and narrow runtime values at trust boundaries.
- Prefer established project helpers and libraries over hand-rolled implementations.
- Prefer idiomatic TanStack Query, Router, and Form patterns for server state, routing, and submitted forms.
- Specs under `specs/` are public design records: what a feature does, how it works, the alternatives considered and why they lost. No line numbers, migration mechanics, test plans, incidents, costs, or internal infrastructure details.

## Testing

- Don't add tests just for the sake of it. A test exists to enforce core behavior or a hard-to-spot edge case that could actually occur.
- Keep tests as simple as possible, and always review them looking for simplifications.
- Test behavior at the public entry point. Assert argument forwarding to a mocked collaborator only when that mapping is the contract (billing params, telemetry events).
- Statically import the module under test. `vi.mock` is hoisted, so per-test `await import()` and `vi.resetModules()` are banned unless module-level state must reset — comment why.
- Never re-declare a production class in a test. Import the real one; if the module is too heavy to import, move the class to a leaf module first (see `ga4Errors.ts`, `gscErrors.ts`).
- `beforeEach` sets default mock return values only. Vitest's `clearMocks` already resets call state — no `mockReset`/`mockClear` ceremonies.
- Fixtures contain only the fields the test asserts on or the types require. Shared shapes get a factory with overrides (see `ga4-test-fixtures.ts`, `tool-test-support.ts`); a fixture longer than its test's assertions is a smell.
- One test per invariant. Don't re-test Zod or a library, and don't repeat an output-schema round-trip in every happy path.
- Don't mock ORM builder chains. Test repositories through services or real SQL evaluation; chain mocks break on refactors that change no behavior.

## Log papercuts

When small, non-blocking repository friction occurs—a retried tool call, confusing setup step, flaky command, stale cache, misleading error, or non-obvious gotcha—use the `papercuts` skill and append it to `.agents/PAPERCUTS.md` in the moment. Continue the current task. Real bugs and tracked work are not papercuts, and sensitive data must never be logged.

Do not mine an entire session for papercuts or start a broad cleanup unless the user explicitly asks.

## Preserve review learnings

After a merge-ready or other code review verifies a finding, use `maintain-greptile-rules` only when the finding exposes a recurring or high-risk repository invariant that existing `.greptile/` context and automated checks do not capture. Do not promote one-off bugs or preferences into permanent review rules.

Changes to `.greptile/**`, `AGENTS.md`, `CLAUDE.md`, `.agents/skills/**`, and `.github/**` alter the review control plane and must receive explicit maintainer review. CODEOWNERS requests that review; where repository settings allow, enable GitHub's requirement for code-owner approval. Repository-specific rules live in `.greptile/`; maintainers should configure or retain a minimal org-enforced Greptile baseline for external-contribution, secret, authentication, billing, CI, and rule-tampering risks. Agents should report an unverified or missing baseline and must not mutate dashboard or organization rules without explicit user authorization.
