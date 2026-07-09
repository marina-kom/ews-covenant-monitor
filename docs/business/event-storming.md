# 🌪 Event Storming: Процесс мониторинга ковенант

> Версия: 1.0 | Воркшоп проведён: 2025-01

## Легенда

- 🟠 **Событие (Event)** — то, что произошло (прошедшее время)
- 🔵 **Команда (Command)** — действие, инициирующее событие
- 🟢 **Агрегат (Aggregate)** — сущность, к которой применяется команда
- 🟣 **Внешняя система (External System)**
- 🔴 **Hot Spot** — проблема / зона риска
- 🟡 **Политика (Policy)** — автоматическая реакция на событие

---

## 📌 Поток 1: Поступление данных и расчёт

```mermaid
graph LR
    style Ext1 fill:#9b59b6,color:#fff
    style Ext2 fill:#9b59b6,color:#fff
    style Ext3 fill:#9b59b6,color:#fff
    style Cmd1 fill:#3498db,color:#fff
    style Cmd2 fill:#3498db,color:#fff
    style Evt1 fill:#e67e22,color:#fff
    style Evt2 fill:#e67e22,color:#fff
    style Evt3 fill:#e67e22,color:#fff
    style Evt4 fill:#e67e22,color:#fff
    style Pol1 fill:#f1c40f,color:#000
    style Pol2 fill:#f1c40f,color:#000
    style Agg1 fill:#2ecc71,color:#fff

    Ext1[🟣 ФНС API] -->|данные о налогах| Cmd1[🔵 Загрузить внешние данные]
    Ext2[🟣 АБС] -->|остатки, транзакции| Cmd1
    Ext3[🟣 Клиент / ERP] -->|отчётность P&L, Balance Sheet| Cmd2[🔵 Загрузить отчётность]
    
    Cmd1 --> Evt1[🟠 Внешние данные загружены]
    Cmd2 --> Evt2[🟠 Отчётность загружена]
    
    Evt1 --> Pol1[🟡 Политика: При поступлении данных → запустить расчёт]
    Evt2 --> Pol1
    
    Pol1 --> Agg1[🟢 Ковенанта]
    Agg1 --> Evt3[🟠 Финансовые показатели рассчитаны]
