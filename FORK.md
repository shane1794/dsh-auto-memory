# English-only fork

Fork of [`Aik358/dsh-auto-memory`](https://github.com/Aik358/dsh-auto-memory) (BSD-3-Clause) with the
user interface and the model-facing text translated to English. Everything else — architecture,
storage format, tool names, retrieval behaviour — is unchanged.

## What was changed

**Default language.** The panel used to start in Chinese and follow the host language. It now starts in
English and stays there:

| Where | Before | After |
|---|---|---|
| `lib/client.js` — `var locale` | `'zh'` | `'en'` |
| `lib/client.js` — `var localeMode` | `'system'` | `'en'` |
| `lib/client.js` — `var sysLocale` | `'zh'` | `'en'` |
| `lib/index.js` — config `locale` | `'system'` | `'en'` |

**Strings.** Every user-visible string in the runtime was translated in place — the memory panel, the
settings page, the workbench skin (nine pages), the classic skin, toasts, error and empty states, tool
descriptions, and the prompt layers that the plugin injects into the model context. `I18N.en` was
completed (681 keys) and the two keys that only existed in `I18N.zh` were added.

**Metadata.** `package.json` description, `notices.json`, `README.md` and `docs/USER-GUIDE.en.md` are
English only. `locale/zh.json` and the `*.zh-CN.md` documents were removed.

**Bilingual tolerance for stored data.** Chinese is still accepted where it is data rather than copy:

- the calendar quadrant values (`重要紧急` … `未分类`) are enum values stored in the calendar file;
- handoff-ledger section headings are matched in both languages (`Goal` / `目标`,
  `Progress and next step` / `进度与下一步`, …) so ledgers written before the translation still parse
  and still land in the right kanban lane;
- Chinese time words, question markers and the stop-word table are parser input, not display text.

## What is still Chinese

Code comments, the inactive `I18N.zh` and `L10N.ja` dictionaries, and the historical documents under
`docs/internal/`, `docs/teamwork-impl/` and `docs/research/`. None of it is rendered by the plugin.

## Tests

The upstream suite is 443 source-guard scripts (`tests/smoke/`). On this fork a number of them fail
because they assert the original Chinese copy verbatim (`FAIL - 待确认区标注为「待人工确认」` and the
like) — that is the expected consequence of translating those strings, not a behavioural regression.
The i18n suite itself passes (`node tests/smoke/smoke-test-i18n-really.mjs` → 43/43), the skin
generator is in sync (`node tools/build-iter5-skin.mjs --check` → SYNC-OK), and every touched file
parses.
