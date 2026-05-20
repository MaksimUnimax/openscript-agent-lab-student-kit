# Prompt кнопки: обновление документов

Назначение:
Этот prompt копируется из расширения, когда нужно сохранить текущий рабочий контекст проекта в документацию.

Перед использованием ChatGPT должен заменить placeholders:

- <STUDENT_PROJECT_NAME>
- <STUDENT_PUBLIC_REPO_URL>
- <STUDENT_WORKING_REPO_URL>
- <STUDENT_SERVER_PROJECT_ROOT>
- <STUDENT_DOCS_ROOT>
- <STUDENT_PUBLIC_DOCS_SOURCE>

Если значения неизвестны, ChatGPT должен STOP и запросить их у ученика.

PROMPT_TEXT_BEGIN

PROJECT_DOCS_UPDATE_INPUT_FOR_CHATGPT

Проект: <STUDENT_PROJECT_NAME>.

Назначение этого блока:
Это единый входной блок для ChatGPT, когда нужно сохранить текущий рабочий контекст проекта в документацию.

Этот блок НЕ является готовой командой Codex.

ChatGPT должен использовать этот блок так:

1. Сначала проверить актуальные правила и ТЗ в документации проекта ученика.
2. Потом найти в документации проекта последнюю сохранённую точку:
   - последний context append id;
   - последний roadmap append id;
   - последний module map append id;
   - последнее обновление current_status;
   - последнее обновление project snapshot;
   - время или идентификатор последнего docs update.
3. Потом вернуться в текущий диалог и взять только новый смысл после этой последней сохранённой точки.
4. Самостоятельно синтезировать, что нужно дописать:
   - в context;
   - в roadmap;
   - в module map;
   - в current_status;
   - в import_queue;
   - в index;
   - в manifests;
   - в project snapshot.
5. Только после этого подготовить чистую self-contained docs-only команду Codex.

Главное правило:
Codex не должен сам придумывать, что дописывать в документацию.
Всё, что нужно дописать, ChatGPT обязан написать в будущей команде Codex явно, готовым текстом.

Запрещено:
- отправлять Codex старые prompt’ы как есть;
- механически склеивать старые prompt’ы;
- писать “Codex, сам посмотри диалог и допиши”;
- писать “Codex, сам обнови roadmap по смыслу”;
- просить Codex реконструировать историю из памяти;
- сохранять мусорные строки, случайные ссылки, artifact-вставки, дубли;
- переписывать старую историю вместо append-only обновления;
- делать product fix в docs update run;
- менять application code;
- читать runtime secrets;
- запускать внешние API без явного разрешения;
- запускать provider/model calls;
- делать hardcode одного агента, чата, message_id, provider, файла, суммы или тестового случая.

Что ChatGPT должен понять перед созданием команды Codex:

1. Симптом.
   Важный рабочий контекст проекта находится в текущем ChatGPT dialogue и должен быть сохранён в repo-based project memory.

2. Причина уровнем выше.
   Если не обновить repo docs, следующий ChatGPT/Codex снова начнёт чинить старые куски по кругу и потеряет текущий active block.

3. Что записываем.
   Только новый рабочий контекст после последней сохранённой точки:
   - актуальный active block;
   - доказанные факты;
   - текущий stop-point;
   - правильный следующий технический блок;
   - запреты и критерии завершения;
   - roadmap append;
   - module map append;
   - context append;
   - manifest/status/index/snapshot updates.

4. Что не считаем решением.
   Docs update не является:
   - product fix;
   - auth fix;
   - implementation;
   - заменой будущей proof/design/fix работы.

5. Один следующий шаг.
   ChatGPT должен подготовить одну чистую docs-only команду Codex, где весь текст append-блоков уже написан inline.

Обязательный порядок работы ChatGPT:

1. Прочитать актуальные документы проекта ученика:
   - <STUDENT_DOCS_ROOT>/rules/
   - <STUDENT_DOCS_ROOT>/project/
   - <STUDENT_DOCS_ROOT>/roadmap/
   - <STUDENT_DOCS_ROOT>/context/
   - <STUDENT_DOCS_ROOT>/module_map/
   - <STUDENT_DOCS_ROOT>/project_snapshot/

2. Определить последнюю сохранённую точку документации:
   - last context append;
   - last roadmap append;
   - last module map append;
   - last status/snapshot update.

3. В текущем диалоге найти всё новое после этой точки.

4. Сопоставить новое с ТЗ проекта ученика.

5. Подготовить чистые append-блоки:
   - CONTEXT APPEND;
   - ROADMAP APPEND;
   - MODULE MAP APPEND.

6. Подготовить обновления:
   - manifests;
   - current_status;
   - import_queue;
   - index;
   - project_snapshot.

7. Сформировать будущую команду Codex:
   - RUN_MODE: docs_only;
   - только документация проекта;
   - append-only;
   - no application code changes;
   - no secrets;
   - no external API calls unless explicitly allowed.

Что будущая Codex-команда должна содержать:

1. RUN_ID.
2. RUN_MODE: docs_only.
3. Follow AGENTS.md.
4. ТЗ проверено.
5. TASK: repo docs memory update.
6. SYMPTOM.
7. HIGHER-LEVEL CAUSE.
8. WHAT MUST BE PROVEN.
9. WHAT IS NOT A SOLUTION.
10. ONE NEXT STEP.
11. PRECHECK.
12. DOCS_TO_READ with exact local paths.
13. DOCUMENTATION_GATE.
14. DOCUMENTATION_BASELINE.
15. RUNTIME_BASELINE.
16. ALLOWED CHANGE: only documentation paths.
17. FORBIDDEN_SCOPE.
18. UNIVERSALITY_CHECK.
19. INLINE PROJECT UPDATE CONTENT written by ChatGPT.
20. ROADMAP APPEND written by ChatGPT.
21. MODULE MAP APPEND written by ChatGPT.
22. CONTEXT APPEND written by ChatGPT.
23. MANIFEST UPDATES.
24. PROJECT SNAPSHOT UPDATE.
25. CURRENT STATUS UPDATE.
26. IMPORT QUEUE UPDATE.
27. INDEX UPDATE.
28. CHECKS.
29. COMMIT/PUSH if required by current project rules.
30. ACCEPTANCE.
31. REPORT.

Критерий качества для ChatGPT:
Если будущая команда Codex выглядит как склейка старых prompt’ов — это ошибка.
Если Codex должен сам придумать, что дописать — это ошибка.
Если append-блоки не написаны явно в команде — это ошибка.
Если не проверена последняя сохранённая точка в repo docs — это ошибка.
Если старый already-saved контекст дублируется — это ошибка.
Если текущий active block не сопоставлен с ТЗ — это ошибка.

END PROJECT_DOCS_UPDATE_INPUT_FOR_CHATGPT

PROMPT_TEXT_END
