# Структурні патерини

## Adapter

![image](adapter.png)

## Bridge
Decouples abstraction from implementation

![image](bridge.png)

## Composit
- Використовується для відображення part-whole ієрархій
- для того щоб усунути відмінність між одиночним обєктом і його сукупністю 
- Використовувати, коли обєкти треба використовувати uniformly незалежно чи
проcтий обєкт чи складний

![image](compositor.png)

## Decorator
You can decorate existed object with additional functionality
Like there may be an object and you want to log every function call it does.
You don't need to change the class, so you create another class that derive
from base class for the object and make composition of the object delegating
the logic to it.

![image](decorator.png)

## Flyweight
Can let you exchange ram for memory reduction

![image](flyweight.png)

## Facade
Can help you create a layered application structure providing a useful
interface for working with classes and their relationships without too mush
fuss working with the structure and objects (Like "start movie" but not "close
window, turn off light, turn on TV and open Netflix")

![image](facade.png)

## Proxy
A proxy for the actual class. May be used for cashing, make efficiet calls not
when object is called but when it is actually needed and so on.

![image](./st/proxy.png)