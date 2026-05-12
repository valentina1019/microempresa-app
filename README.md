# Valentina App — Sistema de Gestión Empresarial

Sistema web PHP para gestión de clientes, inventario, ventas, usuarios y reportes de una microempresa.

---

## 📋 Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- **PHP 7.4** o superior (con soporte para MySQLi)
- **MySQL/MariaDB** (servidor de base de datos)
- **Línea de comandos** (CMD, PowerShell o terminal bash)

### Verificar que tienes PHP instalado

Abre la línea de comandos y ejecuta:

```bash
php --version
```

Si no aparece la versión, [descarga PHP](https://www.php.net/downloads) e instálalo.

---

## 🚀 Instalación y Configuración

### Paso 1: Preparar el Proyecto

1. Clona o descarga este repositorio en tu máquina.
2. Abre la línea de comandos y navega hasta la carpeta del proyecto:

```bash
cd "C:\ruta\a\tu\microempresa-app"
```

### Paso 2: Crear la Base de Datos

#### Opción A: Usando MySQL desde la línea de comandos

1. Abre el símbolo del sistema (CMD) o PowerShell.

2. Conéctate a MySQL:

```bash
mysql -u root -p
```

> Si usas MySQL sin contraseña, omite el `-p`

3. Se te pedirá la contraseña de MySQL. Introdúcela y presiona Enter.

4. Una vez conectado, copia y pega el contenido del archivo `database/schema.sql`:

```sql
CREATE DATABASE microempresa;

USE microempresa;

CREATE TABLE usuarios(
    id INT AUTO_INCREMENT PRIMARY KEY,
    usuario VARCHAR(50),
    password VARCHAR(255),
    rol VARCHAR(20) NOT NULL DEFAULT 'user'
);

INSERT INTO usuarios(usuario,password,rol)
VALUES('admin','12345','admin');

CREATE TABLE clientes(
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100),
    telefono VARCHAR(20),
    correo VARCHAR(100)
);

CREATE TABLE productos(
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100),
    stock INT,
    precio DECIMAL(10,2)
);

CREATE TABLE ventas(
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    producto_id INT,
    cantidad INT DEFAULT 0,
    subtotal DECIMAL(10,2) DEFAULT 0.00,
    iva DECIMAL(10,2) DEFAULT 0.00,
    total DECIMAL(10,2),
    fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id),
    FOREIGN KEY (producto_id) REFERENCES productos(id)
);
```

5. Presiona Enter para ejecutar los comandos. Deberías ver mensajes confirmando que la base de datos se creó correctamente.

#### Opción B: Usando phpMyAdmin (Interfaz gráfica)

1. Abre phpMyAdmin en tu navegador (generalmente `http://localhost/phpmyadmin`).

2. Haz clic en **"Nueva"** o **"Nueva base de datos"**.

3. Escribe `microempresa` como nombre y haz clic en **"Crear"**.

4. Selecciona la base de datos `microempresa` en el menú lateral.

5. Haz clic en la pestaña **"SQL"** en la parte superior.

6. Copia y pega el contenido del archivo `database/schema.sql` (omite la línea `CREATE DATABASE microempresa;`).

7. Haz clic en **"Ejecutar"** para crear las tablas.

### Paso 3: Configurar las Credenciales de Base de Datos

1. Navega hasta la carpeta `config/` del proyecto.

2. Busca o crea el archivo `.env` en la carpeta `config/`.

3. Si no existe `.env.example`, crea el archivo `.env` y añade lo siguiente:

```ini
DB_HOST=localhost
DB_PORT=3306
DB_NAME=microempresa
DB_USER=root
DB_PASSWORD=
```

**Nota:** Si tu MySQL tiene contraseña, actualiza la línea `DB_PASSWORD` con tu contraseña.

---

## ▶️ Ejecutar el Proyecto

### Opción 1: Usando el Servidor PHP Incorporado (Recomendado para desarrollo)

1. Abre la línea de comandos y navega hasta la carpeta del proyecto:

```bash
cd "C:\ruta\a\tu\microempresa-app"
```

2. Ejecuta el servidor PHP:

```bash
php -S localhost:8000
```

Deberías ver un mensaje similar a:

```
Development Server started at http://localhost:8000
```

3. Abre tu navegador web y accede a:

```
http://localhost:8000/login.php
```

### Opción 2: Usando Apache/Nginx o un Servidor Web Local

Si tienes Apache, Nginx o IIS instalado:

1. Copia la carpeta `microempresa-app` a la carpeta raíz de tu servidor web:
   - **Apache (XAMPP/WAMP):** `C:\xampp\htdocs\` o `C:\wamp\www\`
   - **Apache (MAMP):** `/Applications/MAMP/htdocs/`
   - **IIS:** Configura manualmente el sitio web

2. Accede a través de:

```
http://localhost/microempresa-app/login.php
```

---

## 🔐 Acceso Inicial

Después de ejecutar el proyecto, inicia sesión con las credenciales predeterminadas:

- **Usuario:** `admin`
- **Contraseña:** `12345`

> ⚠️ **Importante:** Cambia la contraseña después del primer acceso en la sección de usuarios.

---

## 📁 Estructura de Carpetas

```
microempresa-app/
├── config/              # Archivos de configuración
│   ├── auth.php        # Sistema de autenticación
│   ├── env.php         # Carga de variables de entorno
│   ├── sidebar.php     # Menú lateral
│   └── ui.php          # Componentes UI
├── database/
│   └── schema.sql      # Esquema de la base de datos
├── assets/
│   ├── css/
│   │   └── style.css   # Estilos del sistema
│   ├── js/
│   │   └── ui.js       # Scripts de interactividad
│   └── icons/          # Iconos SVG
├── db.php              # Conexión a base de datos
├── login.php           # Página de inicio de sesión
├── dashboard.php       # Panel principal
├── clientes-list.php   # Gestión de clientes
├── inventario-list.php # Gestión de inventario
├── sales.php           # Registro de ventas
├── reportes.php        # Reportes con filtros
├── usuarios-list.php   # Gestión de usuarios (admin)
├── logout.php          # Cierre de sesión
└── README.md           # Este archivo
```

---

## 📊 Módulos Disponibles

| Módulo         | Archivo                | Descripción                    |
| -------------- | ---------------------- | ------------------------------ |
| **Login**      | `login.php`            | Inicio de sesión seguro        |
| **Dashboard**  | `dashboard.php`        | Panel principal con navegación |
| **Usuarios**   | `usuarios-list.php`    | CRUD de usuarios (solo admin)  |
| **Clientes**   | `clientes-list.php`    | Gestión completa de clientes   |
| **Inventario** | `inventario-list.php`  | Gestión de productos y stock   |
| **Ventas**     | `sales.php`, `php.php` | Registro y control de ventas   |
| **Reportes**   | `reportes.php`         | Análisis con filtro de fechas  |
| **Logout**     | `logout.php`           | Cierre seguro de sesión        |

---

## 👥 Roles de Usuario

El sistema cuenta con dos tipos de roles:

| Rol       | Permisos                                                                                                                                                                                                         |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **admin** | ✅ Acceso completo a todos los módulos<br>✅ Crear, editar y eliminar usuarios<br>✅ Crear, editar y eliminar clientes<br>✅ Crear, editar y eliminar productos<br>✅ Crear y eliminar ventas<br>✅ Ver reportes |
| **user**  | ✅ Crear, editar y eliminar clientes<br>✅ Ver inventario (solo lectura)<br>✅ Crear ventas<br>✅ Ver reportes<br>❌ Gestionar usuarios<br>❌ Editar o eliminar productos                                        |

---

## 🗄️ Base de Datos

### Tablas Creadas

1. **usuarios** - Almacena cuentas de usuario con roles
2. **clientes** - Información de los clientes
3. **productos** - Catálogo de productos con stock y precio
4. **ventas** - Registro de todas las transacciones con IVA

### Relaciones

- `ventas.cliente_id` → `clientes.id`
- `ventas.producto_id` → `productos.id`

---

## ⚙️ Solución de Problemas

### Error: "No se puede conectar a la base de datos"

- Verifica que MySQL esté ejecutándose.
- Comprueba las credenciales en `config/.env`.
- Asegúrate de que la base de datos `microempresa` fue creada.

### Error: "Tabla no existe"

- Ejecuta nuevamente el archivo `database/schema.sql`.
- Verifica que estés usando la base de datos correcta (`microempresa`).

### Puerto 8000 ya está en uso

Si el puerto 8000 está ocupado, usa otro:

```bash
php -S localhost:8001
```

Luego accede a `http://localhost:8001/login.php`.

---

## 💡 Notas de Desarrollo

- Todos los formularios incluyen validación básica en servidor.
- Las sesiones se gestionan automáticamente en `config/auth.php`.
- Los estilos están centralizados en `assets/css/style.css`.
- Los scripts frontend están en `assets/js/ui.js`.
