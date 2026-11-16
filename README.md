# Día en Bloques con inicio de sesión de Google

Planificador diario para estudiantes: 24 bloques por hora, prioridad de rojo a verde y racha diaria.
Cada estudiante entra con su cuenta de Google y su racha y sus actividades quedan guardadas en su cuenta.

Todo corre en Firebase (plan Spark, gratis): Authentication para Google, Firestore para los datos y Hosting para publicar la página.

## Contenido

- `public/index.html`: la aplicación completa.
- `public/firebase-config.js`: aquí pegas los datos de tu proyecto de Firebase.
- `firestore.rules`: reglas para que cada persona solo vea sus propios datos.
- `firebase.json` y `.firebaserc`: configuración para publicar con la terminal.

## Paso 1. Crear el proyecto en Firebase

1. Entra a https://console.firebase.google.com con tu cuenta de Google.
2. Pulsa **Agregar proyecto**, ponle un nombre (por ejemplo `dia-en-bloques`) y crea el proyecto. Google Analytics es opcional, puedes desactivarlo.
3. Anota el **ID del proyecto** que aparece en Configuración del proyecto. Lo usarás en el paso 5.

## Paso 2. Activar el inicio de sesión con Google

1. En el menú **Compilación**, abre **Authentication** y pulsa **Comenzar**.
2. En **Método de acceso**, elige **Google** y pulsa **Habilitar**.
3. Elige un correo de asistencia del proyecto y guarda.

## Paso 3. Crear la base de datos

1. En **Compilación**, abre **Firestore Database** y pulsa **Crear base de datos**.
2. Elige una ubicación (cualquiera sirve; una cercana a Colombia da menos demora).
3. Elige **modo producción** y crea la base de datos. Las reglas del proyecto se suben en el paso 5.

## Paso 4. Conectar la página con tu proyecto

1. En **Configuración del proyecto**, sección **Tus apps**, pulsa el icono web `</>`.
2. Ponle un nombre a la app y regístrala. No hace falta marcar Hosting aquí.
3. Copia los valores de `firebaseConfig` y pégalos en `public/firebase-config.js`, reemplazando los de ejemplo.

Mientras ese archivo tenga los valores de ejemplo, la página muestra el aviso "Falta conectar Firebase" y solo funciona sin cuenta.

## Paso 5. Probar y publicar

Para probar en tu computador sin publicar:

```
npx serve public
```

Abre http://localhost:3000 y pulsa **Continuar con Google**. `localhost` ya viene autorizado.

Para publicar en internet:

1. Instala Node.js desde https://nodejs.org.
2. En la terminal, dentro de esta carpeta:

```
npm install -g firebase-tools
firebase login
```

3. Abre `.firebaserc` y cambia `TU-PROYECTO-ID` por el ID de tu proyecto.
4. Publica la página y las reglas:

```
firebase deploy
```

5. Tu página queda en `https://TU-PROYECTO-ID.web.app`. Compártela con tus compañeros: cualquiera con una cuenta de Google puede entrar.

Si `firebase deploy` dice que no encuentra el sitio de Hosting, abre **Hosting** en la consola de Firebase, pulsa **Comenzar** y repite el comando.

## Cómo funciona la racha

- Un día cuenta cuando creas una actividad o marcas una como hecha.
- Si registras al día siguiente, la racha sube 1. Si pasa un día completo sin registrar, vuelve a 0 y se conserva la mejor marca.
- El día se mide con la fecha del dispositivo de cada persona.
- Las actividades de ejemplo que trae la página no cuentan.

## Privacidad y seguridad

- Cada estudiante guarda sus datos en `users/{su id}/data/`. Las reglas de `firestore.rules` impiden que otra persona los lea o los cambie.
- Los valores de `firebase-config.js` no son secretos. La protección de los datos son las reglas, así que no las quites.
- Si usas un dominio propio, agrégalo en **Authentication > Configuración > Dominios autorizados**.

## Límites del plan gratuito

El plan Spark permite 50 000 lecturas y 20 000 escrituras al día en Firestore, más que suficiente para un curso. Si algún día lo superas, Firebase te avisa antes de cobrar.

## Qué se probó

El flujo (bienvenida, inicio de sesión, guardado de actividades y racha, cierre de sesión y paso de datos locales a la cuenta) se probó con módulos de Firebase simulados. No se probó contra un proyecto real de Firebase ni con una cuenta real de Google, así que la primera prueba real es tuya en el paso 5.
