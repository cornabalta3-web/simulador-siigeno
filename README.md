# Simulador SIIGENO

Página de prueba del SIIGENO con los dos lados —**Requirentes** y **Escribanía**— para recorrer
copias, segundo testimonio y consulta de antecedentes, y anotar los problemas donde aparecen.
Sigue el recorrido final del armador y la bandeja. **Todos los datos son inventados.**

## Usuarios de prueba (clave `1234`)

| Usuario | Quién | Lado |
|---|---|---|
| `ana` | Ana Gómez | Requirente particular |
| `bustos` | Esc. Estela Bustos | Escribana de registro (puede pedir consulta de antecedentes) |
| `suarez` | G. Suárez | Escribano de la EGG: asigna, reasigna, «No corresponde», usuarios y roles |
| `molina` | A. Molina | Auxiliar: trabaja solo lo que tiene asignado |
| `peralta` | L. Peralta | Auxiliar |

La primera vez la página ofrece «Crear usuarios de prueba».

## Dónde quedan los datos

En **Firebase (Cloud Firestore)**, plan gratuito Spark. Los PDF se guardan dentro de la misma base
(hasta 700 KB cada uno), así que no hace falta Cloud Storage, que pide el plan pago.
Si `config.js` está vacío, cada navegador guarda solo para sí.

### Crear la base (una vez, 10 minutos)

1. Entrar a <https://console.firebase.google.com> con una cuenta de Google y **Crear un proyecto**
   (por ejemplo `simulador-siigeno`). Google Analytics no hace falta.
2. Menú **Compilación → Firestore Database → Crear base de datos**. Ubicación: `southamerica-east1`
   (São Paulo). Empezar en **modo de prueba**.
3. En **Reglas**, pegar esto y **Publicar** (deja leer y escribir hasta fin de año, solo para la prueba):

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if request.time < timestamp.date(2026, 12, 31);
       }
     }
   }
   ```
4. **Configuración del proyecto** (engranaje) → **Tus apps** → ícono `</>` (web) → registrar la app.
   Copiar los valores de `firebaseConfig` en `config.js`.

> Ojo: con estas reglas, cualquiera que tenga el link puede leer y escribir. Sirve para probar con
> datos inventados; **no cargar datos reales de personas**.

## Publicar la página (GitHub Pages)

**Settings → Pages → Build and deployment → Source: Deploy from a branch → `main` / `(root)` → Save.**
En un par de minutos queda en `https://<usuario>.github.io/<repositorio>/`.

## Problemas anotados

El botón amarillo «Anotar problema» guarda la pantalla, el trámite y el estado donde estabas.
En «Problemas anotados» se exportan a un CSV que abre Excel, con las columnas de la planilla de relevamiento.

---
El código fuente vive en el repositorio SIGEC (`simulador/simulador-siigeno.html`); este `index.html` es
una copia lista para publicar.
