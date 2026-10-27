# Reflexión del equipo — automatización del despliegue

**Proyecto:** FarmToTable · **Integrantes:** completar con los nombres reales del equipo.

La actividad permitió transformar una aplicación organizada en un monorepositorio en nueve repositorios de microservicios y uno central de automatización. Separar carpetas no era suficiente: los servicios dependían de una configuración ESLint externa a su directorio. Para hacerlos autónomos se incorporó esa configuración localmente, conservando Dockerfiles, pruebas, lockfiles y migraciones.

La decisión principal fue definir un workflow reutilizable mediante `workflow_call`. Cada servicio declara su identidad, su ejecutor de pruebas y su destino Railway, mientras `pipelines` concentra los pasos comunes. Esto evita mantener nueve copias de la lógica de instalación, calidad y despliegue. También exige revisar cuidadosamente los cambios del workflow central, porque un error allí puede afectar a todos los servicios.

El pipeline separa `test`, `docker` y `deploy`, conectándolos con `needs`. El control de calidad incluye instalación reproducible, generación de Prisma, comprobación de tipos, lint, compilación y pruebas. Una ejecución manual permite introducir una aserción fallida sin alterar el código de negocio. La evidencia debe mostrar que los trabajos posteriores quedaron omitidos: una falla detectada y contenida no equivale a desplegar una versión defectuosa.

La seguridad se aborda evitando publicar archivos de entorno, respaldos y claves privadas. El token Railway se almacena cifrado en GitHub Actions Secrets y está limitado al proyecto y al entorno producción. Las credenciales de las bases y claves de aplicación permanecen en Railway. Las imágenes se identifican con el SHA del commit y las acciones externas se fijan por SHA para mejorar la trazabilidad. Railway reconstruye el mismo código validado; conservar la imagen GHCR aporta evidencia, aunque esa reconstrucción adicional representa un costo y una oportunidad de optimización.

La verificación no debe limitarse a subir archivos: el workflow espera que Railway alcance un estado final saludable. La migración conserva las bases, direcciones internas y variables existentes y evita un segundo autodespliegue que eluda las pruebas. Los enlaces y capturas adjuntos documentan los resultados realmente obtenidos; no deben presentarse trabajos omitidos como despliegues exitosos.

Como aprendizaje, la automatización hace repetible un proceso que antes dependía de acciones manuales, pero no sustituye las pruebas funcionales ni la revisión de cambios. Como mejoras futuras quedan la aprobación independiente de producción, la protección de ramas y el despliegue directo del artefacto construido. Staging es opcional en la guía y no se utiliza por decisión del propietario.
