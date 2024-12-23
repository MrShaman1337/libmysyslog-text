# text_log

`text_log` — это вспомогательная функция, предназначенная для записи текстовых сообщений в лог-файл. Она представляет собой упрощенную обертку над функцией `mysyslog`, автоматически устанавливающую формат записи как текстовый и драйвер как `0` (по умолчанию).

## Описание

Функция `text_log` позволяет записывать сообщения разного уровня логирования (например, `INFO`, `DEBUG`, `ERROR`) в указанный лог-файл в простом текстовом формате. Она подходит для случаев, когда JSON-формат или дополнительные параметры (например, `driver`) не требуются.

### Прототип функции

```c
int text_log(const char *message, int level, const char *log_path);
```

### Параметры

- `message` (const char *):  
  Строка с сообщением, которое нужно записать в лог.

- `level` (int):  
  Уровень логирования. Значения уровня должны соответствовать перечислению, например:
  - `LOG_DEBUG` (0): Отладочная информация.
  - `LOG_INFO` (1): Информационное сообщение.
  - `LOG_WARN` (2): Предупреждение.
  - `LOG_ERROR` (3): Ошибка.
  - `LOG_CRITICAL` (4): Критическая ошибка.

- `log_path` (const char *):  
  Путь к лог-файлу, в который будет записано сообщение.

### Возвращаемое значение

- Возвращает `0`, если запись в лог успешно выполнена.
- Возвращает `-1`, если произошла ошибка (например, невозможно открыть файл).

## Пример использования

```c
#include <stdio.h>

/* Пример вызова text_log */
int main(void)
{
    if (text_log("Информационное сообщение", LOG_INFO, "logfile.txt") != 0)
    {
        fprintf(stderr, "Не удалось записать в лог-файл.\n");
        return 1;
    }

    if (text_log("Ошибка приложения", LOG_ERROR, "logfile.txt") != 0)
    {
        fprintf(stderr, "Не удалось записать в лог-файл.\n");
        return 1;
    }

    return 0;
}
```

## Формат лога

Сообщения записываются в простой текстовый формат. Каждая строка лога включает:
1. Временную метку (в формате `YYYY-MM-DD HH:MM:SS`).
2. Уровень логирования.
3. Сообщение.

Пример содержимого лог-файла (`logfile.txt`):

```
2024-12-22 15:05:23 [INFO] Информационное сообщение
2024-12-22 15:06:45 [ERROR] Ошибка приложения
```


## Ограничения

- Функция предназначена исключительно для текстового формата. Для JSON-формата и других особенностей рекомендуется использовать функцию `mysyslog`.
