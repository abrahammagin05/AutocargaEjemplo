# 📦 Laboratorio #4 — Carga Automática (Autoload) con PSR-4 y Composer

<div align="center">

<!-- 🎯 GIF principal del proyecto aquí -->
![gif-principal](PEGA_AQUI_URL_DE_UN_GIF_DE_PHP_O_COMPOSER)

![PHP](https://img.shields.io/badge/PHP-8.x-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Composer](https://img.shields.io/badge/Composer-2.x-885630?style=for-the-badge&logo=composer&logoColor=white)
![PSR--4](https://img.shields.io/badge/PSR--4-Autoload-blue?style=for-the-badge&logo=php&logoColor=white)
![WampServer](https://img.shields.io/badge/WampServer-3.x-orange?style=for-the-badge&logo=apache&logoColor=white)

**Universidad Tecnológica de Panamá**  
Facultad de Ingeniería en Sistemas Computacionales  
Campus Victor Levis Sasso

</div>

---

## 📋 Tabla de Contenidos

- [Objetivo del Laboratorio](#-objetivo-del-laboratorio)
- [Requisitos Previos](#-requisitos-previos)
- [Introducción a PSR-4 y Autoload](#-introducción-a-psr-4-y-autoload)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [Secuencia de Comandos](#-secuencia-de-comandos-utilizados)
- [Código Fuente Documentado](#-código-fuente-documentado)
- [Pruebas de Ejecución](#-pruebas-de-ejecución)
- [Conclusiones Técnicas](#-conclusiones-técnicas)
- [Dificultades y Soluciones](#-dificultades-y-soluciones)
- [Referencias](#-referencias)
- [Footer](#-desarrollado-por)

---

## 🎯 Objetivo del Laboratorio

Implementar el estándar **PSR-4** para la carga automática de clases en PHP utilizando **Composer**, eliminando completamente el uso de `include` y `require` manuales en el proyecto.

| # | Objetivo |
|---|---|
| 1️⃣ | Comprender y aplicar el estándar **PSR-4** para la organización de archivos y clases |
| 2️⃣ | Configurar el archivo `composer.json` para establecer un mapa de **Carga Automática** |
| 3️⃣ | Utilizar el comando `dump-autoload` para eliminar el uso de `include` y `require` manuales |

---

## 🛠️ Requisitos Previos

### Ecosistema de Desarrollo

| Tecnología | Versión | Descripción |
|---|---|---|
| ![PHP](https://img.shields.io/badge/PHP-777BB4?logo=php&logoColor=white) **PHP** | 8.0 o superior | Lenguaje de programación del servidor |
| ![Composer](https://img.shields.io/badge/Composer-885630?logo=composer&logoColor=white) **Composer** | Última versión estable | Gestor de dependencias y autoload de PHP |
| ![WampServer](https://img.shields.io/badge/WampServer-orange?logo=apache&logoColor=white) **WampServer** | 3.x | Entorno de desarrollo local (Apache + PHP) |
| ![VSCode](https://img.shields.io/badge/VS_Code-007ACC?logo=visualstudiocode&logoColor=white) **Visual Studio Code** | Última versión | Editor de código recomendado |
| ![Git](https://img.shields.io/badge/Git-F05032?logo=git&logoColor=white) **Git** | Última versión | Control de versiones |
| ![Windows](https://img.shields.io/badge/Windows-0078D6?logo=windows&logoColor=white) **Sistema Operativo** | Windows 10/11 | SO utilizado en el laboratorio |

---

## 📚 Introducción a PSR-4 y Autoload

### ¿Qué son los PSR?

**PSR** es el acrónimo de *PHP Standard Recommendation*. Fue creado con el objetivo de redactar reglas para escribir código fuente que pueda ser compartido y entendido por toda la comunidad de desarrolladores. Proyectos como **Symfony**, **Laravel**, **Composer** o **Magento** siguen estas reglas.

### ¿Qué es PSR-4?

**PSR-4** es el estándar de autocarga (*autoloading*) que define cómo las clases deben ser **mapeadas automáticamente** a archivos del sistema de archivos, usando los **namespaces** como referencia directa a las carpetas.

### La Analogía de la Carpeta

Si tu archivo está en `App/User.php`, tu namespace debería ser `namespace App;`:

| Parte | Significado |
|---|---|
| `App` | Es el "prefijo" o nombre de tu proyecto (definido en `composer.json`) |
| `Database\Model` | Son las subcarpetas físicas |

### ¿Qué problema resuelve?

| ❌ Sin Autoload (Antes) | ✅ Con Autoload PSR-4 (Después) |
|---|---|
| Múltiples `include_once` / `require_once` por archivo | Una sola línea: `require 'vendor/autoload.php'` |
| Cada nueva clase requiere agregar un `include` manualmente | Las clases se detectan automáticamente por su namespace |
| Propenso a errores: rutas rotas, archivos olvidados | Composer genera el mapa de clases automáticamente |
| Difícil de mantener en proyectos grandes | Escala sin modificar configuraciones globales |

### ¿Y qué hace el `autoload.php`?

Con el autoload de Composer, el proceso se vuelve inteligente. Cuando escribes `$obj = new User();`, PHP se detiene un milisegundo y pregunta: *"¿Alguien conoce la clase User?"*. El archivo `vendor/autoload.php` levanta la mano y dice: *"Yo sé dónde está, según el mapa de carpetas que configuraste, debe estar en App/User.php"*. El autoload hace el `require` por ti de forma invisible y el código sigue corriendo.

### Reglas de Oro (Autocarga PSR-4)

| Regla | Ejemplo |
|---|---|
| El archivo se llama igual que la clase | `User.php` → `class User {}` |
| El namespace refleja la carpeta | Carpeta `App/` → `namespace App;` |
| Los nombres de clase van en **StudlyCaps** | `ProductModel`, `UserProfile`, `DatabaseConnection` |
| En `composer.json` defines el mapeo | `"App\\": "App/"` |
| En el `index.php` importas con `use` | `use App\User;` |

### Convenciones PSR-1 / PSR-4

| Convención | Correcto ✅ | Incorrecto ❌ |
|---|---|---|
| Nombres de clase | `UserProfile` (StudlyCaps) | `userProfile`, `user_profile` |
| Constantes | `VERSION` (MAYÚSCULAS) | `version`, `Version` |
| Nombres de método | `setName()` (camelCase) | `SetName()`, `set_name()` |
| Cada clase en su propio archivo | `User.php` → `class User` | Varias clases en un archivo |
| Namespace mínimo de un nivel | `namespace App;` | Sin namespace (global) |

---

## 🗂️ Estructura del Proyecto

```
AutocargaEjemplo/
│
├── 📄 composer.json              ← Configuración del mapeo PSR-4
├── 📄 .gitignore                 ← Excluye vendor/ del repositorio
├── 📄 index.php                  ← Punto de entrada principal
├── 📄 README.md                  ← Este archivo
│
├── 📁 App/                       ← Namespace: App\
│   └── User.php                  ← Clase App\User
│
├── 📁 Database/                  ← Namespace: Database\
│   └── 📁 Model/                 ← Namespace: Database\Model\
│       └── ProductModel.php      ← Clase Database\Model\ProductModel
│
└── 📁 vendor/                    ← (Generado por Composer — excluido del repo)
    ├── autoload.php              ← Autoloader principal
    ├── autoload_classmap.php     ← Mapa de clases
    ├── autoload_namespaces.php   ← Mapa de namespaces
    ├── autoload_psr4.php         ← Mapa PSR-4
    ├── autoload_real.php         ← Cargador real
    ├── autoload_static.php       ← Cargador estático
    ├── ClassLoader.php           ← Clase interna de Composer
    └── LICENSE                   ← Licencia de Composer
```

### Relación Namespaces ↔ Carpetas Físicas

| Namespace Completo | Archivo Físico |
|---|---|
| `App\User` | `App/User.php` |
| `Database\Model\ProductModel` | `Database/Model/ProductModel.php` |

---

## ⚙️ Secuencia de Comandos Utilizados

<!-- 🎮 GIF de terminal/coding aquí -->
![gif-terminal](PEGA_AQUI_URL_DE_UN_GIF_DE_TERMINAL_O_CODING)

### 1️⃣ Crear la carpeta del proyecto y su estructura

```bash
mkdir AutocargaEjemplo
cd AutocargaEjemplo
mkdir App
mkdir -p Database/Model
```

### 2️⃣ Crear el archivo `composer.json`

```json
{
    "autoload": {
        "psr-4": {
            "App\\": "App/",
            "Database\\Model\\": "Database/Model/"
        }
    }
}
```

### 3️⃣ Crear las clases PHP

```bash
# Clase User en App/
notepad App/User.php

# Clase ProductModel en Database/Model/
notepad Database/Model/ProductModel.php
```

### 4️⃣ Generar el Autoloader con Composer

```bash
composer dump-autoload
```

> 📌 ¡Este es el **comando mágico**! Al ejecutarlo, Composer revisa todos los archivos, namespaces y la estructura de carpetas, y genera el mapa en `vendor/` para que PHP sepa dónde está cada clase.

### 5️⃣ Crear el archivo `index.php`

```bash
notepad index.php
```

### 6️⃣ Ejecutar el proyecto

```bash
php index.php
```

**Salida esperada:**

```
Dave123
```

### 7️⃣ Subir al repositorio

```bash
git init
git add .
git commit -m "Laboratorio PSR-4 Autoload con Composer"
git remote add origin https://github.com/tu-usuario/autoload-lab.git
git push -u origin main
```

---

## 💻 Código Fuente Documentado

### 📄 `composer.json` — Configuración del Autoload

```json
{
    "autoload": {
        "psr-4": {
            "App\\": "App/",
            "Database\\Model\\": "Database/Model/"
        }
    }
}
```

> 🔑 **Línea clave:** `"App\\": "App/"` mapea el namespace `App\` a la carpeta `App/`. Lo mismo con `"Database\\Model\\"` a `Database/Model/`.

---

### 📄 `App/User.php` — Clase User

```php
<?php

namespace App;

class User
{
    public function getName(): string
    {
        return "Dave";
    }
}
```

> 🔑 `namespace App;` vincula la clase a la carpeta `App/`. El nombre del archivo (`User.php`) coincide con el nombre de la clase (`class User`), siguiendo PSR-4.

---

### 📄 `Database/Model/ProductModel.php` — Clase ProductModel

```php
<?php

namespace Database\Model;

class ProductModel
{
    public function getId(): int
    {
        return 123;
    }
}
```

> 🔑 `namespace Database\Model;` tiene **dos niveles** que corresponden a la ruta de carpetas `Database/Model/`.

---

### 📄 `index.php` — Archivo Principal

```php
<?php

use App\User;
use Database\Model\ProductModel;

require 'vendor/autoload.php';

$user = new User();
echo $user->getName();

$product = new ProductModel();
echo $product->getId();
echo "\n";
```

> 🔑 **No hay ningún** `include_once` ni `require_once`. Solo `require 'vendor/autoload.php'` y las declaraciones `use`. Composer hace el `require` de cada clase internamente de forma invisible.

---

### 📄 `.gitignore` — Higiene del Repositorio

```gitignore
# Dependencias de Composer (se generan localmente con composer install)
/vendor/

# Archivos del sistema operativo
.DS_Store
Thumbs.db

# IDEs
.idea/
.vscode/
```

> 📌 La carpeta `vendor/` se excluye del repositorio. Cada desarrollador la genera localmente ejecutando `composer dump-autoload`.

---

## 📸 Pruebas de Ejecución

### ✅ Ejecución de `composer dump-autoload`

<!-- 🖼️ PEGA AQUÍ SCREENSHOT DE composer dump-autoload EN TU TERMINAL -->
![screenshot-dump-autoload](PEGA_AQUI_URL_O_RUTA_DEL_SCREENSHOT_DUMP_AUTOLOAD)

> 📌 *Comando:* `composer dump-autoload` — genera la carpeta `vendor/` con el autoloader.

---

### ✅ Estructura de `vendor/` generada

<!-- 🖼️ PEGA AQUÍ SCREENSHOT DE LA CARPETA VENDOR EN VS CODE -->
![screenshot-vendor](PEGA_AQUI_URL_O_RUTA_DEL_SCREENSHOT_VENDOR)

> 📌 Después de ejecutar el comando, se genera `vendor/` con los archivos: `autoload.php`, `autoload_psr4.php`, `autoload_classmap.php`, `ClassLoader.php`, etc.

---

### ✅ Ejecución de `php index.php`

<!-- 🖼️ PEGA AQUÍ SCREENSHOT DE LA TERMINAL CON LA SALIDA Dave123 -->
![screenshot-ejecucion](PEGA_AQUI_URL_O_RUTA_DEL_SCREENSHOT_EJECUCION)

> 📌 *Comando:* `php index.php`

**Salida obtenida:**

```
Dave123
```

> ✅ Las clases `App\User` y `Database\Model\ProductModel` se instanciaron correctamente **sin errores de "Class not found"** y sin usar ningún `include` ni `require` manual.

---

### ✅ Ciclo de Vida del Autoload

```
1. Preparación (composer.json)
   └─ Defines que App\ → App/
   └─ Defines que Database\Model\ → Database/Model/

2. Generación (composer dump-autoload)
   └─ Composer lee composer.json
   └─ Genera vendor/autoload.php con el mapa de rutas

3. Ejecución (php index.php)
   └─ require 'vendor/autoload.php';
   └─ $user = new User();
   └─ PHP pregunta: "¿Alguien conoce la clase User?"
   └─ Autoload responde: "Sí, está en App/User.php"
   └─ Hace el require por ti de forma invisible ✓
   └─ echo $user->getName(); → "Dave"
   └─ echo $product->getId(); → 123
```

---

## 🧠 Conclusiones Técnicas

### 1️⃣ Mantenibilidad

Con PSR-4, agregar una nueva clase al proyecto es tan simple como crear un archivo PHP en la carpeta correspondiente y declarar su namespace. **No es necesario modificar ningún archivo de configuración global** ni agregar líneas de `include`/`require` en otros archivos.

**Ejemplo:** Si necesitamos agregar una clase `Invoice`, solo creamos:
```
Database/Model/Invoice.php  →  namespace Database\Model;  →  class Invoice {}
```
Y ya está disponible en todo el proyecto con `use Database\Model\Invoice;` — sin tocar nada más.

---

### 2️⃣ Eficiencia de Memoria (Lazy Loading)

El autoloader de Composer implementa **carga bajo demanda (Lazy Loading)**: las clases solo se cargan en memoria cuando se utilizan por primera vez.

| Enfoque | Comportamiento | Impacto en Memoria |
|---|---|---|
| `include_once` manual | Carga **todas** las clases al inicio, aunque no se usen | ❌ Mayor consumo |
| Autoload PSR-4 | Carga **solo** las clases cuando se instancian | ✅ Menor consumo |

Cuando escribes `$user = new User();`, PHP pregunta *"¿Alguien conoce la clase User?"* y el autoload hace el `require` por ti de forma invisible. Si nunca instancias una clase, nunca se carga en memoria.

---

### 3️⃣ Estandarización

Seguir el estándar **PSR-4** garantiza que cualquier desarrollador que conozca la convención pueda entender la estructura del proyecto sin documentación adicional:

| Beneficio | Descripción |
|---|---|
| 🔄 **Interoperabilidad** | Librerías de terceros y frameworks (Laravel, Symfony) usan el mismo estándar |
| 👥 **Colaboración** | Nuevos desarrolladores entienden la estructura inmediatamente |
| 📦 **Ecosistema** | Compatible con los +300,000 paquetes de Packagist |
| 🏗️ **Preparación profesional** | Dominar PSR-4 es requisito en la industria PHP moderna |

---

## ⚠️ Dificultades y Soluciones

<!-- 🎮 GIF de error/bug gracioso aquí -->
![gif-bugs](PEGA_AQUI_URL_DE_UN_GIF_DE_BUG_O_ERROR_GRACIOSO)

### ❓ Dificultad 1 — Class not found al ejecutar `php index.php`

**Error encontrado:**
```
PHP Fatal error: Uncaught Error: Class "App\User" not found in index.php
```

**Causa:** No se había ejecutado `composer dump-autoload` después de crear el `composer.json`.

**Solución aplicada:**
```bash
composer dump-autoload
```

---

### ❓ Dificultad 2 — Namespace no coincide con la carpeta (Case-Sensitive)

**Error encontrado:**
```
PHP Fatal error: Uncaught Error: Class "database\model\ProductModel" not found
```

**Causa:** El namespace en el archivo usaba minúsculas (`database\model`) mientras la carpeta era `Database/Model` con mayúscula. PSR-4 es **sensible a mayúsculas**.

**Solución aplicada:**
```php
// ❌ Incorrecto
namespace database\model;

// ✅ Correcto
namespace Database\Model;
```

---

### ❓ Dificultad 3 — `vendor/` subida al repositorio por accidente

**Error encontrado:** El repositorio pesaba demasiado y contenía archivos innecesarios.

**Causa:** No se creó el `.gitignore` antes del primer commit.

**Solución aplicada:**
```bash
echo "/vendor/" >> .gitignore
git rm -r --cached vendor/
git commit -m "Excluir vendor/ del repositorio"
```

---

## 🎒 Referencias

1. **Video de Autoayuda Proporcionado**  
   🔗 https://www.youtube.com/watch?v=93pCiZT99Ks

2. **PHP-FIG — PSR-4: Autoloader**  
   🔗 https://www.php-fig.org/psr/psr-4/

3. **Cómo escribir código estándar en PHP utilizando los PSRs**  
   🔗 https://www.kodetop.com/como-escribir-codigo-estandar-en-php-utilizando-los-psrs/

4. **Composer — Documentación de Autoload**  
   🔗 https://getcomposer.org/doc/01-basic-usage.md#autoloading

5. **Composer Página Oficial — Descarga**  
   🔗 https://getcomposer.org/download/

---

## 📅 Fecha de Ejecución del Laboratorio

| Detalle | Información |
|---|---|
| 📆 Fecha de entrega | 27 al 29 de abril de 2026 |
| ⏱️ Duración aproximada | 1.5 horas |
| 💻 Entorno | Windows 11 + WampServer + VS Code |

---

<div align="center">

## 👨‍💻 Desarrollado por

<!-- 🎉 GIF de celebración para el footer -->
![gif-footer](PEGA_AQUI_URL_DE_UN_GIF_CHIDO_PARA_EL_FOOTER)

**Este laboratorio ha sido desarrollado por el estudiante de la Universidad Tecnológica de Panamá:**

| Campo | Información |
|---|---|
| 👤 **Nombre** | Abraham Magin |
| 📧 **Correo** | abraham.magin@utp.ac.pa |
| 📚 **Curso** | Desarrollo de Software VII |
| 👩‍🏫 **Instructora** | Ing. Irina Fong |
| 🏫 **Universidad** | Universidad Tecnológica de Panamá |
| 🏛️ **Facultad** | Ingeniería en Sistemas Computacionales |
| 🎓 **Carrera** | Lic. Desarrollo y Gestión de Software |

---

*Campus Victor Levis Sasso — I Semestre 2026*

</div>
