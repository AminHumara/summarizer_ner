 # Парсер статей с извлечением сущностей, суммаризацией и Q&A

Этот проект представляет собой инструмент на Python, который позволяет:
1. Скачивать статьи по заданной ссылке.
2. Извлекать ключевые сущности (например, организации, имена, локации) с использованием Named Entity Recognition (NER).
3. Суммаризировать текст статьи.
4. Генерировать вопросы и ответы на основе извлеченных сущностей.

Проект использует современные NLP-модели из библиотек Hugging Face и spaCy для выполнения этих задач.

---

## Возможности

1. **Скачивание статей**:
   - Извлечение текста статьи по заданной ссылке с использованием `BeautifulSoup`.

2. **Очистка текста**:
   - Удаление лишних символов (например, zero-width spaces) и нормализация пробелов.

3. **Извлечение сущностей (NER)**:
   - Идентификация важных сущностей (организации, имена, локации) с использованием модели `en_core_web_sm` из spaCy.

4. **Суммаризация текста**:
   - Суммаризация статьи с использованием модели `facebook/bart-large-cnn` из Hugging Face.
   - Поддержка как коротких, так и длинных суммаризаций.

5. **Генерация вопросов и ответов (Q&A)**:
   - Генерация вопросов об извлеченных сущностях и поиск ответов с использованием модели `deepset/roberta-base-squad2`.

6. **Форматирование вывода**:
   - Форматирование длинных строк для удобного отображения на экране.

---

## Установка

Для запуска проекта необходимо установить требуемые библиотеки. Это можно сделать с помощью `pip`:

```bash
pip install requests beautifulsoup4 spacy torch transformers
```

# Использование

## Скачивание статьи:
Передайте URL статьи в функцию `scrape_article`, чтобы извлечь текст.

## Очистка текста:
Используйте функцию `clean_text` для удаления лишних символов и нормализации текста.

## Суммаризация статьи:
Используйте функцию `summarize_text` для генерации краткого или подробного summary. Вы можете указать максимальную и минимальную длину summary.

## Извлечение важных сущностей:
Используйте функцию `extract_important_entities`, чтобы выделить ключевые сущности из текста.

## Генерация вопросов и ответов:
Используйте функцию `ask_questions`, чтобы сгенерировать вопросы об извлеченных сущностях и получить ответы с помощью QA-модели.

## Отображение результатов:
Используйте функцию `display_long_string`, чтобы отформатировать и вывести результаты в удобном виде.

---

## Пример использования

```python
# 🔹 ЗАПУСК СКРИПТА 🔹
url = 'https://edition.cnn.com/2025/02/23/world/charts-ukraine-war-status-dg/index.html?iid=cnn_buildContentRecirc_end_recirc'

# Получение и очистка текста статьи
text = scrape_article(url)
cleaned_text = clean_text(text)

# Суммаризация статьи (длинная версия для Q&A, короткая для отображения)
summary_long = summarize_text(cleaned_text, max_length=400, min_length=300)
summary_short = summarize_text(cleaned_text, max_length=150, min_length=70)

# Извлечение важных сущностей
important_entities = extract_important_entities(summary_long, top_n=5)

# Генерация вопросов и ответов
qa_results = ask_questions(summary_long, important_entities)

# Отображение результатов
print("\n🔹 SUMMARY:\n")
display_long_string(summary_short, 80)
print("\n🔹 ВАЖНЫЕ СУЩНОСТИ:\n", important_entities)
print("\n🔹 ВОПРОСЫ И ОТВЕТЫ:")
for q, a in qa_results:
    print(f"❓ {q}\n➡️ {a}\n")
```

```output
🔹 SUMMARY:

Since Russia launched its full-scale invasion in 2022, Ukraine has lost about 11
% of its land. Millions of Ukrainians have been uprooted with thousands killed o
r injured. The United States has been the biggest single contributor of funding 
for Ukraine since the war began in 2022. Ukraine and its European allies are scr
ambling to adapt to the new approach from the United States.

🔹 IMPORTANT ENTITIES:
 [('Ukraine', 4), ('US', 2), ('UN', 2), ('Russia', 1), ('The United States', 1)]

🔹 Q&A:
❓ What is said about Ukraine in the text?
➡️ Millions of Ukrainians have been uprooted with thousands killed or injured

❓ What is said about US in the text?
➡️ The United States has been the biggest single contributor of funding for Ukraine

❓ What is said about UN in the text?
➡️ Human Rights Office

❓ What is said about Russia in the text?
➡️ Russia launched its full-scale invasion

❓ What is said about The United States in the text?
➡️ The United States has been the biggest single contributor of funding for Ukraine
