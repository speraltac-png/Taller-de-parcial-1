# Taller Python – Respuestas
## 1. Seleccion multiple

Dada la clase:
```
class A:
    x = 1
    _y = 2
    __z = 3
    a = A()
```
¿Cuáles de los siguientes nombres existen como atributos accesibles directamente desde
a?
  - a) **a.x**
  - b) **a._y**
  - c) a.__z
  - d) **a._A__z**
    
  **Respuesta correcta:** a , b , d)

## 2. Salida del programa

```
class A:
  def __init__(self):
    self.__secret = 42
  a = A()
  print(hasattr(a, '__secret'), hasattr(a, '_A__secret'))
```
¿Qué imprime?

**Respuesta:** El programa imprime (false, true)

## 3. Verdadero o falso
  - a) El prefijo _ impide el acceso desde fuera de la clase. **falso**, Aunque no es recomendado acceder desde fuera de la clase, si es posible hacerlo
  - b) El prefijo __ hace imposible acceder al atributo. **falso**, Aunque lo hace más dificil de acceder, si es podible acceder al atributo mediante: objeto._nombre_de_clase__atributo 
  - c) El name mangling depende del nombre de la clase. **vardadero**, El nombre del atributo es cambiado por objeto._nombre_de_clase__atributo, por lo tanto si depende del nombre de la clase

## 4. Lectura de código

```
class Base:
    def __init__(self):
        self._token = "abc"
class Sub(Base):
    def reveal(self):
        return self._token
print(Sub().reveal())
```
¿Qué se imprime y por qué no hay error de acceso?

**Respuesta:** El programa imprime "abc", no presenta ningún error de acceso ya que se llama a la clase Sub, que hereda de Base el atributo _token, por otro lado, no se está llamando directamente al atributo, sino que se está retornando mediante un getter de la clase Sub

## 5.Name mangling en herencia

```
class Base:
    def __init__(self):
        self.__v = 1
class Sub(Base):
    def __init__(self):
        super().__init__()
        self.__v = 2
    def show(self):
        return (self.__v, self._Base__v)
print(Sub().show())

```
¿Cuál es la salida?
