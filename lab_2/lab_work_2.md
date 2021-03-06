# Лабораторная работа №2

## Дано
1. Заданный английский текст большого размера.
Его нужно
[скачать](https://www.dropbox.com/s/47pl6kpbdedofbv/very_big_reference_text.txt?dl=0)
и положить в папку `lab_2`. Таким образом, повторить структуру:
```
|-- 2018-2-level-labs
  |-- lab_2
    |-- very_big_reference_text.txt
```
2. Произвольный английский текст в виде мнострочной строки.
Произвольность означает наличие любых симоволов, в том числе знаки
препинания и числа. Строка может быть пустой. Строка может содержать или
не содержать **орфографические ошибки**. Пунктуационные ошибки вне
задач лабораторной работы.
3. Список из допустимых слов. Слова могут быть произвольными. Список
может быть пустым.

Пример произвольного английского текста: 'Mary was quick to realize that
she had won the prize that was a desired thing that everyone wanted'

## Что надо сделать

### Шаг 0.1 Подготовка (проделать вместе с преподавателем на практике).

1. В имеющемся форке репозитория, обновить содержимое до последнего доступного
состояния в родительском репозитории.
2. Изменить файл `main.py`
3. Закоммитить изменения и создать pull request

### Шаг 0.2 Прочитать заданный текст из файла

Уже сделано, обратите внимание на содержимое файла `main.py` ближе к
концу:
```python
if __name__ == '__main__':
  with open('very_big_reference_text.txt', 'r') as f:
    REFERENCE_TEXT = f.read()
```

### Шаг 0.3. Получить частотный словарь по заданному тексту

Нужно импортировать код из Лабораторной работы №1. В описании к ней,
больше деталей о том, как эта функция должна работать.

Импорт уже сделан, обратите внимание на начало файла `main.py`:
```python
from lab_1.main import calculate_frequences
```

Остается корректно вызвать эту функцию с правильными аргументами.

### Шаг 1. Сформировать список слов-модификаций заданного слова

По заданному слову сформировать список слов-модификаций. В качестве
модификаций для слова из N букв считать (например, для слова `apple`):
1. удаление 1 буквы (всего N вариантов) - например, `pple`, `aple`,
`appe` и т.д.
2. добавление 1 буквы в любое место в строке (всего 26*(N+1) вариантов)
\- например, `aapple`, `bapple`, `capple` и т.д.
3. замена 1 буквы на любую другую (всего 26*N вариантов) - например,
`apple`, `bpple`, `cpple` и т.д.
4. перестановки 2 соседних букв (всего N-1 вариантов) - например,
`paple`, `apple`, `aplpe`, `appel`

Всего перестановок: N + 26*(N+1) + 26*N + N - 1 = 54*N + 25
(включая дубликаты).
Например, для слова `apple` - 54 * 5 + 25 = 295

**Важно:** список перестановок на выходе не должен содержать дубликаты.

Для простого случая `max_depth_permutations` всегда равен 1.

Интерфейс: 
```py
def propose_candidates(word: str, max_depth_permutations: int=1) -> list:
  pass
```

### Шаг 2. Очистить список кандидатов от заранее некорректных

Функция принимает на вход список всех кандидатов. Возвращает список, содержащий только тех кандидатов, которые есть в частотном словаре, построенному по большому массиву слов.

Например, `freq_dic = dict(list=1, lust=2)`,
`candidates=['lwst', 'lrst', 'list', 'lust', 'lyst']`. Выход:
`['list', 'lust']`


Интерфейс: 
```py
def keep_known(candidates: tuple, frequencies: dict) -> list:
  pass
```


### Шаг 3. Получить кандидата с наибольшей частотой (веро   ятностью)

Из списка кандатов выбрать только то слово, которое чаще всего
встречалось в заданном большом тексте. Функция должна возвращать объект
типа `str`. Если список кандидатов пуст, возвращать строку вида `'UNK'`.

Если вероятности одинаковые, возвращается слово, которое стоит первым по алфавиту. 
Фигурально это выглядит так: `'last' > 'list' > 'lost'`.

Интерфейс: 
```py
def choose_best(frequencies: dict, candidates: tuple) -> str:
  pass
```

### Шаг 4. Скрыть реализацию проверки орфографии за единственным интерфейсом

Объединить работу трех функций: `propose_candidates`, `keep_known`,
`choose_best` в одну. Кроме того,
если заданное слово уже есть в частотном словаре или в списке заданных слов, не требуется
осуществлять дальнейший подбор и проверку слова.

Интерфейс:
```py
def spell_check_word(frequencies: dict, as_is_words: tuple, word: str) -> str:
  pass
```

### Шаг 5. Осуществить автоматическую корректировку текста

**Важно:** выполнение заданий 1-4 + 5 соответствует оценке 10 баллов.

На вход приходит текст с ошибками в виде многострочной строки.
Требуется на выходе сформировать строку, в которой все орфографические
проблемы решены. Пунктуация должна сохраняться в неизменном виде.

Простой пример: стихи, оформленные как четверостишия, каждая строка с большой буквы, между ними есть
пропуски строк, должны в итоге превратиться все в те же четверостишия с тем же оформлением.

Интерфейс:
```py
def spell_check_text(frequencies: dict, as_is_words: tuple, text: str) -> str:
  pass
```

### Шаг 6. Осуществить подбор кандидатов с N изменениями относительно оригинального слова

**Важно:** выполнение задания совершенно необязательно и никак не влияет на оценку.
Наличие решения укрепит в вас уверенность в правильно выбранном пути и научит
обобщать. 

Начните с кандидатов, содержащих 2 изменения, затем 3. Попробуйте обобщить на
произвольную глубину. Оцените количество кандидатов для глубины 3.
Предложите способы уменьшения огромного числа кандидатов без потери точности.

Интерфейс:
```py
def propose_candidates(word: str, max_depth_permutations: int=1) -> list:
  pass
```