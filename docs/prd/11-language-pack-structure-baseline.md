# 11. Language Pack Structure (Baseline)
See `docs/figma-language-plugin-spec.md` for full schema. Summary:
- terms[]: { preferred, aliases[], forbidden[], note }
- rules[]: { id, desc, severity, pattern: {type, source}, fix, examples }
- locale, version, checksum/signature (optional), scopes (global/domain-specific)

Example (ko-KR):
```json
{
  "version": "1.0.0",
  "locale": "ko-KR",
  "terms": [
    { "preferred": "로그인", "aliases": ["로긴", "Login"], "forbidden": ["Sign in*"], "note": "UI copy uses 로그인" }
  ],
  "rules": [
    {
      "id": "ko-spacing-001",
      "desc": "띄어쓰기: ‘되다/돼’ 구분",
      "severity": "warning",
      "pattern": { "type": "regex", "source": "(?<=)됬|됏" },
      "fix": "됐",
      "examples": { "bad": "됬다", "good": "됐다" }
    }
  ]
}
```
