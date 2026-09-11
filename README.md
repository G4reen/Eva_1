# Eva_1 — Pipeline de Despliegue: ms-vehiculos

Evaluación Parcial 1 — DOY0101 Ingeniería DevOps. Repositorio Git para el microservicio
**ms-vehiculos** (Spring Boot 3.5 / Java 21), preparado como base del pipeline DevOps.



## Estructura del repositorio

```
Eva_1/
├── .github/
│   └── workflows/
│       └── ci.yml          # Pipeline de integración continua (GitHub Actions)
├── ms-vehiculos/           # Código fuente del microservicio (Spring Boot)
│   ├── src/main/java/...   # Controller, service, repository, dto, model
│   ├── src/main/resources/application.yml
│   └── pom.xml
└── README.md
```

## Estrategia de ramificación: GitFlow

Se eligió **GitFlow** por sobre trunk-based development por las siguientes razones:

- El equipo trabaja en parejas con entregas por hitos (evaluaciones parciales), no con
  despliegues continuos varias veces al día, que es el escenario donde trunk-based rinde más.
- GitFlow separa claramente el código estable (`main`) del código en integración (`develop`),
  lo que facilita mostrar trazabilidad y evidencia de cada indicador de logro pedido en la pauta.
- Las ramas `feature/` y `hotfix/` permiten simular de forma explícita un desarrollo colaborativo
  (cambios de feature vs. correcciones urgentes) mediante pull requests, tal como pide el
  enunciado del encargo.
- Al ser un equipo pequeño y un proyecto académico, la sobrecarga de mantener ramas de release
  no es un problema, y en cambio da una estructura clara y fácil de evaluar.

### Ramas del repositorio

| Rama | Propósito |
|---|---|
| `main` | Código estable, listo para desplegar. Solo recibe merges vía Pull Request desde `develop` o `hotfix/*`. |
| `develop` | Rama de integración. Acumula los `feature/*` ya revisados antes de pasar a `main`. |
| `feature/<nombre>` | Una rama por funcionalidad nueva. Nace de `develop` y vuelve a `develop` vía PR. |
| `hotfix/<nombre>` | Corrección urgente sobre `main`. Nace de `main` y se mergea a `main` **y** a `develop`. |

## Convenciones de commits

Se usa el formato [Conventional Commits](https://www.conventionalcommits.org/):

```
<tipo>: <descripción breve en presente>
```

Tipos usados en este proyecto:

- `feat:` nueva funcionalidad (ej. `feat: agrega endpoint para listar vehículos`)
- `fix:` corrección de errores (ej. `fix: corrige timeout de conexión a la BD`)
- `docs:` cambios de documentación (README, wiki, comentarios)
- `ci:` cambios en la configuración de GitHub Actions
- `refactor:` cambios internos que no alteran el comportamiento

## Naming de ramas

- `feature/<nombre-corto-descriptivo>` — ej. `feature/listar-vehiculos`
- `hotfix/<nombre-corto-descriptivo>` — ej. `hotfix/timeout-conexion-bd`
- Nombres en minúsculas, separados por guiones, sin espacios ni tildes.

## Estrategia de revisión (Pull Requests)

1. Toda rama `feature/*` o `hotfix/*` se integra exclusivamente mediante Pull Request, nunca
   con push directo a `develop` o `main`.
2. Cada PR debe ser revisado por el otro integrante de la pareja antes del merge.
3. El PR debe pasar el workflow de GitHub Actions (build exitoso) antes de poder mergearse.
4. Los `hotfix/*` se mergean a `main` y luego se propagan a `develop` para mantener ambas
   ramas sincronizadas.

## CI/CD — GitHub Actions

El workflow (`.github/workflows/ci.yml`) se activa en dos casos:

- **Push a `develop`**: compila el microservicio para validar que la integración de cambios
  no rompe el build.
- **Pull Request hacia `main`**: valida el build antes de permitir el merge a la rama estable.

El job instala JDK 21 (requerido por el `pom.xml`), corre `./mvnw clean install` dentro de
`ms-vehiculos/` y actúa como gate mínimo de integración continua para este entorno cloud
simulado.
## Conclusiones

### Reflexión individual — Bastian

[La verdad es que trabajar con github actions y ocupar el microservicio fue una tarea entretenida más allá de lo que es la materia, me costo un poco el tema de las feature ya que hice commits que quedaron como "a", "arreglo", "cambio 1 o 2", eso queda como enseñanza para más adelante, si pudiera repetir el encargo me gustaria tomarme más tiempo para poder entenderlo al 100% y no al 70%, ya que hubieron cosas con las que realmente me enrede bastante, fallos con temas de versiones de java y demases.]

## Uso de Inteligencia Artificial

Se utilizó IA (Claude, Anthropic) como apoyo para: mejorar la redacción de este README,
revisar la configuración del workflow de GitHub Actions (versión de Java y directorio de
trabajo) y estructurar la documentación de convenciones. Las decisiones técnicas
(elección de GitFlow, estructura de ramas) y las reflexiones del equipo son propias.
