# Fallo controlado y despliegue posterior

En los nueve servicios: la aserción de demostración falló y bloqueó Docker y deploy. Después se ejecutaron pruebas, Docker y despliegue a producción correctamente.

| Microservicio | Intento fallido | Ejecución posterior exitosa |
| --- | --- | --- |
| producer-service | [Fallo controlado](https://github.com/rafaelsibajaramos-arch/producer-service/actions/runs/40607019309) | [Deploy correcto](https://github.com/rafaelsibajaramos-arch/producer-service/actions/runs/40607174061) |
| catalog-service | [Fallo controlado](https://github.com/rafaelsibajaramos-arch/catalog-service/actions/runs/40607023328) | [Deploy correcto](https://github.com/rafaelsibajaramos-arch/catalog-service/actions/runs/40607177361) |
| identity-service | [Fallo controlado](https://github.com/rafaelsibajaramos-arch/identity-service/actions/runs/40607026999) | [Deploy correcto](https://github.com/rafaelsibajaramos-arch/identity-service/actions/runs/40607180511) |
| inventory-service | [Fallo controlado](https://github.com/rafaelsibajaramos-arch/inventory-service/actions/runs/40607030717) | [Deploy correcto](https://github.com/rafaelsibajaramos-arch/inventory-service/actions/runs/40607183696) |
| order-service | [Fallo controlado](https://github.com/rafaelsibajaramos-arch/order-service/actions/runs/40607034336) | [Deploy correcto](https://github.com/rafaelsibajaramos-arch/order-service/actions/runs/40607187138) |
| payment-service | [Fallo controlado](https://github.com/rafaelsibajaramos-arch/payment-service/actions/runs/40607037408) | [Deploy correcto](https://github.com/rafaelsibajaramos-arch/payment-service/actions/runs/40607190400) |
| logistics-service | [Fallo controlado](https://github.com/rafaelsibajaramos-arch/logistics-service/actions/runs/40607040681) | [Deploy correcto](https://github.com/rafaelsibajaramos-arch/logistics-service/actions/runs/40607193880) |
| notification-service | [Fallo controlado](https://github.com/rafaelsibajaramos-arch/notification-service/actions/runs/40607044226) | [Deploy correcto](https://github.com/rafaelsibajaramos-arch/notification-service/actions/runs/40607197251) |
| analytics-service | [Fallo controlado](https://github.com/rafaelsibajaramos-arch/analytics-service/actions/runs/40607047625) | [Deploy correcto](https://github.com/rafaelsibajaramos-arch/analytics-service/actions/runs/40607200762) |

Las capturas están en `evidencias/<microservicio>-failure.png` y `evidencias/<microservicio>-success.png`. Los intentos fallidos no desplegaron cambios.
