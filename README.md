## Clase 5
### Unidades de Testeo / Pruebas Unitarias

- `assert`
- `pytest`
- Pruebas de Cadenas
- Organización de Pruebas en Carpetas
- Resumen

### Unidades de Testeo / Pruebas Unitarias

Hasta ahora, probablemente has estado probando tu propio código usando declaraciones `print`. Alternativamente, ¡puedes haber confiado en CS50 para probar tu código por ti! En la industria, es más común escribir código para probar tus propios programas.

En tu ventana de consola, escribe `code calculator.py`. Nota que puedes haber codificado este archivo en una clase anterior. En el editor de texto, asegúrate de que tu código se vea así:

```python
def main():
    x = int(input("¿Cuál es x? "))
    print("x al cuadrado es", square(x))


def square(n):
    return n * n


if __name__ == "__main__":
    main()
```

Observa que podrías probar el código anterior por tu cuenta usando algunos números obvios como 2. Sin embargo, considera por qué podrías querer crear una prueba que asegure que el código anterior funcione apropiadamente.

Siguiendo la convención, vamos a crear un nuevo programa de prueba escribiendo `code test_calculator.py` y modifica tu código en el editor de texto de la siguiente manera:

```python
from calculator import square


def main():
    test_square()


def test_square():
    if square(2) != 4:
        print("2 al cuadrado no fue 4")
    if square(3) != 9:
        print("3 al cuadrado no fue 9")


if __name__ == "__main__":
    main()
```

Observa que estamos importando la función `square` desde `square.py` en la primera línea de código. Por convención, estamos creando una función llamada `test_square`. Dentro de esa función, definimos algunas condiciones para probar.

En la ventana de la consola, escribe `python test_calculator.py`. Notarás que no se produce ninguna salida. ¡Podría ser que todo esté funcionando bien! Alternativamente, podría ser que nuestra función de prueba no haya descubierto uno de los “casos extremos” que podrían producir un error.

En este momento, nuestro código prueba dos condiciones. Si quisiéramos probar muchas más condiciones, nuestro código de prueba podría volverse fácilmente inflado. ¿Cómo podríamos expandir nuestras capacidades de prueba sin expandir nuestro código de prueba?

### `assert`

El comando `assert` de Python nos permite decirle al compilador que algo, alguna afirmación, es verdadera. Podemos aplicar esto a nuestro código de prueba de la siguiente manera:

```python
from calculator import square


def main():
    test_square()


def test_square():
    assert square(2) == 4
    assert square(3) == 9


if __name__ == "__main__":
    main()
```

Observa que estamos afirmando de manera definitiva lo que `square(2)` y `square(3)` deben ser. Nuestro código se reduce de cuatro líneas de prueba a dos.

Podemos romper intencionalmente nuestro código de calculadora modificándolo de la siguiente manera:

```python
def main():
    x = int(input("¿Cuál es x? "))
    print("x al cuadrado es", square(x))


def square(n):
    return n + n


if __name__ == "__main__":
    main()
```

Observa que hemos cambiado el operador `*` a `+` en la función `square`.

Ahora ejecutando `python test_square.py` en la ventana de la consola, notarás que se produce un `AssertionError` por parte del compilador. Esencialmente, esto es el compilador diciéndonos que una de nuestras condiciones no se cumplió.

Uno de los desafíos que enfrentamos ahora es que nuestro código podría volverse aún más oneroso si quisiéramos proporcionar una salida de error más descriptiva a nuestros usuarios. Plausiblemente, podríamos codificar de la siguiente manera:

```python
from calculator import square


def main():
    test_square()


def test_square():
    try:
        assert square(2) == 4
    except AssertionError:
        print("2 al cuadrado no es 4")
    try:
        assert square(3) == 9
    except AssertionError:
        print("3 al cuadrado no es 9")
    try:
        assert square(-2) == 4
    except AssertionError:
        print("-2 al cuadrado no es 4")
    try:
        assert square(-3) == 9
    except AssertionError:
        print("-3 al cuadrado no es 9")
    try:
        assert square(0) == 0
    except AssertionError:
        print("0 al cuadrado no es 0")


if __name__ == "__main__":
    main()
```

