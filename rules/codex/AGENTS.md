# AGENTS

Codex в этом student-kit работает по коротким рабочим правилам.

## Обязательные правила

1. Codex работает только по prompt от ChatGPT.
2. Один run = одна задача.
3. Если prompt противоречит правилам, нужно STOP.
4. Не читать и не печатать секреты:
   - `.env`;
   - `auth.json`;
   - tokens;
   - API keys;
   - Telegram bot token;
   - private SSH keys;
   - runtime secret files.
5. Приватные ключи никогда не печатать.
6. Для GitHub deploy key печатать только public key, если задача явно про это.
7. Не использовать `localhost` как user-facing URL.
8. Для ученика использовать только шаблоны:
   - `http://<SERVER_PUBLIC_IP>/<PATH>`
   - `http://<SERVER_PUBLIC_IP>:<PORT>/<PATH>`
9. Не hardcode IP тестового сервера.
10. Source/runtime разделять.
11. Не делать runtime-only fix как финал.
12. Не делать direct backend reply вместо Hermes.
13. Агент рабочий только через Hermes.
14. Fake/template reply не считать успехом.
15. Telegram - канал, не business tool.
16. Финансовая БД изменяется только deterministic tools.
17. Не делать Telegram API/model/provider calls, если prompt не разрешает.
18. Не писать production DB, если prompt не разрешает.
19. В docs-update run обновлять только явно указанные docs.
20. Документы состояния проекта:
    - roadmap;
    - technical spec;
    - module map;
    - project snapshot;
    - student dialogue context;
    - current status, если он есть.
21. Всегда возвращать отчёт между:
    - `CHATGPT_REPORT_BEGIN`
    - `CHATGPT_REPORT_END`
22. В отчёте указывать:
    - STATUS;
    - files changed;
    - checks;
    - git status;
    - commit/push;
    - safety.
23. Если не хватает фактов, нужно STOP, а не гадать.
