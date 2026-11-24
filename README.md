# Студ-ИТ 2026 — Модуль B «Защита ИТ-инфраструктуры»

**Участник:** Элия Киролос Хайри Фавзи

Исследование безопасности учебного банковского микросервиса для переводов между счетами и просмотра истории транзакций. Исходный код не исправлялся.

## Результаты

**[Модель угроз и DFD — Threat_Model_Report.pdf](Threat_Model_Report.pdf)**  
Читабельная версия отчёта на основе модели OWASP Threat Dragon 2.6.2: без пустых таблиц, с отдельными разделами по угрозам. Исходный экспорт программы сохранён в [ThreatDragon_Original_Export.pdf](ThreatDragon_Original_Export.pdf). Модель STRIDE: клиент банка, REST API, PostgreSQL, Redis, потоки данных и границы доверия. Три критичных для бизнеса угрозы, их влияние, обоснование и контрмеры.

**[Исходная модель Threat Dragon — ThreatDragon_Model.json](ThreatDragon_Model.json)**  
Редактируемый файл модели угроз.

**[Результаты SAST — semgrep-results.json](semgrep-results.json)**  
Semgrep OSS 1.186.0: 19 срабатываний кастомных правил. Дополнительно два случая IDOR зафиксированы вручную и явно помечены как Manual source review. Итого 21 запись. Результаты общих правил и оригинальные машиночитаемые результаты также приложены.

**[Процесс обработки уязвимостей — Vulnerability_Management.pdf](Vulnerability_Management.pdf)**  
Результаты ревью, риск-ориентированная оценка, три угрозы и управление недостатками в SDLC. В приложениях — увеличенный DFD и подлинный скриншот запуска Semgrep.

## Материалы

| Файл | Назначение |
| --- | --- |
| [Threat_Model_Report.pdf](Threat_Model_Report.pdf) | Итоговый читаемый PDF: DFD и три угрозы с мерами защиты |
| [ThreatDragon_Original_Export.pdf](ThreatDragon_Original_Export.pdf) | Оригинальный PDF, экспортированный непосредственно из OWASP Threat Dragon 2.6.2 |
| [ThreatDragon_Model.json](ThreatDragon_Model.json) | Модель DFD/STRIDE для Threat Dragon |
| [semgrep-results.json](semgrep-results.json) | Объединённый список SAST и ручных IDOR |
| [semgrep-custom-results.json](semgrep-custom-results.json) | Первичный JSON 19 совпадений специальных правил |
| [semgrep-community-results.json](semgrep-community-results.json) | Первичный JSON сканирования 200 общими правилами |
| [semgrep-rules.yml](semgrep-rules.yml) | 19 правил для проверки Python и Compose |
| [Vulnerability_Management.pdf](Vulnerability_Management.pdf) | Отчёт о классификации находок и SDLC |
| [Vulnerability_Management.md](Vulnerability_Management.md) | Текстовый вариант отчёта |
| [microservice/](microservice/) | Исходный код задания без исправлений |
| [Evidence/01_DFD_Enlarged.png](Evidence/01_DFD_Enlarged.png) | Увеличенная диаграмма из исправленного отчёта на основе модели Threat Dragon |
| [Evidence/02_Semgrep_Console.png](Evidence/02_Semgrep_Console.png) | Скриншот реального терминала со сводкой Semgrep |

## Повторение проверки

Из корня данного репозитория:

```powershell
semgrep scan --config semgrep-rules.yml --json --output semgrep-custom-results.json microservice
```

Ручные IDOR-находки помечены отдельно в `semgrep-results.json`. Исходные результаты Semgrep оставлены без ручных изменений.

## Важное уточнение

В задании указана SQL Injection. Однако в предоставленном фрагменте динамический индекс SQL-параметра формируется сервером, а значение search передаётся в asyncpg отдельно. Эксплуатируемая SQLi на этом участке **не подтверждена**. Она оставлена в результате как INFO для ручной проверки, а не выдана за доказанный факт.

Ошибки импортов в app/main.py и отсутствие Dockerfile препятствуют непосредственному запуску неизменённой исходной версии. Поэтому данная работа — статический анализ и threat modeling, а не динамический тест работающего банковского сервиса.
