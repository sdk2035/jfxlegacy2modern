# erp-low-code

Especificación de un módulo low-code, con **Frappe como dependencia runtime obligatoria de este perfil**. El módulo y su integración todavía no están implementados; el manifiesto no instala Frappe.

[Manifiesto de dependencia](module.json) · [Requisitos LC-01…06 y arquitectura](../../docs/erp-modernization.md).

Se propone una app Frappe versionada con DocTypes, formularios, permisos y flujos para proyectos de migración, perfiles ERP, revisiones de mapeos, pasos de upgrade, evidencias y casos de soporte. Exportar configuraciones y scripts; revisar diffs; importar en sandbox y ensayar restauración antes de desplegar.

La familia 15 sirve al ejemplo ERPNext 15. Fijar tags/commits inmutables de Frappe, ERPNext y apps por entorno y verificar sus runtimes. `resolved_revision: null` y `version_tested: null` significan pendiente; no compatibilidad certificada. Otra familia requiere un perfil y pruebas propios.

El adaptador low-code llama contratos del servicio erp-modernization para consultar, proponer, revisar y aprobar operaciones acotadas. La UI no ejecuta SQL sobre el ERP ni reemplaza los adaptadores de migración. El ejecutor verifica el alcance aprobado, revisiones, idempotencia, identidad y puertas antes de aplicar cambios.
