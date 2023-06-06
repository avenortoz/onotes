# Behaviour patterns

## Chain of resposibility
Використання:
- Більше ніж один хендлер може обробляти івент, проте конретний хендлер не відомий завчасно
- Потрібно здійснити запит обробки івенту до декількох обєктів без вказання конкретного хендлера
- Перелік обробників може вказуватись динамічно
Наcлідки:
- Івент може бути не оброблений

Структура:
![image](chain_of_responsibility.png)

## Command
Використання:
- equvivalent to callbacks
- support undo and redo
- specify queue and execute requests at different times
- support loggin changes
- structure a system around high-level operations

Наслідки:
- decouple обєкт що викликає методи від обєкта що реалізує бізес логіку

Примітки:
Крутий патерн, коли один обєкт ралізує логіку, а інший її хоче використовувати,
але заміть того щоб створювати між ними сильний звязок, створюється команда, що
звязує ресівера та інвокера

Приклад реалізації:
![image](command.png)

## Interpretator
## Iterator
Структура:
![image](iterator.png)

## Mediator
Використання:
- існує визначена комунікація між обєктами, проте вона вельми складна.
- повторне використання обєкта є складним, бо він має звязок з великої кількістю обєктів 
- зміна поведінки потребує великої кількості підскласів

Наслідки:
- Обмежує кількість підкасів. Щоб змінити поведінку звязків між класами,
  достатньо наслідувати від медіатора
- decouple colleages
- робить простішим протокол взаємодії між класими
- абстаргує як обєкти взаємодіють

Структура:
![image](mediator.png)

## Memento

## Observer
Структура:
![image](observer.png)

## State
Структура:
![image](state.png)

## Strategy
Структура:
![image](strategy.png)

## Template method
- to implement invariant pats of an algorithm once and leave it up to subclasess
- when common behaviour among subclasess to avoid code duplication

Структура:
![image](template_method.png)

## Visitor
Використання:
- структура обєктів містить багато класів з різними інтерфейсами і необхідно
  здійснити операції над ними залежно від типу
- to apply single resposibility principle by avoiding polluting classes withe
  unraleted operation
- classes rarely change their structure
Наслідки:
- може порушувати інкапсуляцію

Структура:
![image](visitor.png)