Observa que ejecutar este código producirá múltiples errores. Sin embargo, no está produciendo todos los errores anteriores. Esto es una buena ilustración de que vale la pena probar múltiples casos para que puedas detectar situaciones donde hay errores de codificación.

El código anterior ilustra un gran desafío: ¿Cómo podríamos facilitar la prueba de tu código sin docenas de líneas de código como las anteriores?

Puedes aprender más en la documentación de Python sobre `assert`.

### `pytest`

`pytest` es una biblioteca de terceros que te permite realizar pruebas unitarias de tu programa. Es decir, puedes probar tus funciones dentro de tu programa.

Para utilizar `pytest`, escribe `pip install pytest` en tu ventana de consola.

Antes de aplicar `pytest` a nuestro propio programa, modifica tu función `test_calculator` de la siguiente manera:

```python
from calculator import square


def test_assert():
    assert square(2) == 4
    assert square(3) == 9
    assert square(-2) == 4
    assert square(-3) == 9
    assert square(0) == 0
```

Observa cómo el código anterior afirma todas las condiciones que queremos probar.

`pytest` nos permite ejecutar nuestro programa directamente a través de él, de manera que podamos ver más fácilmente los resultados de nuestras condiciones de prueba.

En la ventana del terminal, escribe `pytest test_calculator.py`. Inmediatamente notarás que se proporciona una salida. Observa la F roja cerca de la parte superior de la salida, indicando que algo en tu código falló. Además, observa que la E roja proporciona algunas pistas sobre los errores en tu programa `calculator.py`. Basado en la salida, puedes imaginar un escenario donde `3 * 3` ha producido `6` en lugar de `9`. Basado en los resultados de esta prueba, podemos corregir nuestro código `calculator.py` de la siguiente manera:

```python
def main():
    x = int(input("¿Cuál es x? "))
    print("x al cuadrado es", square(x))


def square(n):
    return n * n


if __name__ == "__main__":
    main()
```

Observa que hemos cambiado el operador `+` a `*` en la función `square`, devolviéndola a un estado funcional.

Volviendo a ejecutar `pytest test_calculator.py`, observa cómo no se producen errores. ¡Felicitaciones!

En este momento, no es ideal que `pytest` deje de ejecutarse después de la primera prueba fallida. Nuevamente, devolvamos nuestro código `calculator.py` a su estado roto:

```python
def main():
    x = int(input("¿Cuál es x? "))
    print("x al cuadrado es", square(x))


def square(n):
    return n + n


if __name__ == "__main__":
    main()
```

Observa que hemos cambiado el operador `*` a `+` en la función `square`, devolviéndola a un estado roto.

Para mejorar nuestro código de prueba, modifiquemos `test_calculator.py` para dividir el código en diferentes grupos de pruebas:

```python
from calculator import square


def test_positive():
    assert square(2) == 4
    assert square(3) == 9


def test_negative():
    assert square(-2) == 4
    assert square(-3) == 9


def test_zero():
    assert square(0) == 0
```

Observa que hemos dividido las mismas cinco pruebas en tres funciones diferentes. Los marcos de pruebas como `pytest` ejecutarán cada función, incluso si hubo una falla en una de ellas. Al volver a ejecutar `pytest test_calculator.py`, notarás que se muestran muchos más errores. Más salida de errores te permite explorar más a fondo lo que podría estar produciendo los problemas dentro de tu código.

Habiendo mejorado nuestro código de prueba, devuelve tu código `calculator.py` a su estado completamente funcional:

```python
def main():
    x = int(input("¿Cuál es x? "))
    print("x al cuadrado es", square(x))


def square(n):
    return n * n


if __name__ == "__main__":
    main()
```

Observa que hemos cambiado el operador `+` a `*` en la función `square`, devolviéndola a un estado funcional.

Volviendo a ejecutar `pytest test_calculator.py`, notarás que no se encuentran errores.

Finalmente, podemos probar que nuestro programa maneja excepciones. Modifiquemos `test_calculator.py` para hacer justamente eso:

```python
import pytest
from calculator import square


def test_positive():
    assert square(2) == 4
    assert square(3) == 9


def test_negative():
    assert square(-2) == 4
    assert square(-3) == 9


def test_zero():
    assert square(0) == 0


def test_str():
    with

 pytest.raises(TypeError):
        square("gato")
```

