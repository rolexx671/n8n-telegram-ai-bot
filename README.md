# Telegram AI Bot — n8n + Claude + Voice + Google Sheets Memory

Умный Telegram-бот на n8n с постоянной памятью, поиском в интернете и поддержкой голосовых сообщений.

## Возможности

- **Claude AI** — умные ответы на любые вопросы
- **Поиск в интернете** (Tavily) — актуальная информация в реальном времени
- **Голосовые сообщения** — расшифровка через OpenAI Whisper
- **Постоянная память** — история диалогов в Google Sheets (15 последних обменов)

## Архитектура

```
Telegram → n8n Webhook
    ↓
[Голос?] → Whisper → текст
    ↓
Google Sheets → загрузка истории диалога
    ↓
Tavily → поиск в интернете
    ↓
Claude API → генерация ответа (история + поиск + вопрос)
    ↓
Google Sheets → сохранение диалога
    ↓
Telegram → отправка ответа
```

## Файлы

| Файл | Описание |
|------|----------|
| `n8n_telegram_bot_v3_sheets_memory.json` | **Основной workflow** (память + голос) |
| `n8n_telegram_bot_v2_memory_voice.json` | Версия 2 (RAM-память + голос) |
| `n8n_telegram_bot_workflow.json` | Версия 1 (базовый бот) |

## Быстрый старт

### 1. API ключи

| Сервис | Где взять |
|--------|-----------|
| Telegram Bot | [@BotFather](https://t.me/BotFather) → `/newbot` |
| Anthropic (Claude) | [console.anthropic.com](https://console.anthropic.com) |
| OpenAI (Whisper) | [platform.openai.com](https://platform.openai.com) |
| Tavily (поиск) | [tavily.com](https://tavily.com) |

### 2. Google Sheets

Создай таблицу с листом `ChatHistory` и заголовками:

| chatId | timestamp | userMessage | assistantMessage |

### 3. Переменные окружения в n8n

```
TELEGRAM_BOT_TOKEN = 7123456789:AAH...
ANTHROPIC_API_KEY  = sk-ant-...
OPENAI_API_KEY     = sk-...
TAVILY_API_KEY     = tvly-...
```

### 4. Импорт в n8n

**Через API:**
```bash
curl -X POST "https://твой-n8n.com/api/v1/workflows" \
  -H "X-N8N-API-KEY: твой_ключ" \
  -H "Content-Type: application/json" \
  -d @n8n_telegram_bot_v3_sheets_memory.json
```

**Или вручную:** n8n → Workflows → Import from file

### 5. Подключить Telegram Webhook

```
https://api.telegram.org/bot<ТОКЕН>/setWebhook?url=https://твой-n8n.com/webhook/tg-ai-bot-v3
```

## Стек

- [n8n](https://n8n.io) — автоматизация
- [Claude claude-sonnet-4-6](https://anthropic.com) — AI модель
- [OpenAI Whisper](https://openai.com/whisper) — speech-to-text
- [Tavily](https://tavily.com) — поиск в интернете
- Google Sheets — хранение истории
