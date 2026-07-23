<!-- GENERATED FILE — DO NOT EDIT MANUALLY -->
<!-- Generado por dcos/public-context/generate-chatgpt-context-package.js, regenerado desde -->
<!-- un sourceCommit exacto por dcos/closure/certified-closure-orchestrator.js. -->
<!-- Edita las fuentes oficiales listadas abajo (y, para la narrativa curada, revisa y -->
<!-- aprueba una nueva huella) en vez de editar este archivo. -->

# DCOS Context Package para ChatGPT — RRSS Platform

**Formato:** chatgpt-context-package/v2 · **Rama de origen:** main · **Commit de origen:** `dc98cd9b9cd29ecf8bd11c2f32734d66baa4c1c2`

Este paquete no reemplaza al corpus documental de `docs/` ni es una fuente manual de verdad:
es un artefacto derivado, regenerado desde un `sourceCommit` exacto. No debe editarse
manualmente. Su manifest privado (`chatgpt-context-manifest.json`) nunca se publica.

## Mapa de actualización del paquete

- **Metadatos técnicos** (rama, commit): dinámicos, tomados del `sourceCommit` de generación.
- **project.json**: descriptor DCOS vigente — 35 fuente(s) en
  `sourceRegistry`, 4 política(s) en `policyRegistry` (conteo
  derivado en vivo, no fijado).
- **Secciones narrativas curadas** (identidad y propósito, límites del producto, arquitectura
  técnica, multitenancy y seguridad, reglas operativas, gobierno DCOS y flujo de trabajo,
  restricciones): curadas manualmente, protegidas por huella SHA-256 de las fuentes oficiales
  que las respaldan. Si una fuente cambia, la generación falla (`SINTESIS_CURADA_DESALINEADA`)
  hasta que la síntesis se revise y la huella se apruebe explícitamente.
- **Estado operativo** (incremento vigente, bloques cerrados/pendientes, capacidades,
  merge a main, estado DCOS): derivado automáticamente en cada generación desde
  `current-state.md`/`roadmap.md`/`backlog.md`, nunca literal fijado en este módulo.

**Huellas de síntesis curadas (SHA-256, primeros 12 caracteres):**

| Síntesis | Fuentes | Huella (12 car.) |
|---|---|---|
| `identity-purpose` | `docs/product/product-definition.md` | `0af7fed9680f` |
| `product-limits` | `docs/product/non-goals.md` | `b7ce7e2e3e5f` |
| `technical-architecture` | `docs/architecture/system-architecture.md`, `docs/architecture/technology-stack.md` | `5f65e56423ce` |
| `multitenancy-security` | `docs/architecture/multitenancy-and-isolation.md`, `docs/architecture/identity-and-access.md`, `docs/architecture/security-and-privacy.md` | `d71ff1f1ce6d` |
| `operational-rules` | `docs/operations/roadmap.md`, `docs/operations/backlog.md` | `8660ac715097` |
| `workflow` | `docs/decisions/ADR-001-project-independence.md` | `f3bd14ae040d` |
| `restrictions` | `docs/architecture/technology-stack.md`, `docs/architecture/infrastructure-and-operations.md` | `11f25c740c38` |

**Advertencia de seguridad**: este paquete no contiene, y no debe contener, secretos,
contraseñas, tokens, connection strings, certificados, rutas personales del operador, ni
datos reales de clientes. El generador rechaza la escritura si detecta patrones sospechosos.

---

## Identidad y propósito

RRSS Platform es un SaaS multiorganización para empresas y agencias que permite planear y
administrar campañas en redes sociales, gobernar la producción de contenido, capturar y
calificar leads, darles seguimiento mediante un CRM ligero, y atribuir oportunidades, ventas e
ingresos a las campañas que las generaron. Facebook e Instagram son los primeros canales;
DIGART es el primer entorno real de validación, sin convertirse en una dependencia estructural
del producto.

Propuesta de valor: convertir la actividad de redes sociales en oportunidades comerciales
trazables, calificadas, atendidas y medibles.

