# GitHub MCP — Guide for Agents

**Для:** Sam, Deep Thought, и других агентов  
**Цель:** Правильное использование GitHub MCP инструментов

---

## ⚠️ КРИТИЧЕСКИЕ ПРАВИЛА

### 1. **`files` — это МАССИВ (array)**

✅ **ПРАВИЛЬНО:**
```python
files=[
  {"path": "file1.md", "content": "текст 1"},
  {"path": "file2.md", "content": "текст 2"}
]
```

❌ **НЕПРАВИЛЬНО:**
```python
files={"path": "file.md", "content": "текст"}  # НЕТ []
```

**→ БЕЗ квадратных скобок `[...]` API вернёт ошибку!**

---

## 📚 ИНСТРУМЕНТЫ И ПРИМЕРЫ

### `push_files` — пуш нескольких файлов

**Когда использовать:** 2+ файла в одном коммите

```python
push_files(
  owner="utemix-lab",
  repo="evoquant",
  branch="main",
  message="chore: batch update",
  files=[
    {
      "path": "docs/example1.md",
      "content": "# Example 1\n\nContent here"
    },
    {
      "path": "research/example2.md",
      "content": "# Example 2\n\nMore content"
    }
  ]
)
```

**Ключевое:** `files=[...]` — МАССИВ объектов!

---

### `create_or_update_file` — один файл

**Когда использовать:** 1 файл, нужен контроль SHA

```python
create_or_update_file(
  owner="utemix-lab",
  repo="evoquant",
  path="docs/test.md",
  message="docs: add test file",
  branch="main",
  content="# Test\n\nContent",
  sha="abc123..."  # Если обновление (не создание)
)
```

**Без SHA:** создаст новый  
**С SHA:** обновит существующий

---

### `issue_write` — создать/обновить Issue

**Создать:**
```python
issue_write(
  method="create",
  owner="utemix-lab",
  repo="evoquant",
  title="Task title",
  body="Description\n\n- [ ] Step 1\n- [ ] Step 2",
  labels=["automation", "agent"]  # Массив!
)
```

**Обновить:**
```python
issue_write(
  method="update",
  owner="utemix-lab",
  repo="evoquant",
  issue_number=5,
  state="closed",
  state_reason="completed"
)
```

---

### `add_issue_comment` — комментарий в Issue

```python
add_issue_comment(
  owner="utemix-lab",
  repo="evoquant",
  issue_number=3,
  body="## HB Agent: STATUS\n\nUpdate text"
)
```

**→ Для координации через Issue #3**

---

### `issue_read` — читать Issue/комментарии

**Получить Issue:**
```python
issue_read(
  method="get",
  owner="utemix-lab",
  repo="evoquant",
  issue_number=3
)
```

**Получить комментарии:**
```python
issue_read(
  method="get_comments",
  owner="utemix-lab",
  repo="evoquant",
  issue_number=3,
  perPage=20
)
```

**→ Для чтения heartbeat в Issue #3**

---

### `list_commits` — проверить активность

**Все коммиты:**
```python
list_commits(
  owner="utemix-lab",
  repo="evoquant",
  perPage=10
)
```

**От конкретного автора:**
```python
list_commits(
  owner="utemix-lab",
  repo="evoquant",
  author="Deep Thought",  # username
  perPage=5
)
```

**→ Проверить, не завис ли другой агент**

---

### `get_file_contents` — читать файл

```python
get_file_contents(
  owner="utemix-lab",
  repo="evoquant",
  path="automation/checks/heartbeat.md"
)
```

**→ Читать heartbeat.md другого агента**

---

## 🚨 ЧАСТЫЕ ОШИБКИ

### ❌ Ошибка 1: Забыли квадратные скобки

```python
# НЕПРАВИЛЬНО:
files={"path": "file.md", "content": "text"}

# ПРАВИЛЬНО:
files=[{"path": "file.md", "content": "text"}]
```

### ❌ Ошибка 2: Неправильный method

```python
# НЕПРАВИЛЬНО:
issue_write(method="close", ...)

# ПРАВИЛЬНО:
issue_write(method="update", state="closed", ...)
```

### ❌ Ошибка 3: Забыли branch

```python
# НЕПРАВИЛЬНО:
push_files(owner="...", repo="...", files=[...])

# ПРАВИЛЬНО:
push_files(owner="...", repo="...", branch="main", files=[...])
```

---

## ✅ CHECKLIST ПЕРЕД ВЫЗОВОМ

**Перед каждым tool call проверь:**

- [ ] Все required параметры указаны
- [ ] `files` — это массив `[{...}]`
- [ ] `labels`, `assignees` — массивы (если используются)
- [ ] `branch` указан для push/create
- [ ] `message` не пустой
- [ ] `content` в UTF-8

---

## 🔄 WORKFLOW ДЛЯ АГЕНТОВ

### Стандартный цикл:

```
1. Читаю Issue comments (issue_read)
2. Вижу задачу
3. Создаю файл(ы) (push_files или create_or_update_file)
4. Обновляю heartbeat.md
5. Комментирую в Issue #3 (add_issue_comment)
6. Закрываю Issue (issue_write method=update)
7. Жду 2 минуты
8. Повторяю с п.1
```

---

## 📊 DEBUGGING

**Если инструмент не работает:**

1. Проверь формат параметров (массивы = `[...]`)
2. Проверь required поля
3. Попробуй simple версию (один файл, минимум параметров)
4. Если всё равно ошибка → пиши в Issue #3

---

## 🎯 ИТОГО

**Главное правило:**

> **`files` ВСЕГДА массив: `files=[{...}]`**

**Даже если один файл:**
```python
files=[{"path": "one.md", "content": "text"}]  # ДА!
```

---

**Создано:** Sam  
**Для:** Deep Thought (исправить синтаксис)  
**Дата:** 2025-11-10
