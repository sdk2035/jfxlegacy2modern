# erp-modernization

Módulo propuesto de soporte, upgrade dentro de un producto y migración entre ERP. Esta carpeta contiene especificaciones declarativas, no un paquete ejecutable.

- [Alcance, procesos y requisitos ERP-01…12](../../docs/erp-modernization.md).
- [Manifiesto](module.json): capacidad y dependencia OpenUpgrade del perfil de worker Odoo.
- [Perfiles y rutas](profiles.json): seis productos candidatos y tres rutas de ejemplo 11→15.

El servicio futuro mantendrá inventario, plan por versión, mapeos, staging, lotes/checkpoints, conciliación, evidencia y runbooks. Extractores y cargadores son adaptadores versionados; se necesitan pruebas por producto. Los planes describen requisitos y puertas; no pueden ejecutarse con revisiones nulas o cobertura pendiente.

`erp-low-code` es una interfaz opcional. El dominio y la orquestación no dependen de Frappe. El usuario de operación define empresa/sitio, alcance, tolerancias, permisos y ventana; una aprobación de plan no autoriza cambios ilimitados posteriores.