Recorrido principal: Campaña → Contenido → Captación → Lead → Calificación → Oportunidad →
Seguimiento → Venta o pérdida → Evaluación y aprendizaje.

Principios rectores clave: la campaña es la unidad central de trazabilidad; el núcleo no
depende de Meta; la captura manual permanece como mecanismo válido y de respaldo; el
aislamiento entre organizaciones es obligatorio; aprobar y publicar son permisos distintos.

## Límites del producto (V1 no será)

RRSS Platform v1 explícitamente no será: un CRM empresarial; un editor multimedia; un
reemplazo total de Meta Business Suite; una plataforma omnicanal; un sistema de facturación;
una solución de inteligencia artificial; un sistema de atribución multicanal avanzada; una
aplicación móvil; ni un administrador completo de Meta Ads.

Capacidades que podrían acercar el producto a alguno de estos no-objetivos (IA generativa,
scoring predictivo, atribución multicanal, CRM empresarial propio, facturación SaaS) están
clasificadas como posteriores o diferidas y requieren autorización explícita antes de evaluarse.

## Arquitectura técnica

RRSS Platform es un monolito modular SaaS: una única base de código desplegada como tres
procesos independientes (rrss-web, rrss-api, rrss-worker) a partir del mismo commit
versionado, sin microservicios ni Kubernetes en esta etapa. Módulos declarados en la
arquitectura vigente: Identity, Organizations, Campaigns, Content, CRM, Analytics,
Integrations, Files, Audit — cada uno propietario exclusivo de sus entidades, organizado en
cuatro capas (API, Application, Domain, Infrastructure). Los módulos se comunican solo
mediante contratos públicos, consultas explícitas o eventos internos; nunca por acceso
directo a tablas de otro módulo.

Pila tecnológica autorizada: C# + .NET 10 LTS + ASP.NET Core; React + TypeScript + Vite;
PostgreSQL 18; Entity Framework Core; ASP.NET Core Identity; almacenamiento de objetos
compatible con S3; contenedores Linux.

Nota de procedencia: el módulo `Brands` (RRSS-I04-A) existe como implementación real (ver
estado operativo derivado más abajo) aunque el listado de módulos de
`docs/architecture/system-architecture.md` aún no lo incluye explícitamente; esta síntesis
refleja fielmente lo que la fuente declara hoy y se actualizará cuando la fuente se actualice.

## Multitenancy y seguridad

La organización es la frontera obligatoria de aislamiento. Estrategia: base de datos y
esquema únicos compartidos, `OrganizationId` obligatorio en toda entidad organizacional,
autorización en servidor, filtros organizacionales en la capa de aplicación, restricciones de
integridad, y Row-Level Security de PostgreSQL como capa adicional de defensa en profundidad
— nunca como único mecanismo. Una solicitud pasa, en orden: autenticación de la persona →
resolución de organización activa y membresía → autorización por rol/permiso → filtro
organizacional explícito → RLS como verificación final.

Identidad, membresía, rol y permiso son conceptos distintos, nunca fusionados; la
autorización real se ejecuta exclusivamente en backend, nunca en el frontend. Sesiones
mediante ASP.NET Core Identity y cookies seguras administradas por el backend; sin tokens
persistentes en `localStorage`.

Principios de seguridad transversales: denegación por defecto, mínimo privilegio, defensa en
profundidad, cifrado en tránsito y en reposo, secretos fuera del código y de los datos
ordinarios de la aplicación, ambientes separados sin compartir datos reales.

## Reglas operativas

