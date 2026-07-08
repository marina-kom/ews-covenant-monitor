# 🏦 EWS — Early Warning System & Ковенант-монитор

> **Пет-проект:** Система раннего предупреждения и автоматического мониторинга 
> финансовых и нефинансовых ковенант корпоративного кредитования.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Documentation](https://img.shields.io/badge/docs-complete-green.svg)](docs/)

---

## 🎯 Бизнес-проблема

Крупный банк выдал кредиты на **₽120 млрд** корпоративным заемщикам. 
По условиям договоров клиенты обязаны поддерживать финансовые показатели 
(коверанты), но:

- ❌ Проверка выполняется **вручную** раз в квартал
- ❌ Риск-менеджеры узнают о нарушении **спустя месяцы**
- ❌ Нет единой картины: данные в АБС, ФНС, Excel-файлах
- ❌ **Alert fatigue** — 70% алертов ложные, реальные пропускаются

## 💡 Решение

**Event-driven платформа**, которая:
1. Агрегирует данные из АБС, ФНС, ФССП, БКИ, отчетности клиента
2. Автоматически рассчитывает ковенанты по **DMN-правилам**
3. Генерирует алерты в реальном времени с **калибровкой порогов**
4. Ведёт жизненный цикл алерта (workflow) для риск-офицеров

## 📐 Архитектура

```mermaid
graph LR
    ABS[АБС<br/>Core Banking] -->|Kafka| Ing[Ingestion<br/>Service]
    FNS[ФНС API] -->|REST| Ing
    FSSP[ФССП] -->|REST| Ing
    
    Ing -->|events| Kafka[(Apache<br/>Kafka)]
    
    Kafka --> Calc[Calculation<br/>Engine]
    Calc -->|events| Kafka
    
    Kafka --> Rule[Rule Engine<br/>DMN]
    Rule -->|events| Kafka
    
    Kafka --> Alert[Alert<br/>Management]
    Alert --> UI[Web UI<br/>Risk Officers]
    Alert -->|Webhook| ABS
