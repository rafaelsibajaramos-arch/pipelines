# FarmToTable · pipelines reutilizables

Actividad **Despliegue automatizado con GitHub Actions**. Los nueve repositorios invocan `.github/workflows/microservice.yml` mediante `workflow_call`:

- [producer-service](https://github.com/rafaelsibajaramos-arch/producer-service)
- [catalog-service](https://github.com/rafaelsibajaramos-arch/catalog-service)
- [identity-service](https://github.com/rafaelsibajaramos-arch/identity-service)
- [inventory-service](https://github.com/rafaelsibajaramos-arch/inventory-service)
- [order-service](https://github.com/rafaelsibajaramos-arch/order-service)
- [payment-service](https://github.com/rafaelsibajaramos-arch/payment-service)
- [logistics-service](https://github.com/rafaelsibajaramos-arch/logistics-service)
- [notification-service](https://github.com/rafaelsibajaramos-arch/notification-service)
- [analytics-service](https://github.com/rafaelsibajaramos-arch/analytics-service)

## Flujo

`test → docker → deploy`

1. **test:** higiene de archivos, `npm ci`, Prisma, TypeScript, ESLint, build y pruebas.
2. **docker:** depende de test; construye y publica en GHCR con el SHA completo del commit. Los PR solo construyen, sin publicar.
3. **deploy:** depende de test y docker; solo en main, con `DEPLOY_ENABLED=true`. Envía el mismo commit a Railway y espera su estado saludable. Railway reconstruye el Dockerfile; la imagen GHCR queda como evidencia del job Docker, no se usa como una imagen distinta en producción.

Las acciones externas se fijan por SHA. El workflow propio usa `@main`, como en la actividad, para centralizar actualizaciones. Las ejecuciones de cada servicio se serializan.

## Credenciales y configuración

En cada repositorio, Settings → Secrets and variables → Actions:

- Secret `RAILWAY_TOKEN`: token de proyecto limitado al entorno producción, no token personal de cuenta.
- Variables `RAILWAY_PROJECT_ID`, `RAILWAY_ENVIRONMENT_ID`, `RAILWAY_SERVICE_ID`, `DEPLOY_ENABLED`.
- `GITHUB_TOKEN` publica imágenes del propio repositorio; no se necesita un PAT adicional.
- Los secretos de PostgreSQL, JWT y correo permanecen en Railway; no se entregan a pruebas ni a Docker.
- Railway: raíz `/`, `Dockerfile`, pre-deploy de migraciones y ruta de salud propia de cada servicio. El script de conexión toma esa ruta de su `railway.toml` y la configura por API; no establece una ruta de archivo de configuración antigua en Railway.
- Evitar el autodespliegue paralelo desde `fram`, pues eludiría el gate de Actions.

`fram` se conserva como integración y respaldo. Las bases, volúmenes y variables de aplicación no se trasladan a GitHub. Staging es opcional en la actividad y no se crea, por decisión del propietario.

## Evidencia de falla

Actions → Deploy → Run workflow → main → `failure-demo=true`.

Se ejecutan las pruebas reales y una aserción intencionalmente incorrecta. Test debe fallar; Docker y deploy deben aparecer omitidos. No se modifica código funcional ni se publica una versión defectuosa.

## Entregables

- Repositorios: enlaces anteriores y este repositorio central.
- `EVIDENCIAS.md` y `evidencias/`: resultados y capturas de ejecuciones reales, cuando estén verificadas.
- `INFORME_REFLEXION.md`: completar los integrantes antes de entregar.
- Una ejecución verde solo demuestra despliegue si deploy terminó correctamente, no si fue omitido.

Referencias: [workflows reutilizables](https://docs.github.com/en/actions/how-tos/reuse-automations/reuse-workflows), [Docker en Actions](https://docs.github.com/en/actions/tutorials/publish-packages/publish-docker-images), [Railway CLI](https://docs.railway.com/cli/deploying).