El roadmap y el backlog son planeación conceptual, no autorización: ningún incremento,
bloque o criterio de cierre queda autorizado para implementación por el solo hecho de estar
documentado o listado. Avanzar de bloque, cerrar un incremento, o hacer merge a `main`
requiere una decisión y autorización explícitas e independientes en cada caso. Una
definición documental de un bloque tampoco lo inicia ni sustituye su autorización
explícita: define su alcance, pero el bloque permanece no iniciado hasta que exista esa
decisión independiente. El estado individual vigente de cada bloque del incremento actual
se deriva exclusivamente de `docs/operations/current-state.md` (ver "Estado operativo
vigente" más abajo), nunca de este resumen narrativo ni de una enumeración fija aquí.

## Gobierno DCOS y flujo de trabajo

RRSS Platform es un proyecto independiente: no comparte código ni repositorio con DCOS Core
ni con digart.mx, y no depende de ningún otro repositorio para operar. DCOS Core (v1.0.0,
commit `a6a518f1824206e5b22ec780bfda92b9796c63bf`) se invoca únicamente como herramienta
externa de gobierno documental, desde su propio repositorio local, contra el descriptor
propio de RRSS Platform (`project.json`); nunca se copia ni se modifica desde este proyecto.

DIGART es el primer caso de uso y validación del producto, no una dependencia estructural.
El repositorio público de contexto derivado (`rrss-public-context`) se crea en un incremento
posterior a RRSS-I00, mediante autorización explícita, no como parte del corpus documental
base.

## Restricciones técnicas vigentes

Explícitamente no seleccionados en esta etapa: proveedor de nube, proveedor concreto de
almacenamiento compatible con S3, proveedor de correo transaccional, herramienta de
observabilidad, cola externa de mensajería, plataforma CI/CD definitiva, sistema de
facturación.

Los tres procesos se ejecutan en contenedores Linux con artefactos inmutables vinculados a
un commit; las migraciones de base de datos son controladas y explícitas, nunca implícitas
en el arranque productivo; no se adopta Kubernetes en esta etapa; toda configuración sensible
o dependiente de ambiente permanece externa a la imagen del contenedor.

## Estado operativo vigente (derivado)

**Incremento vigente:** RRSS-I04 — Núcleo editorial y planeación social

**Incrementos cerrados:**

- RRSS-I02 (RRSS-I02-A a H)
- RRSS-I03 (RRSS-I03-A a C) — Interfaz base y sistema visual

**Bloques cerrados del incremento vigente:**

- RRSS-I04-B — Campañas (cerrado local y remotamente, commit `21e6faf0cd5a866a7aa295b8768c94ac95f12ee6`, rama `feature/rrss-i04-campaigns`, integrado en main mediante merge `519ab6a0a59470a755883f02a30f0f6d44e03ce6`)
- RRSS-I04-A — Marcas y perfiles sociales internos (cerrado local y remotamente, commit `9421c6be718f1053fcdbedd85a272375968b79bc`)
- RRSS-I03-A — Sistema visual, autenticación y shell base (cerrado local y remotamente, commit `63b1309f3eca4ab4e25048f82160c052501fa44d`)
- RRSS-I03-B — Navegación, estados globales y estructura de aplicación (cerrado local y remotamente, commit `401e452939d797bd12a2cfaead42e3a84b6dbead`)
- RRSS-I03-C — Patrones de datos, formularios y confirmaciones (cerrado local y remotamente, commit `e7597fb66a9d8bc66aee01fd915be3890a0b8e01`)
- RRSS-I04-C — Contenido y calendario editorial (cerrado local y remotamente, rama `feature/rrss-i04-content-calendar`, commit `d354126e5eb7b56dcb0906d9675d77dbf15f8a1b`, integrado en main mediante merge `e77ddea24175b3a38939e8530755029288afa1e8`)

**Siguiente bloque:** RRSS-I04-D (revisión y aprobación), definido documentalmente, no iniciado, pendiente de autorización

### Definición operativa del siguiente bloque

**Identificador:** RRSS-I04-D — Revisión y aprobación

**Estado:** Definido documentalmente. No iniciado. Pendiente de autorización.

**Propósito:**

Incorporar el flujo interno de revisión, observaciones y aprobación de piezas y variantes de contenido (RRSS-I04-C) antes de cualquier publicación futura.

**Alcance incluido:**

Solicitudes de revisión; revisores autorizados; comentarios u observaciones internas; decisiones de aprobación o solicitud de cambios; historial de decisiones; estados de revisión; relación con piezas y variantes existentes; permisos backend; aislamiento por organización; persistencia; RLS; API; interfaz responsive; indicadores de estado de revisión visibles en Content y en el calendario editorial.

**Estados previstos:**

Como referencia mínima: `PendingReview`, `ChangesRequested`, `Approved`, y `Rejected` (o equivalente) únicamente si la documentación vigente al momento de implementar lo respalda. Estados y transiciones válidas quedan por definir en detalle durante la implementación, sin fijarlos aquí como código.

**Entregables:**

Dominio, contratos, aplicación, infraestructura, persistencia, migraciones, RLS, permisos, API, interfaz y documentación operativa, limitados al flujo de revisión y aprobación de piezas/variantes.

**Fuera de alcance:**

Publicación real, Meta, programación automática, archivos creativos, notificaciones externas, CRM, Analytics, inteligencia artificial, automatizaciones, y aprobación de campañas completas (salvo que ya esté explícitamente aprobada en una definición posterior).

**Dependencias:**

RRSS-I04-C cerrado e integrado; Content y calendario editorial operativos; organizaciones, permisos y multitenancy operativos.

**Criterios de entrada:**

Definición documental aprobada; autorización explícita del usuario; `main` limpio y sincronizado; RRSS-I04-C integrado en `main`.

**Criterio de cierre futuro:**

El flujo de revisión y aprobación deberá estar implementado de extremo a extremo, con historial de decisiones, permisos backend, aislamiento organizacional, API e interfaz, sin incluir publicación real.

RRSS-I04: continúa abierto (bloque A cerrado e integrado; bloque B cerrado e integrado; bloque C cerrado en la rama `feature/rrss-i04-content-calendar`, pendiente de integración a main; bloque D definido documentalmente, no iniciado, pendiente de autorización). Merge de RRSS-I04-C a main: pendiente de decisión independiente.

Herramienta de gobierno (fuera de los bloques de RRSS-I04): generador determinista de
DCOS Context Package para ChatGPT implementado en `dcos/public-context/` (ver
`docs/operations/current-state.md`). Artefacto derivado de la documentación oficial,
no autoriza ni cierra ningún bloque por sí mismo.

Repositorio público `lalolona/rrss-public-context` creado, publicando únicamente
`chatgpt-context-package.md`, actualizable de forma controlada mediante
`.github/workflows/publish-chatgpt-context.yml` (`workflow_dispatch` + `source_commit`
explícito). El mecanismo regenera el paquete y su manifest privado desde el árbol Git
exacto de `source_commit` (síntesis narrativa curada protegida por huella SHA-256 +
estado operativo derivado automáticamente); no copia un Markdown ya comprometido. El
manifest permanece siempre privado. Ver `docs/operations/current-state.md`. No forma
parte de los bloques de RRSS-I04 y no autoriza RRSS-I04-B.

Primera publicación certificada completada (`source_commit`
`33db63afb7487dcf5947e1a617a97d11a4a60dfc`, commit público
`de2873f62ccb65bbef56e1620e2de2ca8692a66d`); ver `docs/operations/current-state.md`.
Sigue sin formar parte de los bloques de RRSS-I04 y sin autorizar RRSS-I04-B; merge de
`feature/rrss-i04-brands` a `main` sigue pendiente de decisión independiente.

**Autorización requerida:** Pendiente de autorización.

**Estado individual de los bloques del incremento vigente (derivado, uno por bloque):**

- RRSS-I04-A: cerrado
- RRSS-I04-B: cerrado
- RRSS-I04-C: cerrado local y remotamente, integrado en main mediante merge `e77ddea24175b3a38939e8530755029288afa1e8`
- RRSS-I04-D: no iniciado, definido documentalmente (ver `docs/operations/roadmap.md`), pendiente de autorización

**Merge a main:** Merge de RRSS-I04-C a main: completado (commit de main `e77ddea24175b3a38939e8530755029288afa1e8`, integrado local y remotamente)

**Estado DCOS:** válido

**Capacidades implementadas (derivado, líneas ": operativo/a(s)" del estado vigente):**

- Persistencia: operativa
- Identity y sesiones: operativas
- Organizaciones y autorización: operativas
- Multitenancy y RLS: operativos
- Infraestructura transversal: operativa
- Sistema visual base: operativo (tokens, estilos globales, componentes reutilizables)
- Autenticación visual: operativa (Login y Registro rediseñados)
- Shell responsive: operativo
- Estados globales: operativos (404, acceso denegado, sesión expirada, error inesperado)
- Error Boundary: operativo
- Notificaciones globales: operativas (organización creada, cambio de organización activa, logout)
- Patrones de datos: operativos (tablas y listas responsivas, filtros, búsqueda, paginación, selección y acciones masivas/contextuales)
- Formularios reutilizables: operativos (Form, FormSection, FieldGroup, CheckboxField, SelectField, TextAreaField, FormActions, ValidationSummary)
- Diálogos y confirmaciones: operativos (Modal con focus trap, Dialog, ConfirmDialog)
- Empaquetado productivo base: operativo
- Migrator: operativo
- Campaigns (RRSS-I04-B): operativo (núcleo de campañas: creación, edición condicionada por estado, transición de estados Draft/Planned/Active/Paused/Finalized/Cancelled, asociación con marca y perfiles sociales activos, cancelación; sin contenido, calendario editorial, revisión/aprobación ni publicación real — reservado para RRSS-I04-C/D)
- Content y calendario editorial (RRSS-I04-C): operativo (piezas maestras, variantes por perfil social, calendario editorial interno, integración con Campaigns y Brands mediante contratos públicos; sin revisión, aprobación, publicación real ni integración con Meta — reservado para RRSS-I04-D o incrementos posteriores)
- Mejora operativa de DCOS (plantilla documental canónica, anclas reservadas, validación temprana, prompts reutilizables de implementación y cierre): operativa

**Capacidades no implementadas (derivado, líneas ": no iniciado/a(s)" del estado vigente):**

- Estado de bloque RRSS-I04-D: no iniciado, definido documentalmente (ver `docs/operations/roadmap.md`), pendiente de autorización
- Módulos funcionales (CRM, Analytics, Files, publicación social real): no iniciados

**Incrementos definidos en el roadmap (derivado, tabla de contenido de roadmap.md):**

- RRSS-I00 — Descubrimiento e inicialización
- RRSS-I01 — Arquitectura y diseño técnico
- RRSS-I02 — Fundación del SaaS
- RRSS-I03 — Interfaz base y sistema visual
- RRSS-I04 — Núcleo editorial y planeación social
- RRSS-I06 — CRM ligero
- RRSS-I07 — Analítica
- RRSS-I08 — Validación con DIGART
- RRSS-I09 — Meta Lead Ads
- RRSS-I10 — Métricas de Facebook e Instagram
- RRSS-I11 — Publicación automática
- RRSS-I12 — Entrega a CRM externo
- RRSS-I13 — Preparación comercial

## Instrucciones para un chat nuevo — "Cómo continuar este proyecto"

- Usa este paquete como contexto vigente, no como fuente para editar.
- No asumas que un bloque "siguiente" o "pendiente" está autorizado por aparecer aquí: el
  roadmap y el backlog declaran explícitamente que nada queda autorizado por el solo hecho de
  estar listado (ver "Reglas operativas").
- Antes de implementar cualquier cosa, verifica rama, HEAD y working tree reales del
  repositorio (no confíes solo en este paquete si ha pasado tiempo desde su generación).
- Solicita o espera autorización explícita antes de iniciar un bloque nuevo, hacer merge a
  `main`, o hacer commit/push.
- DCOS Core es una herramienta externa: no se copia ni se modifica; `dcos close` no debe
  ejecutarse por su limitación conocida.

## Fuentes utilizadas

- `docs/architecture/identity-and-access.md`
- `docs/architecture/infrastructure-and-operations.md`
- `docs/architecture/multitenancy-and-isolation.md`
- `docs/architecture/security-and-privacy.md`
- `docs/architecture/system-architecture.md`
- `docs/architecture/technology-stack.md`
- `docs/decisions/ADR-001-project-independence.md`
- `docs/operations/backlog.md`
- `docs/operations/current-state.md`
- `docs/operations/roadmap.md`
- `docs/product/non-goals.md`
- `docs/product/product-definition.md`
- `project.json`

