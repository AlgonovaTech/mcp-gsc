# GSC MCP — подключение для команды Algonova

Наш форк `AlgonovaTech/mcp-gsc` с захардкоженным **read-only** scope (безопасно).

## Что нужно
1. `uv` установлен (`curl -LsSf https://astral.sh/uv/install.sh | sh`)
2. Доступ к репо `AlgonovaTech/mcp-gsc`
3. **Service-account JSON** `seobot@seo-bot-488014` — НЕ в гите. Возьми у владельца SEO-проекта (Pavel) и положи в `~/.config/algonova/gsc-sa.json`, затем `chmod 600`.
   - У этого SA уже есть доступ к property `https://algonova.com/` (read).

## Установка
```bash
git clone https://github.com/AlgonovaTech/mcp-gsc.git ~/algonova-mcp-gsc

claude mcp add gsc -s user \
  -e GSC_CREDENTIALS_PATH=$HOME/.config/algonova/gsc-sa.json \
  -e GSC_SKIP_OAUTH=true \
  -- uvx --from $HOME/algonova-mcp-gsc mcp-gsc

# перезапусти Claude Code → проверь: claude mcp list  (gsc: ✓ Connected)
```

## Безопасность (важно)
- Scope = `webmasters.readonly` (строка 105 `gsc_server.py`) — запись физически невозможна.
- `GSC_ALLOW_DESTRUCTIVE` НЕ выставляем (деструктивные операции выключены).
- SA-ключ держим вне репо (`~/.config/algonova/`), `chmod 600`, НИКОГДА не коммитим.
- При обновлении с upstream — ревью диффа перед merge (форк под нашим контролем).
