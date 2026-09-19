---
title: "Ejercicio 1. Control de Versiones con Git y GitHub"
subtitle: "Reporte Técnico y Evidencia de Práctica"
author:
  - "Estrada Sánchez Emiliano"
  - "Espinosa Gómez David Enrique"
  - "Mora Acosta Pablo"
institution: "Instituto Politécnico Nacional - Escuela Superior de Cómputo"
course: "Base de Datos"
group: "3CV1"
date: "Septiembre 2026"
geometry: "margin=2.5cm"
output: pdf_document
---


# INSTITUTO POLITÉCNICO NACIONAL
## ESCUELA SUPERIOR DE CÓMPUTO


### UNIDAD DE APRENDIZAJE: BASE DE DATOS
**GRUPO: 3CV1**


---


## REPORTE DE EVIDENCIA TÉCNICA
# Ejercicio 1. Control de Versiones con Git y GitHub


---


**INTEGRANTES DEL EQUIPO:**
- **Estrada Sánchez Emiliano**
- **Espinosa Gómez David Enrique**
- **Mora Acosta Pablo**


**PROFESOR TITULAR:** Academia de Bases de Datos  
**PERIODO ESCOLAR:** 2026 - 2027 / 1  
**FECHA DE ENTREGA:** 18 de Septiembre de 2026  


---


## Índice General


