¡Excelente elección! Integrar Flutter con Firebase es una de las combinaciones más potentes para el desarrollo móvil actual. Aquí tienes la guía detallada y estructurada para configurar tu entorno en Windows.

---

## 1. Requisitos previos: Node.js y npm
Para usar las herramientas de Firebase (CLI), necesitas **Node.js**, que incluye automáticamente el gestor de paquetes **npm**.

### Software necesario
* **Instalador de Node.js:** Descarga la versión **LTS** (Long Term Support) desde [nodejs.org](https://nodejs.org/). Es la más estable para desarrollo.
* **Permisos de Administrador:** Los necesitarás para la instalación global.

### Verificación de instalación
Abre una terminal (PowerShell o CMD) y escribe los siguientes comandos:

* Para Node.js: `node -v`
* Para npm: `npm -v`

> Si aparece una versión (ej. `v20.10.0`), ya lo tienes. Si recibes un error de "comando no reconocido", sigue los pasos de abajo.

---

## 2. Instalación de Node.js y npm paso a paso
Si no los tienes instalados, sigue este procedimiento:

1.  **Descarga:** Ve a [nodejs.org](https://nodejs.org/) y descarga el instalador `.msi` para Windows (LTS).
2.  **Ejecución:** Abre el archivo y sigue el asistente.
3.  **Configuración Crítica:** Durante la instalación, asegúrate de que la opción **"Add to PATH"** esté seleccionada (esto permite que los comandos funcionen de forma global).
4.  **Herramientas adicionales:** Si el instalador pregunta por "Tools for Native Modules", puedes marcarlo (instalará Python y Visual Studio Build Tools), es útil pero no estrictamente obligatorio solo para Firebase.
5.  **Finalizar:** Reinicia tu terminal o tu PC para que los cambios en el "PATH" surtan efecto.

---

## 3. Instalación de Firebase CLI (firebase-tools)
Una vez que `npm` funciona, instalaremos las herramientas de Firebase de manera **global**. Esto significa que podrás usar el comando `firebase` en cualquier carpeta de tu computadora.

### Comando de instalación global
Ejecuta el siguiente comando en tu terminal:
```bash
npm install -g firebase-tools
```
* `-g`: Indica que la instalación es **global**.
* `firebase-tools`: Es el nombre del paquete oficial.

**Verificación:**
```bash
firebase --version
```

---

## 4. Uso de Firebase y Flutter
Para que tu aplicación Flutter se comunique con Firebase, el ecosistema moderno utiliza **FlutterFire CLI**.

### Acceder a Firebase con tu cuenta de Google
Antes de configurar el proyecto, debes "loguearte":
1.  En la terminal escribe: `firebase login`
2.  Se abrirá una ventana en tu navegador. Selecciona tu cuenta de Google vinculada a tu consola de Firebase.
3.  Acepta los permisos. Al terminar, verás un mensaje de éxito en la consola.

### Cómo usar firebase-tools en Flutter
Para vincular tu App de Flutter de forma profesional, te recomiendo usar el flujo de **FlutterFire**:

1.  **Activar el CLI de FlutterFire:**
    ```bash
    dart pub global activate flutterfire_cli
    ```
2.  **Configurar el proyecto:**
    Navega hasta la carpeta raíz de tu proyecto Flutter y ejecuta:
    ```bash
    flutterfire configure
    ```
    Este comando te permitirá seleccionar tu proyecto de Firebase y generará automáticamente el archivo `firebase_options.dart`, configurando Android, iOS y Web al mismo tiempo.

---

### Resumen de comandos útiles
| Tarea | Comando |
| :--- | :--- |
| **Login** | `firebase login` |
| **Cerrar Sesión** | `firebase logout` |
| **Listar Proyectos** | `firebase projects:list` |
| **Inicializar Proyecto** | `firebase init` |
| **Desplegar (Hosting/Rules)** | `firebase deploy` |
