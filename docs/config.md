# Configuration

For basic configuration instructions, see [this documentation](https://developers.openai.com/codex/config-basic).

For advanced configuration instructions, see [this documentation](https://developers.openai.com/codex/config-advanced).

For a full configuration reference, see [this documentation](https://developers.openai.com/codex/config-reference).

## Prompt-cache lifetime

For Responses API models and backends that support minimum cache lifetimes, set:

```toml
prompt_cache_ttl = "12h"
```

The valid values are `"30m"` and `"12h"`. Leaving the setting unset omits
`prompt_cache_options` from requests and preserves the backend's defaults.
The setting also works in a selected configuration profile file and as a CLI
override: `codex -c 'prompt_cache_ttl="12h"'`.

The option is sent as `prompt_cache_options.ttl` on HTTP and WebSocket Responses
requests, including WebSocket warmup and HTTP fallback. It preserves implicit
caching; it does not add explicit breakpoints or change the session cache key.
The selected model, provider, and account must support the requested lifetime.
Backend rejection is reported normally rather than silently retrying with a
different cache policy.

A 12-hour write has a higher cache-write rate than a 30-minute write. Select it
for context reused across longer gaps; it is a minimum cache lifetime, not a
guarantee of lower latency or a deletion deadline. See the
[OpenAI prompt-caching guide](https://developers.openai.com/api/docs/guides/prompt-caching#cache-lifetime).

## Lifecycle hooks

Admins can set top-level `allow_managed_hooks_only = true` in
`requirements.toml` to ignore user, project, and session hook configs while
still allowing managed hooks from requirements and managed config layers. This
setting is only supported in `requirements.toml`; putting it in `config.toml`
does not enable managed-hooks-only mode.
