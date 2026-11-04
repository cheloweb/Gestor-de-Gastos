# Gastos compartidos en Firebase

Página de gastos mensuales para dos personas. Todo corre en Firebase: la página (Hosting), los datos (Firestore) y el acceso con Google (Authentication).

Contenido de esta carpeta:

- `public/index.html`: la página.
- `firestore.rules`: reglas de seguridad. Solo los correos que pongas pueden leer y escribir.
- `firebase.json`: configuración de despliegue.
- `gastos-export.json`: tus 9 gastos actuales, para importarlos una vez. No está dentro de `public`, así que no se publica.

## Antes de empezar

Necesitas una cuenta de Google y Node.js instalado (para el comando `firebase`). El plan gratuito de Firebase (Spark) alcanza de sobra para dos personas. Revisa los límites vigentes en firebase.google.com/pricing.

## Pasos

1. **Crear el proyecto.** Entra en https://console.firebase.google.com, pulsa "Crear un proyecto", ponle un nombre (por ejemplo `gastos-casa`) y desactiva Google Analytics.

2. **Activar el acceso con Google.** En el menú Compilación > Authentication > "Comenzar" > pestaña "Método de acceso" > Google > Habilitar. Elige tu correo como correo de asistencia y guarda.

3. **Crear la base de datos.** Compilación > Firestore Database > "Crear base de datos". Elige una región de Europa (por ejemplo `eur3`) y el modo de producción.

4. **Copiar la configuración web.** Engranaje junto a "Descripción general" > Configuración del proyecto > "Tus apps" > icono web `</>` > registra la app (sin marcar Hosting). Copia el bloque `firebaseConfig` y pégalo en `public/index.html`, donde dice `PEGA AQUÍ TU CONFIGURACIÓN DE FIREBASE`. La `apiKey` de Firebase no es un secreto: quien protege los datos son las reglas del paso siguiente.

5. **Poner los dos correos en las reglas.** Abre `firestore.rules` y cambia `TU_CORREO@gmail.com` y `CORREO_DE_LA_OTRA_PERSONA@gmail.com` por las cuentas de Google que van a usar la página. Cualquier otra cuenta quedará fuera.

6. **Desplegar.** En una terminal, dentro de esta carpeta:

   ```
   npm install -g firebase-tools
   firebase login
   firebase use --add        # elige el proyecto que creaste
   firebase deploy
   ```

   Al terminar, el comando muestra la dirección de la página, del tipo `https://TU_PROYECTO.web.app`.

7. **Importar tus gastos.** Abre esa dirección, entra con Google, despliega "Copia de seguridad" al final de la página y pulsa "Importar JSON" con `gastos-export.json`. Los gastos quedan a nombre de la cuenta con la que importas. Si importas el mismo archivo dos veces no se duplican, pero se pisan las ediciones hechas después.

8. **Compartir.** Envía la dirección a la otra persona. Debe entrar con el correo que pusiste en las reglas.

## Si algo falla

- **"Este dominio no está autorizado"**: en Authentication > Configuración > Dominios autorizados, añade el dominio desde el que abres la página. Los dominios `.web.app` y `.firebaseapp.com` del proyecto ya vienen autorizados.
- **"La cuenta ... no tiene acceso"**: el correo no está en `firestore.rules`, o no has vuelto a ejecutar `firebase deploy` después de editarlo.
- **La página pide configuración**: falta pegar el `firebaseConfig` del paso 4.
- **Cambiar quién tiene acceso más adelante**: edita los correos en `firestore.rules` y ejecuta `firebase deploy --only firestore:rules`.

## Copias de seguridad

"Exportar JSON" (en "Copia de seguridad") descarga todos los gastos. Guarda una copia de vez en cuando.
