# Soporte, migración y upgrade de ERP open source

## Alcance y estado

Este documento extiende JFXLEGACY2MODERN con una capacidad **propuesta** de modernización ERP. El repositorio incorpora diseño, manifiestos descriptivos y ejemplos; no contiene un ejecutor ERP, conectores funcionales, recetas de upgrade implementadas ni una app Frappe instalada. Los nombres de los módulos describen responsabilidades futuras.

El intervalo 11 a 15 se interpreta de tres maneras explícitas: Odoo Community 11.0 → 15.0, ERPNext 11 → 15 y Odoo Community 11.0 → ERPNext 15. Las cifras de productos distintos no identifican esquemas equivalentes. Son ejemplos históricos para planificar rutas; la selección actual del destino debe revisar mantenimiento, seguridad, módulos y localización.

## Catálogo inicial de perfiles

Son candidatos de integración, no un ranking ni soporte implementado. Cada perfil necesita versión/edición exacta, commit/tag, módulos, contrato de extracción/carga, runtimes y expediente de prueba.

| ERP | Referencia primaria | Alcance propuesto | Estado en este proyecto |
|---|---|---|---|
| Odoo Community | [Repositorio](https://github.com/odoo/odoo), [licencias 15](https://www.odoo.com/documentation/15.0/legal/licenses.html) | Inventario, soporte y upgrade; origen/destino para migración de procesos/datos | Diseño; edición Community explícita |
| ERPNext | [Repositorio](https://github.com/frappe/erpnext) | Inventario, soporte y upgrade; destino inicial para ejemplo Odoo→ERPNext | Diseño; Frappe y apps deben fijarse juntos |
| Flectra | [Repositorio](https://gitlab.com/flectra-hq/flectra), [documentación](https://doc.flectrahq.com/) | Perfil origen/destino a desarrollar | Sin asumir que recetas Odoo sean reutilizables |
| Dolibarr | [Repositorio](https://github.com/Dolibarr/dolibarr) | Perfil origen/destino a desarrollar | API, módulos y esquema por cualificar |
| Tryton | [Proyecto](https://www.tryton.org/) | Perfil origen/destino a desarrollar | Modelo y versiones propios |
| iDempiere | [Proyecto](https://idempiere.org/) | Perfil origen/destino a desarrollar | Plugins y versiones propios |

Odoo Community 15 usa LGPLv3; Enterprise y módulos adicionales tienen condiciones distintas. El inventario debe conservar edición y licencia de cada módulo; no convierte componentes propietarios en open source ni presupone derechos de redistribución.

## Tres servicios distintos

| Servicio | Entrada | Responsabilidad | Evidencia de salida |
|---|---|---|---|
| Soporte operativo | Producto, revisión, módulos y niveles de servicio | Diagnóstico, incidentes, parches evaluados, monitoreo, copias/recuperación, formación y escalado | Runbook, responsables, pruebas de recuperación y acta de transferencia |
| Upgrade dentro del producto | Producto y versión origen/destino | Cambios de esquema, recetas de datos, portado de módulos, apps/dependencias y contratos | Pruebas y conciliación por salto; versiones exactas y resultados |
| Migración entre productos | Dos productos, procesos y datos en alcance | Análisis de brechas, mapeos, reimplementación de extensiones, carga y aceptación de negocio | Matriz de equivalencia, reporte de diferencias y evidencia de aceptación |

No se promete soporte upstream para una versión por el hecho de inventariarla. Cada plan registrará qué se mantiene, quién lo hace, severidad, tiempos acordados, condiciones de escalado y fecha de revisión. El mantenimiento correctivo, las mejoras y el upgrade se aprueban con alcances separados.

## Rutas de referencia 11 → 15

### Odoo Community 11.0 → 15.0

Proponer 11.0 → 12.0 → 13.0 → 14.0 → 15.0 en copias aisladas, verificando por salto cobertura OpenUpgrade, módulos propios y runtimes. [OpenUpgrade](https://oca.github.io/OpenUpgrade/) documenta cobertura por transición; su [introducción](https://oca.github.io/OpenUpgrade/010_introduction.html) diferencia migraciones consecutivas y multiversión. Es una dependencia candidata del worker de upgrade Odoo, no un migrador Odoo→ERPNext. Ninguna cobertura general garantiza las personalizaciones del cliente.

Antes de cada salto: respaldo consistente de base, filestore y configuración; receta fijada; inventario de módulos; entorno compatible; restauración ensayada. Después: arranque, procesos críticos, adjuntos, permisos y conciliación. Se detiene al fallar una puerta; conservar el checkpoint aceptado y evidencia de errores.

### ERPNext 11 → 15

Proponer 11 → 12 → 13 → 14 → 15 como plan de investigación y ensayos; confirmar la viabilidad y receta de cada salto en la documentación y revisiones seleccionadas. No se declara soporte validado de esta cadena. Las migraciones de esquema/patches de Frappe forman parte del mecanismo del producto y no equivalen a una ruta universal entre versiones ([database migrations](https://docs.frappe.io/framework/user/en/database-migrations)). Fijar ERPNext, Frappe, Bench, apps y dependencias por etapa.

Las [guías de ERPNext 14](https://github.com/frappe/erpnext/wiki/Migration-Guide-to-ERPNext-version-14) y [15](https://github.com/frappe/erpnext/wiki/Migration-Guide-to-ERPNext-version-15) describen módulos que pasan a apps separadas. La versión 15 cambia el tratamiento de series/lotes con Serial and Batch Bundle. Revisar también scripts, reportes, formatos de impresión y localización del alcance; no limitar el ensayo a que el servicio arranque.

### Odoo Community 11.0 → ERPNext 15

Esta ruta es una migración entre productos. No exige actualizar antes Odoo a 15: la extracción del origen y el destino elegido se cualifican de forma independiente. Un upgrade previo solo se introduce si una decisión técnica justificada lo necesita.

1. Inventariar procesos, módulos, compañías, impuestos, monedas, cuentas, inventario, operaciones abiertas y adjuntos.
2. Aprobar equivalencias y brechas; seleccionar maestros/saldos de apertura, histórico completo o archivo consultable por entidad. No importar histórico contable como si fuera un saldo de apertura.
3. Extraer con un adaptador Odoo 11 específico hacia staging y modelo de intercambio versionado.
4. Validar identidad, relaciones, duplicados, decimales, unidades y calidad; guardar rechazos con procedencia.
5. Preparar empresas/catálogos destino y cargar según dependencias. [Data Import](https://docs.frappe.io/erpnext/data-import) y la [API Frappe](https://docs.frappe.io/framework/user/en/api/rest) son interfaces candidatas; el conector debe probar campos, permisos y semántica real del destino.
6. Conciliar por empresa/moneda y proceso; probar series/lotes y reportes del destino. Aceptar excepciones explícitas y reconstruir funciones propias en una app destino.
7. Ensayar corte, delta y reversión; aprobar go/no-go y ejecutar con monitoreo. Las nuevas transacciones del destino requieren un plan de conciliación si se revierte; restaurar una copia anterior no las conserva automáticamente.

Ejemplos de mapeo a cualificar:

| Origen Odoo | Destino ERPNext candidato | Decisión pendiente |
|---|---|---|
| res.partner | Customer / Supplier / Contact / Address | Roles, deduplicación y relaciones; no correspondencia uno a uno |
| product.template / product.product | Item / variantes | Unidades, categorías, atributos y claves |
| account.account y moneda/empresa | Account / Company / Currency | Plan contable, grupos y localización |
| stock.location y movimientos | Warehouse y documentos de stock | Jerarquía, valoración, lotes y corte temporal |
| Facturas, pedidos y pagos | Documentos de ventas/compras/pagos | Estados, cancelaciones, operaciones abiertas e histórico |
| ir.attachment y campos propios | Archivos y app/campos destino | Propiedad, permisos, referencias y portado de reglas |

Estos son mapeos conceptuales, no recetas de importación ejecutables. Odoo y ERPNext tienen modelos, ORM y lógica de negocio diferentes; no convertir sus bases con sustituciones de nombres de tablas.

## Módulos y dependencia low-code

- [`erp-modernization`](../modules/erp-modernization/README.md): inventario, planificación, adaptadores, conciliación, corte, soporte y evidencias; independiente de proveedor low-code.
- [`erp-low-code`](../modules/erp-low-code/README.md): interfaz para configurar perfiles, mapeos, formularios y aprobaciones. Declara **Frappe** como dependencia runtime externa obligatoria de su perfil; no de todo el núcleo.

[Frappe](https://github.com/frappe/frappe) se presenta como framework low-code y su [licencia en version-15](https://github.com/frappe/frappe/blob/version-15/LICENSE) es MIT. Se elige como proveedor inicial por su relación con ERPNext. Registrar revisiones inmutables y dependencias transitivas antes de desplegar; una rama no es un pin de release. El manifiesto deja `version_tested` y `resolved_revision` nulos porque aquí no se han realizado pruebas de runtime.

El módulo propondrá DocTypes/configuraciones para Migration Project, ERP Profile, Mapping Revision, Upgrade Step, Evidence Record y Support Case. Exportar configuraciones como app versionada; probar su reconstrucción y upgrade. Permisos, formularios y workflow facilitan la revisión, pero no implementan por sí solos extracción, portado de módulos ni integridad contable.

La UI low-code se conecta al servicio de modernización mediante contratos. Solo el adaptador destino autorizado carga datos por interfaces de negocio; la UI no escribe directamente tablas del ERP. Conservar identidades, aprobación, expiración de acciones y referencias a secretos externas. La IA propone mapeos o reglas; su aplicación requiere validación determinista y aceptación.

## Integración con Rascal MPL

El diseño existente de Rascal analiza AST/hechos y normaliza representaciones con procedencia. La extensión ERP propone frontends para código Python, XML/configuración y metadatos ERP; deben desarrollarse y cualificarse. Los extractores Java M3 o el puente Roslyn no proporcionan automáticamente esa cobertura. Mantener las reglas de transformación de código separadas de mapeos de datos y recetas de upgrade. La IA y Rascal no sustituyen pruebas de negocio, conciliación ni el mecanismo de migración del producto.

## Puertas de aceptación y trazabilidad

Descubrimiento → viabilidad y brechas → ensayo → conciliación/UAT → corte y reversión → soporte. Cada puerta conserva requisito, perfil/revisión, mapeo/receta, entradas/hashes, resultados, excepciones, actor y aprobación. La implementación futura no puede avanzar con un paso fallido o una dependencia crítica sin resolver.

Los parámetros de servicio (RTO/RPO, ventana, volumen, duración, latencia, tolerancias y tiempos de soporte) deben acordarse y registrarse antes del ensayo. Los roles de la matriz deben asignarse a responsables concretos.

| ID | Capacidad | Especificación de alto nivel | Responsable | Criterio de aceptación |
|---|---|---|---|---|
| ERP-01 | Inventario | Registrar producto, edición, versión exacta, módulos, código propio, datos, infraestructura y licencias. | TI | Inventario completo del alcance; exclusiones y dependencias aprobadas. |
| ERP-02 | Ruta y compatibilidad | Distinguir upgrade de producto, migración entre ERP y soporte operativo; fijar pasos y revisiones. | Arquitectura | Cada paso tiene receta, matriz de módulos, runtime y revisión; ninguna ruta se etiqueta validada sin ensayo. |
| ERP-03 | Modelo de intercambio | Versionar entidad, identidad externa, empresa, relaciones, unidades, moneda y procedencia. | Datos | Mapeos explícitos; claves duplicadas y referencias huérfanas detectadas. |
| ERP-04 | Equivalencia de negocio | Mapear procesos, permisos, personalizaciones y localización del origen al destino. | Negocio | Escenarios críticos aceptados; brechas y excepciones documentadas. |
| ERP-05 | Upgrade escalonado | Planificar 11→12→13→14→15 con validación y respaldo por salto. | Migración | Arranque, pruebas críticas y conciliación pasan en cada salto; fallos detienen el avance. |
| ERP-06 | Importación entre productos | Usar adaptadores e interfaces de negocio del ERP destino y orden de dependencias. | Integraciones | Ensayo de carga aceptado; repetición idempotente; errores en cuarentena. |
| ERP-07 | Conciliación | Comparar conteos, saldos, existencias, impuestos, monedas, series/lotes y adjuntos según alcance. | Datos + negocio | Tolerancias acordadas antes del ensayo; diferencias explicadas y aprobadas. |
| ERP-08 | Corte y reversión | Definir congelación, delta final, respaldo consistente, restauración y retorno al origen. | Operaciones | Ensayo de restauración y reversión aceptado; RTO/RPO y ventana aprobados. |
| ERP-09 | Soporte | Definir incidentes, severidad, responsables, monitoreo, recuperación, formación y transferencia. | Soporte | Runbook, escalado y niveles de servicio aprobados; traspaso operativo aceptado. |
| ERP-10 | Versiones y mantenimiento | Registrar mantenimiento upstream y riesgos de cada revisión; revisar runtimes y apps separadas. | TI | Estado revisado antes de decidir destino; 11 y 15 no se presuponen versiones recomendadas. |
| ERP-11 | Seguridad y acceso | Preservar autorización por empresa/rol y referencias de secretos; probar rechazo y auditoría. | Seguridad | Accesos no autorizados rechazados; sin secretos en manifiestos; actor y cambios auditables. |
| ERP-12 | Trazabilidad | Relacionar fuente, requisito, mapeo/receta, caso, evidencia y aprobación. | QA | Todos los requisitos en alcance tienen evidencia o excepción aprobada. |
| LC-01 | Dependencia low-code | El módulo erp-low-code declara Frappe como dependencia runtime del perfil low-code. | Arquitectura | Manifiesto identifica repositorio, licencia, familia de versión y estado no validado. |
| LC-02 | Configuración exportable | Versionar modelos, formularios, flujos, permisos y scripts como app/artefactos exportables. | Low-code | Exportación e importación en sandbox reproducibles con revisión y procedencia. |
| LC-03 | Cambios revisables | Publicar configuraciones y scripts solo después de revisar y probar en destino. | Low-code + QA | Cambios aprobados; reversión de app/configuración ensayada. |
| LC-04 | Compatibilidad de framework | Fijar conjuntamente revisiones Frappe, ERPNext y apps del perfil. | TI | Pines inmutables, runtimes y regresiones validados antes de habilitar el perfil. |
| LC-05 | Frontera de integración | La interfaz low-code usa contratos del servicio; no escribe directamente BD del ERP. | Integraciones | Pruebas de contratos y permisos pasan; importaciones usan adaptador destino. |
| LC-06 | Persistencia de artefactos | Conservar el historial de configuraciones, aprobaciones y evidencias tras el upgrade. | Datos + QA | Artefactos e identidades reconstruibles; cambios incompatibles detectados. |

## Entregables incluidos y trabajo futuro

Incluido: catálogo, especificación de módulos, manifiestos declarativos, planes JSON de referencia, requisitos y diagrama editable. Estado de todos los perfiles/rutas: `proposed`; ningún conector está `validated`.

Pendiente: esquemas de contratos ejecutables, adaptadores, parsers, recetas por módulo, app Frappe, pines y lockfiles de runtime, fixtures, migraciones de prueba y evidencia de aceptación. Los JSON son convenciones descriptivas de este proyecto; no son archivos de instalación de Frappe/Bench, Maven ni pip y no ejecutan una migración.

[Diagrama draw.io](../MBSE/CAS/Drawio/erp-modernization-low-code.drawio) · [Perfiles y rutas](../modules/erp-modernization/profiles.json) · [Manifiesto low-code](../modules/erp-low-code/module.json).