Observa que en lugar de usar `assert`, estamos aprovechando una función dentro de la biblioteca `pytest` llamada `raises` que te permite expresar que esperas que se produzca un error. Necesitamos ir a la parte superior de nuestro programa y agregar `import pytest` y luego llamar a `pytest.raises` con el tipo de error que estamos esperando.

Nuevamente, al volver a ejecutar `pytest test_calculator.py`, notarás que no se encuentran errores.

En resumen, depende de ti como programador definir tantas condiciones de prueba como consideres necesarias.

Puedes aprender más en la documentación de `pytest`.

### Pruebas de Cadenas

Volviendo en el tiempo, considera el siguiente código `hello.py`:

```python
def main():
    name = input("¿Cómo te llamas? ")
    hello(name)


def hello(to="mundo"):
    print("hola,", to)


if __name__ == "__main__":
    main()
```

Observa que podríamos querer probar el resultado de la función `hello`.

Considera el siguiente código para `test_hello.py`:

```python
from hello import hello


def test_hello():
    assert hello("David") == "hola, David"
    assert hello() == "hola, mundo"
```

Al mirar este código, ¿crees que este enfoque para probar funcionará bien? ¿Por qué podría no funcionar bien esta prueba? Observa que la función `hello` en `hello.py` imprime algo: es decir, ¡no devuelve un valor!

Podemos cambiar nuestra función `hello` dentro de `hello.py` de la siguiente manera:

```python
def main():
    name = input("¿Cómo te llamas? ")
    print(hello(name))


def hello(to="mundo"):
    return f"hola, {to}"
```

Observa que cambiamos nuestra función `hello` para devolver una cadena. Esto significa que ahora podemos usar `pytest` para probar la función `hello`.

Al ejecutar `pytest test_hello.py`, ¡nuestro código pasará todas las pruebas!

Como en nuestro caso de prueba anterior en esta lección, podemos separar nuestras pruebas de la siguiente manera:

```python
from hello import hello


def test_default():
    assert hello() == "hola, mundo"


def test_argument():
    assert hello("David") == "hola, David"
```

Observa que el código anterior separa nuestras pruebas en múltiples funciones para que todas se ejecuten, incluso si se produce un error.

### Organización de Pruebas en Carpetas
c
El código de pruebas unitarias utilizando múltiples pruebas es tan común que tienes la capacidad de ejecutar una carpeta completa de pruebas con un solo comando.

Primero, en la ventana del terminal, ejecuta `mkdir test` para crear una carpeta llamada `test`.

Luego, para crear una prueba dentro de esa carpeta, escribe en la ventana del terminal `code test/test_hello.py`. Observa que `test/` instruye al terminal a crear `test_hello.py` en la carpeta llamada `test`.

En la ventana del editor de texto, modifica el archivo para incluir el siguiente código:

```python
from hello import hello


def test_default():
    assert hello() == "hola, mundo"


def test_argument():
    assert hello("David") == "hola, David"
```

Observa que estamos creando una prueba tal como lo hicimos antes.

`pytest` no nos permitirá ejecutar pruebas como una carpeta simplemente con este archivo (o un conjunto completo de archivos) solo sin un archivo especial `__init__`. En tu ventana del terminal, crea este archivo escribiendo `code test/__init__.py`. Nota el `test/` como antes, así como los dobles guiones bajos a cada lado de `init`. Incluso dejando este archivo `__init__.py` vacío, `pytest` está informado de que toda la carpeta que contiene `__init__.py` tiene pruebas que se pueden ejecutar.

Ahora, escribiendo `pytest test` en el terminal, puedes ejecutar toda la carpeta de pruebas de código.

Puedes aprender más en la documentación de `pytest` sobre mecanismos de importación.

### Resumen

Probar tu código es una parte natural del proceso de programación. Las pruebas unitarias te permiten probar aspectos específicos de tu código. Puedes crear tus propios programas que prueben tu código. Alternativamente, puedes utilizar marcos como `pytest` para ejecutar tus pruebas unitarias por ti. En esta clase, aprendiste sobre...

- Pruebas unitarias
- `assert`
- `pytest`
