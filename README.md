
# Reporte de Mejoras – SDI UPM / SPI.Edu

## 1. SEGURIDAD (Prioridad alta)

### 1.1 Contraseñas en texto plano
- **Ubicación:** `AuthService.cs`, modelo `User`, tabla `users`
- **Problema:** Las contraseñas se guardan y comparan en texto plano.

### 1.2 Contraseña de BD en localStorage
- **Ubicación:** `spi_db_config` en localStorage
- **Problema:** La contraseña de MySQL se guarda en claro en el navegador.

---

## 2. INCONSISTENCIAS Y NOMENCLATURA

### 2.1 Nombre del proyecto: SPI vs SDI
- **Observación:** La app se llama "SPI.Edu" en la UI pero el proyecto es "SDI UPM".

### 2.2 Bases de datos por defecto distintas
- **DatosLF.razor:** `DbName` por defecto = `"spi_edu_db"`
- **DatabaseService.cs:** `"sdi_upm_db"`
- **database_setup.sql:** `sdi_upm_db`

### 2.3 Error tipográfico
- **MainLayout.razor (footer):** "Pedimiento" → debería ser "Pedimento" me mame xd.

---

## 3. ARQUITECTURA Y PERSISTENCIA

### 3.1 Dos sistemas de usuarios separados
- **AuthService / Login:** Modelo `User` (FullName, Email, Password, RFC, Role), usa `sdi_users` o tabla `users`.
- **Usuarios.razor:** Modelo `UsuarioRecord` (Nombre, Email, Rfc, Patente, Rol, Activo), usa `sdi_usuarios`.
- **Problema:** No hay relación entre ambos; son dos catálogos de usuarios distintos.

### 3.2 Módulos que no usan BD cuando está en modo Host
- **Cove.razor:** Solo localStorage (`sdi_coves`, `sdi_bls`).
- **Ferroviario.razor:** Solo localStorage (`sdi_manifiestos`).
- **ManifestacionValor.razor:** Solo localStorage (`sdi_mv`).
- **Vucem.razor:** Solo localStorage (`sdi_vucem_config`, `sdi_vucem_historial`).
- **Usuarios.razor:** Solo localStorage (`sdi_usuarios`), no usa tabla `users`.
- **Problema:** Al elegir Servidor SQL, solo Pedimentos y la autenticación usan BD; el resto sigue en localStorage.
---

## 4. CÓDIGO Y MANTENIMIENTO

### 4.1 Componente FocusOnNavigate
- **Ubicación:** `Routes.razor`
- **Observación:** Se usa `<FocusOnNavigate RouteData="routeData" Selector="h1" />` pero no hay componente propio definido.

### 4.2 Páginas de plantilla sin usar
- **Archivos:** `Weather.razor`, `Counter.razor` (típicos de plantillas Blazor).

### 4.3 Logging con Console.WriteLine
- **Ubicación:** `DatabaseService.cs`, `DatosLF.razor`
- **Problema:** Errores críticos se registran solo con `Console.WriteLine`.

---