1. [Parte A: Investigación Conceptual y Fundamentos de Git](#parte-a-investigación-conceptual-y-fundamentos-de-git)
   - 1.1 [Definición de VCS y problemáticas resueltas en el trabajo colaborativo](#11-definición-de-vcs-y-problemáticas-resueltas-en-el-trabajo-colaborativo)
   - 1.2 [Diferencias arquitectónicas y operativas entre Git y GitHub](#12-diferencias-arquitectónicas-y-operativas-entre-git-y-github)
   - 1.3 [Definición técnica de conceptos clave con ejemplos ilustrativos](#13-definición-técnica-de-conceptos-clave-con-ejemplos-ilustrativos)
   - 1.4 [Flujo de trabajo basado en ramas y revisión entre pares (Peer Review)](#14-flujo-de-trabajo-basado-en-ramas-y-revisión-entre-pares-peer-review)
2. [Parte B: Desarrollo Práctico y Evidencias de Ejecución](#parte-b-desarrollo-práctico-y-evidencias-de-ejecución)
   - 2.1 [Configuración inicial y creación del repositorio remoto](#21-configuración-inicial-y-creación-del-repositorio-remoto)
   - 2.2 [Clonación y entorno de trabajo local](#22-clonación-y-entorno-de-trabajo-local)
   - 2.3 [Creación de archivos base: README.md y .gitignore](#23-creación-de-archivos-base-readmemd-y-gitignore)
   - 2.4 [Historial de confirmaciones atómicas (Commits descriptivos)](#24-historial-de-confirmaciones-atómicas-commits-descriptivos)
   - 2.5 [Flujo de ramificación, modificación y Pull Request](#25-flujo-de-ramificación-modificación-y-pull-request)
   - 2.6 [Evidencias formales de entrega](#26-evidencias-formales-de-entrega)
3. [Conclusiones](#conclusiones)
4. [Referencias Bibliográficas](#referencias-bibliográficas)


---


# Parte A: Investigación Conceptual y Fundamentos de Git


## 1.1 Definición de VCS y problemáticas resueltas en el trabajo colaborativo


Un **Sistema de Control de Versiones** (*Version Control System*, VCS) es una infraestructura de software que actúa como una base de datos encargada de rastrear, gestionar y versionar los cambios efectuados sobre una colección de archivos digitales a lo largo del tiempo, almacenando múltiples registros de un solo archivo en momentos específicos. A diferencia de los sistemas tradicionales que registran únicamente diferencias línea por línea (deltas), Git opera mediante una secuencia de copias instantáneas (*snapshots*) inmutables de todo el árbol de archivos, proveyendo un diseño eficiente para manejar proyectos masivos como el kernel de Linux.


Históricamente, la mayoría de los VCS eran **centralizados (CVCS)**. En este esquema, un servidor central almacenaba todo el historial y los desarrolladores se conectaban a él para descargar y registrar cambios. Dicha arquitectura generaba cuellos de botella y riesgos severos de concurrencia al trabajar simultáneamente en las mismas secciones de código. 


Como respuesta técnica surgen los **Sistemas de Control de Versiones Distribuidos (DVCS)**. En un DVCS, cada desarrollador posee una copia local completa de la base de datos del proyecto (el historial íntegro). Esto permite sincronizar mediante parches, trabajar desconectado de la red corporativa y confirmar cambios localmente para integrarlos cuando se disponga de conectividad.


### Problemas concretos que resuelve un DVCS en equipo:


1. **Colisiones y sobreescritura accidental:**  
   Elimina el riesgo crítico de que un ingeniero sobreescriba inadvertidamente las funciones desarrolladas por otro al intercambiar carpetas compartidas o archivos comprimidos (`.zip`), garantizando la integridad colaborativa del código base.
2. **Falta de trazabilidad histórica y auditoría:**  
   Permite determinar con exactitud matemática qué autor modificó una línea específica, en qué fecha, bajo qué contexto y motivado por qué necesidad técnica o corrección de error (`git blame`, `git log`).
3. **Capacidad de reversión (*Rollback*):**  
   Otorga la capacidad de revertir el sistema ante fallos imprevistos en entornos de producción, restituyéndolo a un estado previamente validado y estable sin poner en riesgo la integridad global del proyecto.
4. **Punto único de fallo (*Single Point of Failure* - SPOF):**  
   En los sistemas centralizados, la caída o corrupción del servidor provocaba la pérdida total del historial. En un DVCS como Git, cada clon local es un respaldo redundante íntegro capaz de restaurar el servidor por completo.
5. **Desarrollo lineal restrictivo:**  
   Facilita el desarrollo no lineal mediante ramas paralelas ultraligeras y rápidas, impidiendo que el trabajo concurrente de múltiples ingenieros se bloquee entre sí.


---


## 1.2 Diferencias arquitectónicas y operativas entre Git y GitHub


Aunque con frecuencia se emplean como términos intercambiables, **Git** y **GitHub** operan en capas distintas de la pila tecnológica del desarrollo de software:


* **Git (Software local de control de versiones):**  
  Es un software libre de control de versiones distribuido creado en 2005 por Linus Torvalds y la comunidad de Linux tras perder el acceso a BitKeeper. Diseñado para ofrecer alta velocidad en proyectos masivos y soporte nativo para ramificación no lineal, Git se ejecuta de forma puramente local en el sistema operativo del usuario. Su arquitectura se basa en un **Grafo Acíclico Dirigido (DAG)** que almacena instantáneas inmutables direccionadas mediante funciones hash criptográficas (SHA-1 y SHA-256).
* **GitHub (Plataforma colaborativa en la nube):**  
  Es una plataforma de computación en la nube (adquirida en 2018 por Microsoft) diseñada para alojar repositorios remotos Git. Actúa como capa de gobernanza, colaboración y automatización sobre Git, proporcionando herramientas como seguimiento de incidencias (*Issue Tracking*), revisión formal de código mediante *Pull Requests*, control granular de accesos (RBAC), integración y despliegue continuo (*GitHub Actions*) y alojamiento web estático (*GitHub Pages*).


| Criterio | Git | GitHub |
| :--- | :--- | :--- |
| **Naturaleza** | Software de control de versiones distribuido (motor de bajo nivel). | Plataforma web SaaS y servicio en la nube (gobernanza y colaboración). |
| **Origen y Propiedad** | Creado en 2005 por Linus Torvalds (Código Abierto / Licencia GPLv2). | Fundado en 2008; adquirido en 2018 por Microsoft Corporation. |
| **Entorno de ejecución** | Local en la terminal o cliente del desarrollador. | Servidores remotos e infraestructura distribuida en la nube. |
| **Dependencia de red** | 100% funcional sin conexión a internet (operaciones locales). | Requiere conexión de red para sincronización y colaboración. |
| **Estructura interna** | Grafo Acíclico Dirigido (DAG) con *snapshots* criptográficos (SHA). | Capa de abstracción sobre Git con bases de datos relacionales y API web. |
| **Funcionalidades clave** | `commit`, `branch`, `merge`, `rebase`, `checkout`, `log`. | *Pull Requests*, *Code Review*, *Actions* (CI/CD), *Wiki*, *Issues*, *Projects*. |


---


## 1.3 Definición técnica de conceptos clave con ejemplos ilustrativos


* **Repositorio (*Repository*):**  
  Contenedor o estructura de datos donde reside el árbol de archivos de un proyecto junto con la totalidad de su historial, ramas, etiquetas y metadatos gestionados en el directorio oculto `.git`.  
  * *Ejemplo:* Se inicializa una carpeta local ejecutando `git init` para comenzar el seguimiento de una base de datos de comercio electrónico denominada `tienda-online`.


* **Confirmación (*Commit*):**  
  Instantánea (*snapshot*) inmutable del estado del proyecto en un instante dado del tiempo. Cada confirmación posee un hash criptográfico único, metadatos del autor, marca de tiempo y un mensaje descriptivo del cambio atómico realizado.  
  * *Ejemplo:* Tras implementar la validación de contraseñas en el formulario de registro, se ejecuta:  
    `git commit -m "feat: validar longitud de contraseñas"`


* **Rama (*Branch*):**  
  Puntero móvil liviano hacia un *commit* particular en el historial. Permite bifurcar el flujo principal de trabajo para desarrollar nuevas funcionalidades o corregir errores en completo aislamiento.  
  * *Ejemplo:* En lugar de aplicar cambios experimentales en la rama principal `main`, se crea y activa la rama `login-con-google`.


* **Fusión (*Merge*):**  
  Operación algorítmica que unifica el historial y los cambios contenidos en dos ramas divergentes (por ejemplo, incorporando una rama de características dentro de `main`).  
  * *Ejemplo:* Concluida la característica `login-con-google`, el desarrollador se posiciona en `main` y ejecuta `git merge login-con-google`.


* **Conflicto de fusión (*Merge Conflict*):**  
  Estado de discordancia donde Git es incapaz de conciliar automáticamente las diferencias debido a modificaciones concurrentes sobre las mismas líneas de un archivo o eliminaciones cruzadas. Requiere arbitraje humano manual.  
  * *Ejemplo:* En el archivo `app.js`, la rama `feature` define `const PORT = 3000;` mientras que en `main` se definió `const PORT = 8080;`. Git suspende la fusión hasta que el usuario resuelva la colisión.


* **Pull Request (PR):**  
  Mecanismo colaborativo formal propio de plataformas como GitHub donde un desarrollador solicita la integración de una rama secundaria hacia la rama base, facilitando el debate técnico, pruebas automatizadas y revisión de código (*Peer Review*).  
  * *Ejemplo:* Se sube la rama `login-con-google` al servidor remoto y se abre un Pull Request hacia `main` para que el equipo evalúe la seguridad del código.


* **Archivo `.gitignore`:**  
  Archivo de configuración en texto plano ubicado en la raíz del repositorio que define patrones de búsqueda de archivos y directorios que Git debe omitir intencionalmente del seguimiento.  
  * *Ejemplo:* Reglas para excluir librerías pesadas, archivos de entorno y temporales:
    ```gitignore
    node_modules/
    .env
    build/output/*
    .DS_Store
    Thumbs.db
    ```


* **Archivo `README.md`:**  
  Documento redactado en formato Markdown que actúa como manual técnico principal y carta de presentación de un repositorio, describiendo el propósito, configuración, requisitos y guías de uso.  
  * *Ejemplo:* Archivo que especifica la versión requerida de Node.js, instrucciones de instalación (`npm install`) y ejecución del servidor (`npm run dev`).


---


## 1.4 Flujo de trabajo basado en ramas y revisión entre pares (Peer Review)


Un **Flujo de Trabajo Basado en Ramas** (*Branch-Based Workflow*, alineado con directrices de *GitHub Flow* y *Trunk-Based Development*) se rige bajo la premisa de que la rama troncal (`main`) representa la única fuente de verdad (*single source of truth*), manteniéndose siempre estable, compilable y desplegable. Las modificaciones directas sobre `main` se bloquean mediante reglas de protección de ramas (*branch protection rules*), obligando a que cada incremento funcional se elabore en ramas secundarias efímeras (*short-lived feature branches*) e ingrese a `main` únicamente mediante un Pull Request.


La **Revisión por Pares** (*Peer Code Review*) constituye una compuerta de calidad (*quality gate*) fundamental por tres razones:


1. **Detección temprana de fallas y reducción del costo del cambio:**  
   Bajo el principio *Shift-Left* y la regla económica de Boehm, reparar un defecto lógico, vulnerabilidad de inyección SQL o falla de concurrencia durante la revisión cuesta hasta un orden de magnitud menos que subsanarlo en entornos productivos. El revisor humano complementa el análisis estático automatizado evaluando casos de borde y reglas de negocio.
2. **Mitigación del factor de autobús (*Bus Factor*) y soberanía colectiva:**  
   Evita la creación de silos técnicos donde un solo individuo comprende módulos críticos (como el modelo entidad-relación o la persistencia). La revisión distribuye el conocimiento del sistema de manera homogénea en todo el equipo.
3. **Consistencia técnica y gobernanza del código:**  
   Asegura el cumplimiento riguroso de convenciones de nombrado, arquitectura limpia y restricciones de integridad transaccional (ACID), convirtiendo cada revisión en una instancia continua de mentoría técnica.


---


# Parte B: Desarrollo Práctico y Evidencias de Ejecución


## 2.1 Configuración inicial y creación del repositorio remoto


Para el cumplimiento del ejercicio, se empleó la cuenta en la plataforma GitHub con el identificador de usuario `pablo140706`.


* **Nombre del Repositorio:** `practica1db`
* **Visibilidad:** Pública (*Public*)
* **Descripción asignada:** `Práctica 1: "Modelo Entidad Relación"`
* **Dirección remota oficial (URL):** https://github.com/pablo140706/practica1db


---


## 2.2 Clonación y entorno de trabajo local


Se instaló el software cliente Git en su versión oficial **2.55.0 para Windows** (obtenido del portal oficial https://git-scm.com/). La sincronización inicial del repositorio hacia el entorno de trabajo local se gestionó mediante el asistente gráfico **Git GUI**:


1. Se ejecutó la utilidad **Git GUI** y se seleccionó la opción `Clone Existing Repository`.
2. **Source Location:** `https://github.com/pablo140706/practica1db`
3. **Target Directory:** `C:\Users\P\Documents\escuela\git\1`
4. Se inicializó la copia local completa con verificación de submódulos.


---


## 2.3 Creación de archivos base: README.md y .gitignore


Utilizando el editor **Visual Studio Code** en combinación con el área de preparación (*staging area*) de Git GUI, se redactaron los dos archivos esenciales solicitados:


### A) Archivo `README.md`
Diseñado para identificar la práctica, los participantes y el índice temático:
```markdown
# Practica 1: "Modelo Entidad Relacion"
- Nombre de integrantes: [Espinosa Gómez David Enrique, Estrada Sanchez Emiliano, Mora Acosta Pablo]
- Grupo: [3CV1]
- Carrera: [Ingenieria en Sistemas Computacionales]
```


### B) Archivo `.gitignore`
Configurado para evitar el rastreo accidental de archivos generados por el entorno de desarrollo y el sistema operativo:
```gitignore
# Archivos de configuración de entorno y secretos
.env
# Metadatos del sistema operativo
Thumbs.db
.DS_Store
# Artefactos de compilación
build/
```


---


## 2.4 Historial de confirmaciones atómicas (Commits descriptivos)


Se realizaron las operaciones de preparación de archivos mediante `Rescan` y `Stage Changed` en Git GUI. Se registraron más de cinco confirmaciones atómicas sucesivas con mensajes descriptivos sobre la rama `main`:


| # | Hash SHA | Mensaje de Confirmación | Archivos Involucrados | Justificación Técnica del Cambio |
| :-: | :--- | :--- | :--- | :--- |
| **1** | `eff253b` | `"Aqui ponemos el indice de practica"` | `README.md` | Estructuración del índice y encabezado principal de la práctica. |
| **2** | `5fbacf8` | `"Aqui el nombre de los integrantes"` | `README.md` | Incorporación de los nombres de los miembros del equipo. |
| **3** | `b3e7170` | `"Aqui el grupo"` | `README.md` | Declaración del grupo académico asignado (3CV1). |
| **4** | `be99912` | `"La carrera"` | `README.md` | Especificación de la carrera universitaria (ISC). |
| **5** | `6626de2` | `"Aqui lo que se debe ignorar"` | `.gitignore` | Creación y configuración de reglas en el archivo `.gitignore`. |


Posteriormente, se efectuó la publicación del historial base en el servidor remoto mediante `git push origin main`.


---


## 2.5 Flujo de ramificación, modificación y Pull Request


Para validar el ciclo de vida de desarrollo colaborativo y aislado, se implementó el flujo de ramificación y solicitud de extracción:


1. **Creación de la rama secundaria:** Desde la interfaz de Git GUI se creó la rama `Rama1prueba` a partir del estado de `main`.
2. **Modificación atómica:** En la rama `Rama1prueba`, se editó el archivo `README.md` añadiendo la línea identificadora `+ ."rama1".`.
3. **Commit aislado:** Se registró la confirmación con hash `b7e8068` bajo el mensaje `"pruebaf"`.
4. **Envío al servidor remoto:** Se ejecutó `git push origin Rama1prueba` para publicar la rama en GitHub.
5. **Apertura de Pull Request:** En la interfaz web de GitHub se abrió una solicitud de extracción:
   * **Rama Base:** `main`
   * **Rama Comparada:** `Rama1prueba`
   * **Título del PR:** `"pruebaf"`
   * **Descripción:** `"cambio 1 en github y fusion"`
   * **Inspección de diferencias (*Diff*):** 1 archivo modificado (`README.md`), 2 adiciones y 1 eliminación.
6. **Revisión y Fusión (*Merge*):** Al no presentarse colisiones sintácticas (`Able to merge`), se ejecutó la fusión formal mediante el commit `e5e7873`, cerrando exitosamente el ciclo colaborativo.


---


## 2.6 Evidencias formales de entrega


### Evidencia 1: Enlace oficial al Repositorio Remoto
* **URL:** https://github.com/pablo140706/practica1db
* **Estado:** Público, accesible y verificado.


### Evidencia 2: Registro del árbol de historial (`git log --oneline --graph --all`)
A continuación se presenta la salida obtenida en la terminal Git Bash (MINGW64) que certifica el historial de confirmaciones, las ramas locales y remotas, y la posición de `HEAD`:


```text
@DESKTOP-KE07CDP MINGW64 ~/Documents/escuela/git/1 (ram2)
$ git log --oneline --graph --all
* b7e8068 (origin/Rama1prueba, Rama1prueba) "pruebaf"
* 6626de2 (HEAD -> ram2, origin/main, main) "Aqui lo que se debe ignorar"
* be99912 "La carrera"
* b3e7170 "Aqui el grupo"
* 5fbacf8 "Aqui el nombre de los integrantes"
* eff253b "Aqui ponemos el indice de practica"
```


### Evidencia 3: Registro de Pull Request Fusionado y Cerrado en GitHub
* **Identificador de Fusión:** Merge commit `e5e7873` into `main`.
* **Dictamen de GitHub:**  
  `Pull request successfully merged and closed`  
  *“You’re all set — the branch has been merged.”*
* **Autor de la Fusión:** Usuario `pablo140706`.


### Evidencia 4: Catálogo de Capturas Gráficas del Procedimiento


| No. | Etapa del Proceso | Herramienta | Contenido Visual y Validación |
| :-: | :--- | :--- | :--- |
| **Fig. 1** | Cuenta GitHub | Web Browser | Perfil del usuario `pablo140706` con actividad reciente. |
| **Fig. 2** | Descarga de Git | Web Browser | Descarga del instalador Git v2.55.0 para Windows desde git-scm.com. |
| **Fig. 3** | Creación de Repositorio | GitHub Web | Parámetros del repositorio `practica1db` público con descripción. |
| **Fig. 4** | Clonación Local | Git GUI | Ventana de clonación indicando URL de origen y ruta local `C:\Users\P\Documents\escuela\git\1`. |
| **Fig. 5** | Entorno de Desarrollo | VS Code / Git GUI | Edición simultánea de `README.md` y `.gitignore`. |
| **Fig. 6** | Preparación de Cambios | Git GUI | Ejecución de `Rescan` y visualización de cambios en `Staged Changes`. |
| **Fig. 7** | Registro de Commits | Git GUI | Panel de entrada del mensaje y ejecución del botón `Commit`. |
| **Fig. 8** | Historial de 5 Commits | Git GUI History | Vista gráfica de los 5 commits realizados en la rama `main`. |
| **Fig. 9** | Creación de Rama | Git GUI | Ventana de creación de la rama `Rama1prueba` basada en `main`. |
| **Fig. 10** | Push de Ramas | Git GUI | Envío exitoso de ramas hacia el repositorio remoto (`origin`). |
| **Fig. 11** | Inspección en Terminal | Git Bash | Salida textual de `git log --oneline --graph --all`. |
| **Fig. 12** | Comparación de Diff | GitHub PR | Comparación visual de cambios entre `main` y `Rama1prueba`. |
| **Fig. 13** | Cierre de Pull Request | GitHub PR | Confirmación del Pull Request fusionado y cerrado con commit `e5e7873`. |
| **Fig. 14** | Estado Final de Rama | GitHub Web | Vista principal del repositorio con el `README.md` actualizado. |


---


# Conclusiones


La realización del presente ejercicio permitió consolidar de forma práctica y analítica los conceptos torales de los sistemas de control de versiones distribuidos. Se constató empíricamente la independencia funcional existente entre el motor de control local (**Git**) y la plataforma colaborativa en la nube (**GitHub**).


El flujo de trabajo basado en ramas (*Branch-Based Workflow*) demostró ser un mecanismo indispensable para salvaguardar la integridad de la rama principal `main`, previniendo colisiones de código y garantizando que toda integración sea validada formalmente mediante *Pull Requests* y revisión entre pares (*Peer Code Review*). Finalmente, el uso riguroso de confirmaciones atómicas descriptivas y la exclusión sistemática de archivos mediante `.gitignore` sientan las bases de buenas prácticas de ingeniería requeridas para el desarrollo colaborativo de sistemas de información y bases de datos.


---


# Referencias Bibliográficas


* [1] S. Chacon and B. Straub, *Pro Git*, 2nd ed. New York, NY, USA: Apress, 2014. [Online]. Disponible en: https://git-scm.com/book/es/v2
* [2] GitHub, Inc., "GitHub Docs: About pull requests and branch protection rules", 2026. [Online]. Disponible en: https://docs.github.com/
* [3] Software Freedom Conservancy, "Git Reference Manual: git-log, git-merge, and branching models", 2026. [Online]. Disponible en: https://git-scm.com/doc