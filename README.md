# Eva_1 — ms-vehiculos

Microservicio de vehículos (`cl.matiivilla.vehiculos`), Spring Boot / Java 21 / Maven.
Evaluación Parcial 1 — Ingeniería DevOps (DOY0101).

## 1. Estrategia de ramificación: GitFlow

Se optó por **GitFlow** (en lugar de trunk-based development) por las siguientes razones:

- El equipo es de 2 integrantes y trabaja de forma asincrónica, por lo que separar `main` (código estable) de `develop` (integración) reduce el riesgo de romper una versión desplegable.
- El trabajo incluye tanto **funcionalidades nuevas** (`feature/*`) como **correcciones urgentes** (`hotfix/*`), y GitFlow define un camino explícito para cada caso.
- Es un proyecto con entregas por hitos, no despliegue continuo diario, por lo que un modelo con ramas de mayor duración (GitFlow) se ajusta mejor que trunk-based, que exige alta disciplina de tests automatizados y feature flags que este proyecto aún no tiene.
- Facilita la trazabilidad exigida por el IL1.1: cada rama documenta su propósito (`main` = producción, `develop` = integración, `feature/x` = funcionalidad en curso, `hotfix/x` = corrección urgente).

### Estructura de ramas

| Rama | Propósito |
|---|---|
| `main` | Código estable, listo para producción/despliegue |
| `develop` | Rama de integración de features antes de pasar a `main` |
| `feature/<nombre>` | Nueva funcionalidad, se crea desde `develop` |
| `hotfix/<nombre>` | Corrección urgente, se crea desde `main` |

## 2. Convención de mensajes de commit

Formato [Conventional Commits](https://www.conventionalcommits.org/): `<tipo>: <descripción breve>`

- `feat:` nueva funcionalidad
- `fix:` corrección de errores
- `docs:` cambios de documentación
- `refactor:` cambios internos sin alterar comportamiento
- `test:` agregar o modificar pruebas
- `chore:` tareas de mantenimiento (configuración, dependencias, CI)

Ejemplos:
```
feat: agregar endpoint de búsqueda de vehículos por patente
fix: corregir validación de VIN duplicado
chore: corregir working-directory del workflow de CI
```

## 3. Flujo de merge

1. Toda funcionalidad nueva se desarrolla en `feature/<nombre>` creada desde `develop`.
2. Al finalizar, se abre un **Pull Request** de `feature/<nombre>` hacia `develop`.
3. El otro integrante revisa el PR (mínimo 1 aprobación) antes de mergear.
4. Los hotfixes se crean desde `main` como `hotfix/<nombre>`, con PR directo a `main`; una vez mergeado, se replica el cambio en `develop`.
5. `main` solo recibe merges desde `develop` (release) o desde `hotfix/*`.

## 4. Naming de ramas

- `feature/<nombre-descriptivo-corto>` — ej: `feature/busqueda-por-patente`
- `hotfix/<nombre-descriptivo-corto>` — ej: `hotfix/validacion-vin`
- Minúsculas y guiones, sin espacios ni mayúsculas.

## 5. Estructura de carpetas del proyecto

```
Eva_1/
├── .github/
│   └── workflows/
│       └── ci.yml               # pipeline de integración continua
├── ms-vehiculos/                 # código fuente del microservicio
│   ├── src/
│   │   ├── main/java/cl/matiivilla/vehiculos/
│   │   └── main/resources/
│   ├── mvnw / mvnw.cmd           # Maven Wrapper
│   └── pom.xml
└── README.md
```

## 6. Control de versiones

- Versionado del artefacto según Maven (`<version>` en `pom.xml`, ej. `0.0.1-SNAPSHOT`).
- Tags en Git para marcar versiones estables liberadas a `main` (ej. `v1.0.0`).

## 7. Estrategia de revisión

- Todo cambio ingresa por Pull Request, nunca por push directo a `main` o `develop`.
- El PR debe describir brevemente el cambio y su motivo.
- El revisor verifica que el check de CI (GitHub Actions) haya pasado antes de aprobar.

## 8. Uso de Inteligencia Artificial

Se utilizó IA (Claude) como apoyo para redactar este README y el workflow de GitHub Actions, y para revisar la configuración del pipeline (corrección de working-directory y versión de Java). Las decisiones técnicas (elección de GitFlow, estructura de ramas, convenciones) fueron definidas y validadas por el equipo.