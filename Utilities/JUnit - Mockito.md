---
tags: utilities java backend
---
----

[Documentación]()

----

# ¿Qué es

Con JUnit 5 es una biblioteca de java que permite escribir pruebas unitarias y ejecutarlas en la JVM. Utiliza programación funcional y lambda e incluye diferentes estilos de pruebas, configuraciones, anotaciones, etc.

A diferencia de JUnit 4 la nueva version ya no es monolitica y se compone de 3 grandes utilidades:
- JUnit Platform: Encargada del contexto de ejecución del test (plataforma).
- JUnit Jupiter: Nos permite escribir los test (API), incluye las anotaciones.
- JUnit Vintage: Permite integrar a la versión 5 pruebas unitarias escritas en la versión 3 o 4.

# Anotaciones comunes en JUnit Jupiter

- `@Test`
- `@DisplayName`
- Nested
- Tag
- ExtendWith
- BeforeEach
- AfterEach
- BeforeAll
- AfterAll
- Disable

