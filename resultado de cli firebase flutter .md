Para configurar un entorno profesional de desarrollo con **Flutter** y **Firebase**, es fundamental dominar el uso de la terminal y las herramientas de Node.js. A continuación, presento la guía técnica paso a paso para preparar tu estación de trabajo en Windows.

---

## 1. Software Necesario: Node.js y NPM
Para utilizar el **Firebase CLI**, necesitas **Node.js**, que incluye automáticamente **NPM** (Node Package Manager).

### Cómo verificar si ya están instalados
Abre una terminal (PowerShell o CMD) y escribe:
```bash
node -v
npm -v
```
* **Si aparece una versión (ej. v20.11.0):** Ya puedes saltar al paso de instalación global.
* **Si dice "comando no reconocido":** Debes seguir los pasos de instalación.

### Instalación paso a paso en Windows
1.  **Descarga:** Ve al sitio oficial [nodejs.org](https://nodejs.org/) y descarga la versión **LTS** (Long Term Support). Es la más estable para desarrollo.
2.  **Ejecución:** Abre el instalador `.msi`.
3.  **Configuración Clave:** Durante la instalación, asegúrate de que la opción **"Add to PATH"** esté seleccionada (esto permite que Windows reconozca los comandos en cualquier carpeta).
4.  **Finalización:** Haz clic en "Next" hasta terminar. Reinicia tu terminal para que los cambios surtan efecto.

---

## 2. Instalación Global de Firebase CLI
Una vez que tienes NPM, instalamos las herramientas de Firebase para que estén disponibles en todo el sistema.

### Comando de instalación global:
```bash
npm install -g firebase-tools
```
* El flag `-g` significa **Global**, permitiéndote usar el comando `firebase` desde cualquier proyecto o carpeta.

---

## 3. Comandos Esenciales de Firebase

### Cómo acceder a Firebase con tu Cuenta de Google
Para vincular tu terminal con tu consola de Firebase, usa el comando de autenticación:
```bash
firebase login
```
* **Procedimiento:** Se abrirá una ventana en tu navegador predeterminado. Selecciona tu cuenta de Google, otorga los permisos y regresa a la terminal. Verás un mensaje de éxito.

### Cómo usar `firebase-tools` (Comandos más comunes)
| Comando | Propósito |
| :--- | :--- |
| `firebase projects:list` | Muestra todos tus proyectos creados en la consola de Firebase. |
| `firebase init` | Inicia el asistente para configurar servicios (Firestore, Hosting, etc.) en tu carpeta local. |
| `firebase deploy` | Sube tus reglas de seguridad o funciones a la nube. |
| `firebase logout` | Cierra la sesión actual en la terminal. |

---

## 4. Instalación de Firebase CLI para Flutter (FlutterFire)
En el ecosistema moderno de Flutter, ya no se configuran los archivos manualmente. Usamos la herramienta oficial **FlutterFire CLI**.

### Paso 1: Instalar el activador de Dart
En tu terminal ejecuta:
```bash
dart pub global activate flutterfire_cli
```

### Paso 2: Configurar el proyecto
Dentro de la carpeta de tu aplicación Flutter (`xflutterdiaz0562/crudburgerking`), ejecuta:
```bash
flutterfire configure
```
* **¿Qué hace esto?** Te permite seleccionar tu proyecto de Firebase, elige las plataformas (Android, iOS, Web) y genera automáticamente el archivo `firebase_options.dart` con todas las llaves necesarias.

---

## 5. Resumen del Flujo de Trabajo
1.  **Instalar Node.js/NPM** (Motor de herramientas).
2.  **Instalar firebase-tools** vía NPM (Puente con la nube).
3.  **Firebase Login** (Identidad).
4.  **FlutterFire Configure** (Vinculación automática con el código Dart).

> **Tip de Arquitecto:** Si planeas trabajar con funciones en la nube (Cloud Functions), asegúrate de tener instalada una versión de Node.js compatible con el entorno de ejecución que elijas en Firebase (normalmente Node 18 o 20).
