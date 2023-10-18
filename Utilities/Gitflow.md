---
tags: utilities
---
----

[Flujo de trabajo de Gitflow | Atlassian Git Tutorial](https://www.atlassian.com/es/git/tutorials/comparing-workflows/gitflow-workflow)

----

# ¿Qué es Gitflow? 

[Gitflow](https://www.atlassian.com/es/git/tutorials/comparing-workflows/gitflow-workflow) es un flujo de trabajo basado en [[Git]], este tiene como objetivo tener una mejor gestión del repositorio evitando conflictos en los pull request.  

Este flujo de trabajo es ideal para metodologías ágiles ya que es un modelo estricto de ramificación, esto quiere decir que cada rama posee una nomenclatura a manera de prefijo, los más comunes son:
```sh
feature/ -> Para nuevas funcionalidades o modificaciones

bug/ -> Para la corrección de errores (pueden ser a nivel producción o desarrollo)

support/ -> Para la parte de soporte

release/ -> Para las pruebas pre-lanzamiento (para corregir errores y posteriormente lanzarse a producción)

hotfix/ -> Para parches en producción (en caliente)
```

Cada una de estas ramas debe de salir de alguna de las principales, buscando así estar integrada a otras ramas.

Generalmente las ramas principales son 2: `main` y  `develop`

![[gitflow-diagram.png]]

Ejemplo de una tabla de cambios:

| ID | Descripción corta | Rama de trabajo | Rama de origen | Rama de destino |
| --------- | --------------------------------------------- | ------------------------------ | -------------- | --------------- |
| Cambio #1 | Implementar inicio de sesión con Facebook | feature/login-con-facebook | develop | develop |
| Cambio #2 | Exportar reporte de usuarios a Google Drive | feature/exportar-reporte-drive | develop | develop |
| Cambio #3 | Error al iniciar sesión con Linkedin (v1.0.0) | hotfix/login-linkedin | main | main y develop |
| Cambio #4 | Liberar versión v1.2.0 | release/v1.2.0 | develop | main y develop |

