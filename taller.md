# Taller Python – Respuestas parte A
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

**Respuesta:** La salida de este programa es (2,1)

## 6.Identificar el error

```
class Caja:
__slots__ = ('x',)
c = Caja()
c.x = 10
c.y = 20
```
¿Qué ocurre y por qué?

**Respuesta:** Salta un Attribute error porque el atributo <y> no esta definido en el __slots__ por lo tanto no se crea

## 7.Completar espacios

Completa para que b tenga un atributo “protegido por convención”.
```
class B:
    def __init__(self):
        self ______ = 99
```
Escribe el nombre correcto del atributo.

**Respuesta:** self._numero = 99

## 8.Lectura de métodos “privados”

```
class M:
    def __init__(self):
        self._state = 0
    def _step(self):
        self._state += 1
        return self._state
    def __tick(self):
        return self._step()
m = M()
print(hasattr(m, '_step'), hasattr(m, '__tick'), hasattr(m,'_M__tick'))
```
¿Qué imprime y por qué?

**Respuesta:** Imprime (true, false, true) porque _step si está y es accesible, por lo tanto es true, __tick está, pero no es accesible porque es privado, por lo tanto false, y por ultimo, _M__tick es el nuevo nombre del método __tick, por lo tanto es accesible y devuelve true

## 9.Acceso a atributos privados

```
class S:
    def __init__(self):
        self.__data = [1, 2]
    def size(self):
        return len(self.__data)
s = S()
# Accede a __data (solo para comprobar), sin modificar el código de la clase:
# Escribe una línea que obtenga la lista usando name mangling y la imprima.
```
**Respuesta:**
```
class S:
    def __init__(self):
        self.__data = [1, 2]
    def size(self):
        return len(self.__data)
s = S()

print(s._S__data) 
```
## 10.Comprensión de dir y mangling
```
class D:
    def __init__(self):
        self.__a = 1
        self._b = 2
        self.c = 3
d = D()
names = [n for n in dir(d) if 'a' in n]
print(names)
```
¿Cuál de estos nombres es más probable que aparezca en la lista: __a, _D__a o a?
Explica.
**Respuesta:**  _D__a es la respuesta más probable ya que debido al name mangling el atributo privado __a cambia su nombre a esta nueva forma

# Taller Python - Respuestas Parte B. Encapsulación con @property y validación

## 11.Completar propiedad con validación
Completa para que saldo nunca sea negativo.
```
class Cuenta:
    def __init__(self, saldo):
        self._saldo = 0
        self.saldo = saldo
    @property
    def saldo(self):
        ______
    @saldo.setter
    def saldo(self, value):
        # Validar no-negativo
        ______
```
**Respuesta:**
```
class Cuenta:
    def __init__(self, saldo):
        self._saldo = 0
        self.saldo = saldo
    @property
    def saldo(self):
        return self._saldo
    @saldo.setter
    def saldo(self, value):
        if (value >= 0):
            self._saldo = value
        else:
            print("El valor no puede ser un numero negativo")
```

## 12. Propiedad de solo lectura
Convierte temperatura_f en un atributo de solo lectura que se calcula desde
temperatura_c.

```
class Termometro:
    def __init__(self, temperatura_c):
        self._c = float(temperatura_c)
# Define aquí la propiedad temperatura_f: F = C * 9/5 + 32
```
Escribe la propiedad

**Respuesta:**
```
class Termometro:
    def __init__(self, temperatura_c):
        self._c = float(temperatura_c)
    @property
    def temperatura_f(self):
        return self._c * 9 / 5 + 32
        
```
## 13. Invariante con tipo
Haz que nombre sea siempre str. Si asignan algo que no sea str, lanza TypeError.
```
class Usuario:
    def __init__(self, nombre):
        self.nombre = nombre
    # Implementa property para nombre
```
**Respuesta**
```
class Usuario:
    def __init__(self, nombre):
        self.nombre = nombre
    @property
    def nombre(self):
        return self._nombre
    @nombre.setter
    def nombre(self, valor):
        if isinstance(valor, str):
            self._nombre = valor
        else:
            print("El nombre debe ser una cadena de texto")
```
# 14. Encapsulación de colección
Expón una vista de solo lectura de una lista interna.

```
class Registro:
    def __init__(self):
        self.__items = []
    def add(self, x):
        self.__items.append(x)
# Crea una propiedad 'items' que retorne una tupla inmutable con el contenido
```
**Respuesta**
```
class Registro:
    def __init__(self):
        self.__items = []
    def add(self, x):
        self.__items.append(x)
    @property
    def items(self):
        tuplaInmutable = tuple(self.__items)
        return tuplaInmutable
```
# Taller Python - Respuestas Parte C. Diseño y refactor
## 15. Refactor de encapsulación
Refactoriza para evitar acceso directo al atributo y validar que velocidad sea entre 0 y
200.
```
class Motor:
    def __init__(self, velocidad):
        self.velocidad = velocidad # refactor aquí
```
Escribe la versión con @property.

**Respuesta:** 
```
class Motor:
    def __init__(self, velocidad):
        self.velocidad = velocidad 

    @property
    def velocidad(self):
        return self.__velocidad

    @velocidad.setter
    def velocidad(self, valor):
        if (0 <= valor <= 200):
            self.__velocidad = valor
        else:
            print("velocidad no valida")
```
## 16. Elección de convención
Explica con tus palabras cuándo usarías _atributo frente a __atributo en una API
pública de una librería.

**Respuesta:** 

Supongo que _atributo la usaría en casos donde no son datos delicados, es decir, datos que no deben ser cambiados desde el usuario, pero que al mismo tiempo no signifique un riesgo dejarlos accesibles, para asi facilitar el codigo. En cambio __atributo solo lo usaria si de verdad necesito una encapsulacion completa

## 17. Detección de fuga de encapsulación

¿Qué problema hay aquí?
```
class Buffer:
    def __init__(self, data):
        self._data = list(data)
    def get_data(self):
        return self._data
```
Propón una corrección.




