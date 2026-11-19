# Día en Bloques con inicio de sesión de Google

Planificador diario para estudiantes: 24 bloques por hora, prioridad de rojo a verde y racha diaria.
Cada estudiante entra con su cuenta de Google y su racha y sus actividades quedan guardadas en su cuenta.

Todo corre en Firebase (plan Spark, gratis): Authentication para Google, Firestore para los datos y Hosting para publicar la página.

## Contenido

- `public/index.html`: la aplicación completa.
- `public/firebase-config.js`: aquí pegas los datos de tu proyecto de Firebase.
- `firestore.rules`: reglas para que cada persona solo vea sus propios datos.
- `firebase.json` y `.firebaserc`: configuración para publicar con la terminal.

La página queda en `https://dia-en-bloques.web.app`. Cualquiera con una cuenta de Google puede entrar.


## Cómo funciona la racha

- Un día cuenta cuando creas una actividad o marcas una como hecha.
- Si registras al día siguiente, la racha sube 1. Si pasa un día completo sin registrar, vuelve a 0 y se conserva la mejor marca.
- El día se mide con la fecha del dispositivo de cada persona.
- Las actividades de ejemplo que trae la página no cuentan.

## Organizar mi día

En el panel "Por organizar" agregas actividades con duración, prioridad, tipo y fecha límite opcional (sin hora). El botón "Organizar mi día" las reparte en las horas libres del día que estás viendo, entre las 06:00 y las 22:00:

- Primero van las de mayor prioridad. Una fecha límite cercana (hoy o en 2 días) sube la prioridad.
- Con la misma prioridad se alternan académicas y personales.
- Después de 3 horas seguidas deja una hora libre de descanso.
- Si una actividad no cabe de una vez, la parte en bloques de hasta 2 horas. Lo que no cabe queda en la lista para otro día.
- Si es hoy, solo usa horas que aún no han pasado.
- Organizar no suma racha; la racha sube cuando creas o completas una actividad.

La lista se guarda junto a las actividades en `users/{id}/data/planner` (campo `pending`), por eso `firestore.rules` permite ese campo. Después de actualizar el repo hay que publicar las reglas: `firebase deploy --only firestore:rules`.

## Privacidad y seguridad

- Cada estudiante guarda sus datos en `users/{su id}/data/`. Las reglas de `firestore.rules` impiden que otra persona los lea o los cambie.
- Los valores de `firebase-config.js` no son secretos. La protección de los datos son las reglas, así que no las quites.
- Si usas un dominio propio, agrégalo en **Authentication > Configuración > Dominios autorizados**.

## Límites del plan gratuito

El plan Spark permite 50 000 lecturas y 20 000 escrituras al día en Firestore, más que suficiente para un curso. Si algún día lo superas, Firebase te avisa antes de cobrar.

## Qué se probó

El flujo (bienvenida, inicio de sesión, guardado de actividades y racha, cierre de sesión y paso de datos locales a la cuenta) se probó con módulos de Firebase simulados. No se probó contra un proyecto real de Firebase ni con una cuenta real de Google, así que la primera prueba real es tuya en el paso 5.
