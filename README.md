
# Лабораторна робота №8

## Тема роботи

Тестування відповідей LLM у RAG-сценарії за допомогою promptfoo на локальній моделі в ізольованому середовищі (Docker + Ollama).

## Мета роботи

Ознайомитися з автоматизованим тестуванням відповідей мовних моделей, навчитися створювати тест-кейси, налаштовувати promptfoo, порівнювати промпти та аналізувати отримані результати.

## Середовище виконання

- Docker — 29.9.3;
- образ Ollama — 0.40.16;
- модель — qwen3:0.6b;
- Node.js — v26.13.0 (фактично використана версія);
- npm — 11.20.0;
- promptfoo — 0.121.17.

Рекомендована версія Node.js — 24 LTS, зазначена у файлі `.nvmrc`.

## Структура репозиторію

```text
lab_8_llm_testing/
├── docs/
│   ├── doc1.txt
│   ├── doc2.txt
│   ├── doc3.txt
│   └── doc3_injected.txt
├── docs_individual/
│   ├── doc1.txt
│   ├── doc2.txt
│   ├── doc3.txt
│   └── doc3_injected.txt
├── .nvmrc
├── promptfooconfig.yaml
├── promptfooconfig_individual.yaml
└── README.md
```

## Інструкція із запуску

### 1. Клонування репозиторію

```powershell
git clone https://github.com/Sorrengai1/lab_8_llm_testing.git
cd lab_8_llm_testing
```

### 2. Перевірка середовища

Необхідно встановити Node.js, npm та Docker Desktop.
Рекомендований обсяг вільного дискового простору — не менше 10 ГБ.

```powershell
node -v
npm.cmd -v
docker --version
```

### 3. Запуск контейнера Ollama

Запустити Docker Desktop та виконати команду:

```powershell
docker run -d --name ollama -v ollama:/root/.ollama -p 127.0.0.1:11434:11434 --memory=2g --cpus=2 ollama/ollama:0.40.16
```

Перевірити стан контейнера:

```powershell
docker ps
docker exec ollama ollama --version
```

Порт 11434 має бути вільним перед запуском контейнера.

### 4. Завантаження мовної моделі

```powershell
docker exec -it ollama ollama pull qwen3:0.6b
docker exec ollama ollama list
```

Перевірка працездатності моделі:

```powershell
docker exec -it ollama ollama run qwen3:0.6b --think=false "Say hello"
```

### 5. Запуск основного тестування

```powershell
npx.cmd promptfoo@0.121.17 eval -c promptfooconfig.yaml
```

Основне завдання містить 8 тест-кейсів та 2 промпти.
Загальна кількість перевірок — 16.

### 6. Запуск індивідуального тестування

```powershell
npx.cmd promptfoo@0.121.17 eval -c promptfooconfig_individual.yaml
```

Індивідуальне завдання містить 5 тест-кейсів на тему бібліотеки та 2 промпти.
Загальна кількість перевірок — 10.

### 7. Повторне тестування без використання кешу

```powershell
npx.cmd promptfoo@0.121.17 eval -c promptfooconfig.yaml --no-cache
npx.cmd promptfoo@0.121.17 eval -c promptfooconfig_individual.yaml --no-cache
```

### 8. Перегляд результатів

```powershell
npx.cmd promptfoo@0.121.17 view
```

У вебінтерфейсі promptfoo можна переглянути матрицю результатів, відповіді моделі та статуси автоматизованих перевірок.

## Отримані результати

### Основне завдання

| Промпт | Пройдено / усього | Успішність |
|---|---:|---:|
| Промпт 1 | 7/8 | 87,5 % |
| Промпт 2 | 4/8 | 50 % |
| Загалом | 11/16 | 68,75 % |

### Індивідуальне завдання

| Промпт | Пройдено / усього | Успішність |
|---|---:|---:|
| Промпт 1 | 4/5 | 80 % |
| Промпт 2 | 4/5 | 80 % |
| Загалом | 8/10 | 80 % |

Результати наведено за фактично проведеними запусками.
Під час повторного тестування відповіді моделі можуть відрізнятися.

## Очищення середовища

Після завершення тестування за потреби можна видалити контейнер, том із моделлю та образ Ollama:

```powershell
docker stop ollama
docker rm ollama
docker volume rm ollama
docker rmi ollama/ollama:0.40.16
```

Перевірка результатів очищення:

```powershell
docker ps -a
docker volume ls
```

Після видалення контейнера, тому та образу для повторного тестування необхідно виконати кроки 3–4.

## Автор

Студентка групи НАІ-243  
Задорожна Анна
