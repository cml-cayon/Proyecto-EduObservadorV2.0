# EduObservador v2.0

Sistema de gestión de observaciones escolares desarrollado en Java con interfaz gráfica Swing. Permite a administradores, docentes y estudiantes interactuar con un registro centralizado de observaciones disciplinarias y académicas, organizado por roles con acceso diferenciado.

---

## Tabla de contenidos

- [Descripción del sistema](#descripción-del-sistema)
- [Requisitos previos](#requisitos-previos)
- [Configuración del almacenamiento](#configuración-del-almacenamiento)
- [Instalación y despliegue local](#instalación-y-despliegue-local)
- [Credenciales de prueba](#credenciales-de-prueba)
- [Roles y funcionalidades](#roles-y-funcionalidades)
- [Estructura del proyecto](#estructura-del-proyecto)

---

## Descripción del sistema

EduObservador es una aplicación de escritorio orientada a instituciones educativas de básica y media. Centraliza el registro, consulta y seguimiento de observaciones estudiantiles, clasificadas por nivel de gravedad (Leve, Grave, Gravísima), materia, grado y fecha.

### Evolución respecto a v1.0

La versión 2.0 introduce mejoras sustanciales sobre la v1.0:

- **Interfaz completamente rediseñada** con tema visual de color azul marino, botones personalizados con bordes redondeados y degradados, y navegación por pestañas en el panel de administrador.
- **Gestión de grados** como entidad propia: los grados ahora se administran de forma independiente, con listado de estudiantes vinculados a cada uno.
- **Persistencia robusta** mediante serialización binaria de objetos Java, reemplazando el almacenamiento en texto plano de versiones anteriores.
- **Filtros avanzados de observaciones**: filtrado por grado y ordenamiento por fecha (más reciente / más antiguo) o por nivel de gravedad.
- **Perfil de usuario** con cambio de contraseña persistente para todos los roles.
- **Generación automática de nombres de usuario** a partir del nombre completo del registro.

---

## Requisitos previos

- **Java JDK 11 o superior**

Para verificar que Java está instalado correctamente, ejecutar en la terminal:

```bash
java -version
javac -version
```

Ambos comandos deben mostrar una versión 11 o superior.



## Configuración del almacenamiento

EduObservador utiliza **persistencia local mediante archivos binarios** (serialización Java). No requiere base de datos ni servidor externo.

### Ubicación de los archivos de datos

Los archivos se crean automáticamente en la carpeta `data/` dentro del **directorio desde el que se ejecuta el programa**:

```
data/
├── usuarios.dat        # Usuarios registrados (admin, docentes, estudiantes)
├── observaciones.dat   # Observaciones registradas
└── grados.dat          # Grados y sus estudiantes asociados
```

> La carpeta `data/` se genera sola en la primera ejecución. No es necesario crearla manualmente.

### Consideraciones importantes

- Los archivos `.dat` son binarios; **no deben editarse manualmente**.
- Si se desea reiniciar el sistema a un estado limpio, basta con eliminar la carpeta `data/`. En la siguiente ejecución se recreará con el usuario administrador por defecto.
- Para hacer una copia de seguridad del sistema, copiar la carpeta `data/` completa.
- El programa debe ejecutarse siempre desde la misma carpeta raíz para que encuentre la ruta `data/` correctamente.



## Instalación y despliegue local

## Instalación

### 1. Obtener el código fuente

Descomprimir el archivo `.zip` del proyecto en la ubicación deseada. La estructura resultante será:

```
EduObservadorV2.0/
├── src/
└── README.md
```
## Despliegue local.
```
1. Verificar que el JDK esté instalado.
2. Abrir una IDE compatible.
3. Abrir el proyecto en la IDE.
4. Localizar el archivo Main.java y ejecutarlo.
5. En la primera ejecución, la aplicación crea automáticamente la carpeta `data/` y registra el usuario administrador por defecto.
6. Iniciar sesión con las credenciales del administrador (ver sección siguiente) para registrar los demás usuarios.

El programa no requiere conexión a internet. Funciona completamente de forma local.

---
```
## Credenciales de prueba
```
### Administrador (creado por defecto)

| Campo      | Valor     |
|------------|-----------|
| Usuario    | `admin`   |
| Contraseña | `admin123`|

El administrador no puede ser eliminado desde la interfaz.
```
### Docentes y estudiantes
```
Los usuarios de tipo Docente y Estudiante son creados por el administrador desde el panel de gestión. Al momento del registro, el sistema asigna automáticamente:

| Campo      | Valor asignado automáticamente                                         |
|------------|------------------------------------------------------------------------|
| Usuario    | Primera letra del nombre + apellido (ej: `ccayon` para Camilo Cayon)  |
| Contraseña | `12345678`                                                             |

Cada usuario puede cambiar su contraseña desde la opción **Perfil** una vez dentro del sistema. El cambio se guarda de forma persistente.

### Ejemplo de usuarios de prueba

Para probar el sistema sin necesidad de crear registros manualmente, ingresar como administrador y registrar los siguientes usuarios:

**Docente de ejemplo:**

| Campo      | Valor              |
|------------|--------------------|
| Nombre     | Leonardo Melo      |
| Materia    | Biología           |
| Usuario    | `lmelo`            |
| Contraseña | `12345678`         |

**Estudiante de ejemplo:**

| Campo      | Valor          |
|------------|----------------|
| Nombre     | Camilo Cayon   |
| Grado      | 11°            |
| Usuario    | `ccayon`       |
| Contraseña | `cml123`       |

---
```
## Roles y funcionalidades

### Administrador

- Registrar y eliminar estudiantes y docentes.
- Gestionar grados (crear, eliminar, ver estudiantes por grado).
- Consultar todas las observaciones con filtros por grado y orden por fecha o gravedad.
- Ver perfil y cambiar contraseña.

### Docente

- Registrar observaciones para cualquier estudiante, indicando grado, gravedad y descripción. La fecha y la materia se asignan automáticamente.
- Consultar el historial de observaciones registradas, con código de color según gravedad.
- Ver perfil y cambiar contraseña.

### Estudiante

- Consultar únicamente sus propias observaciones.
- Ver sus datos personales (correo, fecha de nacimiento, grado).
- Cambiar su contraseña.
```
---
```
## Estructura del proyecto
```

src/
├── main/
│   └── Main.java                        # Punto de entrada
├── controlador/
│   └── LoginController.java             # Lógica de autenticación
├── modelo/
│   ├── usuario/
│   │   ├── Usuario.java                 # Clase abstracta base
│   │   ├── Administrador.java
│   │   ├── Docente.java
│   │   └── Estudiante.java
│   ├── observacion/
│   │   └── Observacion.java
│   └── grado/
│       └── Grado.java
├── repositorio/
│   └── DataRepository.java              # Persistencia (serialización)
├── servicio/
│   ├── AuthService.java                 # Gestión de usuarios y autenticación
│   ├── ObservacionService.java          # Registro y consulta de observaciones
│   └── GradoService.java               # Gestión de grados
└── vista/
    ├── UITheme.java                     # Componentes visuales reutilizables
    ├── LoginFrame.java                  # Pantalla de inicio de sesión
    ├── DashboardFrame.java              # Panel del administrador
    ├── DocenteFrame.java                # Panel del docente
    ├── EstudianteFrame.java             # Panel del estudiante
    └── PerfilDialog.java                # Diálogo de perfil compartido


```
## Proyecto realizado Por Camilo Alejandro Cayon Cadavid y Samuel Quevedo Ossa para la asignatura Programación orientada a objetos como práctica acerca de los conceptos aprendidos durante el curso.
```

