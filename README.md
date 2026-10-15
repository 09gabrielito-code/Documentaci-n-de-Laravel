# Documentaci-n-de-Laravel
<div align="center">

# 🎓 UNIVERSIDAD TECNOLÓGICA DE PANAMÁ
### Facultad de Ingeniería de Sistemas Computacionales
### Campus Víctor Levis Sasso

---

# 🔐 Laboratorio #2 – Implementación del Login en Laravel

**Asignatura:** Desarrollo Web  
**Tema:** Patrón MVC – Unidad I  
**Instructora:** Ing. Irina Fong  
**Estudiante:** Gabriel Chifundo  
**Semestre:** II Semestre 2026  
**Fecha de ejecución del laboratorio:** 14 de octubre de 2026  
**Fecha de entrega:** 16 de octubre de 2026

![Laravel](https://img.shields.io/badge/Laravel-13.36.0-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.3.28-777BB4?style=for-the-badge&logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.4.7-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Composer](https://img.shields.io/badge/Composer-2.10.3-885630?style=for-the-badge&logo=composer&logoColor=white)
![Node](https://img.shields.io/badge/Node.js-24.22.0-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-8.3.4-646CFF?style=for-the-badge&logo=vite&logoColor=white)

</div>

---

## 📑 Tabla de contenido

1. [Introducción y objetivo](#-1-introducción-y-objetivo)
2. [Requisitos previos](#-2-requisitos-previos)
3. [Instalación y flujo de comandos](#-3-instalación-y-flujo-de-comandos)
4. [Base de datos](#-4-base-de-datos)
5. [Resultado del laboratorio](#-5-resultado-del-laboratorio)
6. [Dificultades y soluciones](#-6-dificultades-y-soluciones)
7. [Referencias](#-7-referencias)
8. [Fecha de ejecución](#-8-fecha-de-ejecución-del-laboratorio)
9. [Estructura del repositorio](#-9-estructura-del-repositorio)

---

## 📖 1. Introducción y objetivo

### 🎯 Objetivo del laboratorio
Implementar un módulo de **login (registro e inicio de sesión)** en Laravel usando el paquete de autenticación **Laravel Breeze**, aplicando la arquitectura **Modelo–Vista–Controlador (MVC)** y conectando la aplicación a una base de datos MySQL mediante migraciones.

### 🧩 Arquitectura MVC en Laravel

| Componente | Carpeta | Función |
|---|---|---|
| **Modelo (Model)** | `app/Models/` | Representa las tablas de la base de datos y su lógica. Ej.: `User.php`. |
| **Vista (View)** | `resources/views/` | Interfaz que ve el usuario, escrita con plantillas **Blade**. Ej.: `auth/login.blade.php`, `auth/register.blade.php`, `dashboard.blade.php`. |
| **Controlador (Controller)** | `app/Http/Controllers/` | Recibe la petición, procesa la lógica y devuelve una vista. Ej.: `Auth/RegisteredUserController.php`, `Auth/AuthenticatedSessionController.php`. |
| **Rutas (Routes)** | `routes/` | Conectan cada URL con su controlador o vista. Ej.: `web.php` y `auth.php` (creado por Breeze). |
| **Migraciones** | `database/migrations/` | Definen la estructura de las tablas en código PHP. |

**Flujo de una petición:**

```
Navegador → Ruta (routes/auth.php) → Controlador → Modelo (User) → Base de datos
                                          ↓
                                   Vista (Blade) → Respuesta al navegador
```

### 🗄️ Comandos utilizados para las migraciones

```bash
php artisan migrate:status    # Ver el estado de las migraciones
php artisan migrate           # Ejecutar las migraciones pendientes
php artisan migrate:fresh     # Borrar todas las tablas y volver a migrar desde cero
```

---

## 🛠️ 2. Requisitos previos

### Ecosistema de desarrollo utilizado

| Tecnología | Versión usada | Icono |
|---|---|---|
| **Sistema operativo** | Windows 11 | ![Windows](https://skillicons.dev/icons?i=windows) |
| **Paquete de servidor local** | WampServer 3.4.0 (64 bits) | 🟣 Wamp |
| **Servidor web** | Apache 2.4.65 | ![Apache](https://skillicons.dev/icons?i=apache) |
| **PHP** | 8.3.28 (requisito de la guía: 8.0 o superior) | ![PHP](https://skillicons.dev/icons?i=php) |
| **Composer** | 2.10.3 | ![Composer](https://skillicons.dev/icons?i=composer) |
| **Laravel** | 13.36.0 (creado con `composer create-project`) | ![Laravel](https://skillicons.dev/icons?i=laravel) |
| **Base de datos** | MySQL 8.4.7 (puerto 3306) / MariaDB 11.4.9 (puerto 3307) | ![MySQL](https://skillicons.dev/icons?i=mysql) |
| **Editor de código** | Visual Studio Code | ![VSCode](https://skillicons.dev/icons?i=vscode) |
| **NPM / Node.js** | Node v24.22.0 · npm 11.19.1 (sí fue necesario) | ![NodeJS](https://skillicons.dev/icons?i=nodejs) |
| **Bundler de assets** | Vite 8.3.4 | ![Vite](https://skillicons.dev/icons?i=vite) |
| **Frontend (Breeze)** | Blade + Alpine.js + Tailwind CSS | ![TailwindCSS](https://skillicons.dev/icons?i=tailwind) |
| **Control de versiones** | Git / GitHub | ![Git](https://skillicons.dev/icons?i=git) ![GitHub](https://skillicons.dev/icons?i=github) |

### 📸 Evidencia del entorno

**Versiones de PHP y Composer**

![Versiones PHP y Composer](images/01-versiones-php-composer.png)

**Página de inicio de WampServer (`localhost`)**

![Wamp localhost](images/02-wamp-localhost.png)

**Versiones de Node.js y NPM**

![Node y NPM](images/03-node-npm.png)

---

## ⚙️ 3. Instalación y flujo de comandos

### 3.1 Crear el proyecto Laravel

Dentro de la carpeta `www` de Wamp:

```bash
cd C:\wamp64\www
composer create-project laravel/laravel lab-login
cd lab-login
```

> Laravel no se descarga como programa aparte: Composer lo descarga dentro del proyecto.

### 3.2 Instalar el paquete de autenticación (Laravel Breeze)

```bash
composer require laravel/breeze --dev
php artisan breeze:install
```

Opciones elegidas durante la instalación:

| Pregunta | Respuesta |
|---|---|
| Which Breeze stack would you like to install? | **Blade with Alpine** (`blade`) |
| Would you like dark mode support? | **No** |
| Which testing framework do you prefer? | **PHPUnit** |

![Instalación de Breeze](images/12-breeze-instalacion.png)

### 3.3 Dependencias de Node y compilación de assets

```bash
npm install
npm run dev
```

![npm install y npm run dev](images/13-npm-install-dev.png)

### 3.4 Levantar el servidor de Laravel

En una **segunda terminal** (la de Vite debe seguir abierta):

```bash
php artisan serve
```

![php artisan serve](images/13b-artisan-serve.png)

Abrir en el navegador: **http://127.0.0.1:8000**

### 3.5 Resumen del flujo completo de comandos

```bash
# 1. Crear el proyecto
cd C:\wamp64\www
composer create-project laravel/laravel lab-login
cd lab-login

# 2. Configurar el .env y crear la BD en phpMyAdmin (ver sección 4)

# 3. Autenticación con Breeze
composer require laravel/breeze --dev
php artisan breeze:install

# 4. Migraciones
php artisan migrate:fresh

# 5. Assets y servidor (en dos terminales)
npm install
npm run dev
php artisan serve
```

### 3.6 Dependencias de Laravel para autenticación

| Paquete | Uso |
|---|---|
| `laravel/breeze` (dev) | Genera controladores, rutas, vistas y estilos del login y registro. |
| `laravel/framework` | Núcleo de Laravel (autenticación, sesiones, hash de contraseñas). |
| `tailwindcss`, `alpinejs`, `vite` | Estilos, interactividad y compilación del frontend. |

---

## 🗄️ 4. Base de datos

### 4.1 Entorno

- **Gestor:** MySQL 8.4.7 (servidor activo en el puerto **3306** de WampServer).
- **Administración:** phpMyAdmin 5.2.3.
- **Nombre de la base de datos:** `lab-login`.

![phpMyAdmin con la BD creada](images/06-phpmyadmin-bd-creada.png)

### 4.2 Configuración del archivo `.env`

Laravel guarda su configuración de entorno en el archivo `.env` (en la raíz del proyecto). Por defecto viene con SQLite; para este laboratorio se cambió a MySQL:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=lab-login
DB_USERNAME=root
DB_PASSWORD=
```

> ⚠️ El nombre de `DB_DATABASE` debe ser **idéntico** al de la base creada en phpMyAdmin.  
> 🔒 El `.env` está en `.gitignore` y no se sube al repositorio (contiene claves como `APP_KEY`).

![Archivo .env configurado](images/07-env-configurado.png)

### 4.3 Motor InnoDB

En `config/database.php`, dentro de la conexión `mysql`, se configuró el motor:

```php
'engine' => 'InnoDB',
```

### 4.4 Comprobar la conexión

```bash
php artisan config:clear
php artisan migrate:status
```

El mensaje **"Migration table not found"** confirmó que Laravel ya se conectaba a la base y que aún no existían tablas:

![migrate:status](images/08-migrate-status.png)

### 4.5 Migraciones: levantar las tablas

```bash
php artisan migrate:fresh
```

Migraciones ejecutadas (todas en **DONE**):

| Migración | Tabla(s) creada(s) |
|---|---|
| `0001_01_01_000000_create_users_table` | `users`, `password_reset_tokens`, `sessions` |
| `0001_01_01_000001_create_cache_table` | `cache`, `cache_locks` |
| `0001_01_01_000002_create_jobs_table` | `jobs`, `job_batches`, `failed_jobs` |

![migrate:fresh exitoso](images/09-migrate-exitoso.png)

### 4.6 Tablas resultantes en phpMyAdmin

La base `lab-login` quedó con 9 tablas: `cache`, `cache_locks`, `failed_jobs`, `jobs`, `job_batches`, `migrations`, `password_reset_tokens`, `sessions` y `users`.

![Tablas en phpMyAdmin](images/10-tablas-phpmyadmin.png)

Tabla `users` con el usuario registrado durante la prueba:

![Tabla users](images/18-tabla-users.png)

### 4.7 💾 Respaldo (backup) de la base de datos

Se exportó la base de datos completa desde phpMyAdmin (`lab-login` → **Export** → método *Quick* → formato *SQL*).

📁 Archivo incluido en el repositorio: [`database/backup/lab-login.sql`](database/backup/lab-login.sql)

![Backup SQL](images/11-backup-sql.png)

Para restaurarlo: en phpMyAdmin crear la base `lab-login` → pestaña **Import** → seleccionar `lab-login.sql`.

---

## ✅ 5. Resultado del laboratorio

### 5.1 Pantalla de bienvenida con las opciones **Log in** y **Register**

![Bienvenida de Laravel](images/14-login-register-arriba.png)

### 5.2 Formulario de registro

![Registro](images/15-registro.png)

### 5.3 Formulario de inicio de sesión

![Login](images/16-login.png)

### 5.4 🏆 Dashboard (resultado principal)

Tras registrarse o iniciar sesión, el usuario es redirigido al **Dashboard**, que muestra el mensaje *"You're logged in!"*:

![Dashboard](images/17-dashboard.png)

### 5.5 Cierre de sesión

El menú del usuario (arriba a la derecha) permite **Log Out**, que devuelve a la pantalla de bienvenida:

![Bienvenida tras Log Out](images/05-bienvenida-laravel.png)

---

## 🧯 6. Dificultades y soluciones

Durante la ejecución del laboratorio se presentaron las siguientes complicaciones:

### 🔸 Dificultad 1: El `.env` venía configurado con SQLite
- **Problema:** `DB_CONNECTION=sqlite` y las líneas `DB_HOST`, `DB_PORT`, `DB_DATABASE`, etc. estaban comentadas con `#`.
- ✅ **Solución:** Se cambió a `DB_CONNECTION=mysql`, se quitó el `#` de las líneas `DB_` y se configuraron los datos de MySQL.

![.env original con sqlite](images/21-env-sqlite.png)

### 🔸 Dificultad 2: `Unknown database`
- **Problema:** `SQLSTATE[HY000] [1049] Unknown database 'lab_login'`. La base se había creado como `lab-login` (con guion) y el `.env` decía `lab_login` (con guion bajo).
- ✅ **Solución:** Se igualó el nombre en ambos lados: `DB_DATABASE=lab-login`, y luego `php artisan config:clear`.

![Error Unknown database](images/19-error-unknown-database.png)

### 🔸 Dificultad 3: `'npm' / 'node' is not recognized`
- **Problema:** Al instalar Breeze, `npm` no se reconocía porque Node.js no estaba instalado.
- ✅ **Solución:** Se instaló Node.js (versión LTS) desde nodejs.org y se abrió una terminal nueva para que reconociera el cambio. Se verificó con `node -v` y `npm -v`.

![Error npm y node](images/20-error-npm-node.png)

### 🔸 Dificultad 4: `Table 'users' already exists`
- **Problema:** `SQLSTATE[42S01] ... 1050 Table 'users' already exists` al ejecutar `php artisan migrate`.
- ✅ **Solución:** Se reinició el estado de la base con `php artisan migrate:fresh`.

![Error tabla existente](images/22-error-tabla-existente.png)

### 🔸 Dificultad 5: `Specified key was too long`
- **Problema:** `SQLSTATE[42000] ... 1071 Specified key was too long; max key length is 1000 bytes`. MySQL creaba las tablas con un motor que limita la longitud de las claves.
- ✅ **Solución:** Se configuró `'engine' => 'InnoDB'` en `config/database.php` y se volvió a ejecutar `php artisan migrate:fresh`.

![Error key too long](images/23-error-key-too-long.png)

### 🔸 Dificultad 6: Error de Vite (`@vite`) al abrir Register
- **Problema:** La página de registro mostraba una excepción en la línea `@vite(['resources/css/app.css', 'resources/js/app.js'])` de `guest.blade.php`.
- ✅ **Solución:** Se dejó **Vite corriendo** (`npm run dev`) en una terminal y el servidor de Laravel (`php artisan serve`) en otra. Si no se quiere depender de Vite, se puede generar una versión compilada con `npm run build`.

![Error Vite](images/24-error-vite.png)

---

## 📚 7. Referencias

1. Fong, I. (2026). *Tarea: Documentar el Laboratorio de Laravel – Laboratorio #2: Implementación del Login en Laravel.* Desarrollo Web, Universidad Tecnológica de Panamá. (Documento PDF de la instructora, utilizado como guía principal).
2. Laravel. *Laravel Documentation – Installation.* https://laravel.com/docs
3. Laravel. *Laravel Starter Kits – Breeze.* https://laravel.com/docs/starter-kits
4. Composer. *Composer Documentation.* https://getcomposer.org/doc/

---

## 📅 8. Fecha de ejecución del laboratorio

La práctica se realizó el **14 de octubre de 2026**.

---

## 🗂️ 9. Estructura del repositorio

```
lab-login/
├── app/
│   ├── Http/Controllers/        # Controladores (Auth/ creado por Breeze)
│   └── Models/                  # Modelos (User.php)
├── config/                      # Configuración (database.php)
├── database/
│   ├── migrations/              # Migraciones de las tablas
│   └── backup/
│       └── lab-login.sql        # 💾 Respaldo de la base de datos
├── images/                      # 📸 Capturas usadas en este README
├── resources/views/             # Vistas Blade (auth/, dashboard.blade.php)
├── routes/
│   ├── web.php                  # Rutas web
│   └── auth.php                 # Rutas de autenticación (Breeze)
├── .env.example                 # Plantilla de configuración
└── README.md                    # 📄 Esta documentación
```

---

<div align="center">

### 👨‍🎓 Información del estudiante

Este laboratorio ha sido desarrollado por el estudiante de la **Universidad Tecnológica de Panamá**:

**Nombre:** Gabriel Chifundo  
**Correo:** *(escribe aquí tu correo)*  
**Curso:** Desarrollo Web  
**Instructor del Laboratorio:** Irina Fong.

</div>
