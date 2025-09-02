# 🚀 Guía de Instalación - ERP Open Suite Perú

Guía paso a paso para instalar y configurar el sistema ERP en una nueva computadora.

## 📋 Prerrequisitos

### Sistema Operativo
- **Windows 10/11** (Recomendado)
- **Linux** (Ubuntu 20.04+)
- **macOS** (10.15+)

### Especificaciones Mínimas
- **RAM:** 8 GB (Recomendado: 16 GB)
- **Almacenamiento:** 10 GB de espacio libre
- **Procesador:** Intel i5 / AMD Ryzen 5 o superior

## 🛠️ Instalación de Herramientas

### 1. Java Development Kit (JDK) - ⚠️ IMPORTANTE
```bash
# SOLO JDK 11 - NO usar Java 17
# Descargar desde: https://adoptium.net/temurin/releases/?version=11
# O usar OpenJDK 11 específicamente
```

**Verificar instalación:**
```bash
java -version
# Debe mostrar: openjdk version "11.x.x"
javac -version
# Debe mostrar: javac 11.x.x
```

### 2. PostgreSQL
```bash
# Versión requerida: PostgreSQL 12 o superior
# Descargar desde: https://www.postgresql.org/download/
```

**Configuración inicial:**
```bash
# Crear usuario y base de datos
sudo -u postgres psql
CREATE USER axelor WITH PASSWORD 'TU_CONTRASEÑA_AQUI';
CREATE DATABASE axelor_open_suite OWNER axelor;
GRANT ALL PRIVILEGES ON DATABASE axelor_open_suite TO axelor;
\q
```

### 3. Node.js (Opcional - para módulos React)
```bash
# Versión requerida: Node.js 16 o superior
# Descargar desde: https://nodejs.org/
```

**Verificar instalación:**
```bash
node --version
npm --version
```

## 🗄️ Configuración de Base de Datos

### 1. Instalar PostgreSQL
Seguir las instrucciones oficiales de instalación para tu sistema operativo.

### 2. Configurar Usuario y Base de Datos
```sql
-- Conectar como superusuario
sudo -u postgres psql

-- Crear usuario (REEMPLAZA 'TU_CONTRASEÑA_AQUI' con tu contraseña)
CREATE USER axelor WITH PASSWORD 'TU_CONTRASEÑA_AQUI';

-- Crear base de datos
CREATE DATABASE axelor_open_suite OWNER axelor;

-- Otorgar privilegios
GRANT ALL PRIVILEGES ON DATABASE axelor_open_suite TO axelor;

-- Salir
\q
```

### 3. Configurar Contraseña en el Proyecto
**IMPORTANTE:** Después de crear la base de datos, debes modificar la contraseña en el archivo de configuración:

1. Abrir el archivo: `src/main/resources/axelor-config.properties`
2. Buscar la línea: `db.default.password = vilchez`
3. Cambiar `vilchez` por tu contraseña:
   ```properties
   db.default.password = TU_CONTRASEÑA_AQUI
   ```

## 📥 Clonación e Instalación

### 1. Clonar el Repositorio
```bash
git clone https://github.com/ronnypruebas123-alt/ERP-v1.git
cd ERP-v1
```

### 2. Verificar la Estructura del Proyecto
```bash
# El proyecto ya incluye todos los módulos necesarios
# No es necesario inicializar submódulos
ls modules/axelor-open-suite/
```

### 3. Verificar Configuración
```bash
# Verificar que Java 11 esté configurado
java -version
# Debe mostrar versión 11.x.x

# Verificar que PostgreSQL esté corriendo
# En Windows: Verificar servicios > PostgreSQL
# En Linux: sudo systemctl status postgresql

# Verificar que todos los módulos estén presentes
ls modules/axelor-open-suite/
# Debe mostrar todos los módulos (axelor-base, axelor-account, etc.)
```

## ⚙️ Configuración del Proyecto

### 1. Configuración de Base de Datos
El archivo `src/main/resources/axelor-config.properties` debe estar configurado así:
```properties
# Base de datos
db.default.driver = org.postgresql.Driver
db.default.ddl = update
db.default.url = jdbc:postgresql://localhost:5432/axelor_open_suite
db.default.user = axelor
db.default.password = TU_CONTRASEÑA_AQUI  # ← CAMBIAR AQUÍ

# Configuración regional
application.locale = es
application.timezone = America/Lima
data.import.demo-data = false
```

### 2. Verificar Configuración
```bash
# Verificar que el archivo de configuración tenga tu contraseña
grep "db.default.password" src/main/resources/axelor-config.properties
```

## 🚀 Ejecución

### 1. Compilar el Proyecto
```bash
# Usando Gradle Wrapper (recomendado)
./gradlew build
```

### 2. Ejecutar la Aplicación
```bash
# COMANDO CORRECTO DE ARRANQUE:
./gradlew run
```

### 3. Acceder a la Aplicación
- **URL:** http://localhost:8080
- **Usuario por defecto:** admin
- **Contraseña por defecto:** admin

## 🐛 Solución de Problemas

### Error: Java 17 detectado
```bash
# Verificar versión de Java
java -version

# Si muestra Java 17, instalar Java 11 y configurar JAVA_HOME
# Windows: Configurar variables de entorno
# Linux: export JAVA_HOME=/usr/lib/jvm/java-11-openjdk
```

### Error de Conexión a Base de Datos
```bash
# Verificar que PostgreSQL esté corriendo
# Windows: Servicios > PostgreSQL
# Linux: sudo systemctl status postgresql

# Verificar conexión con tu contraseña
psql -h localhost -U axelor -d axelor_open_suite
# Te pedirá la contraseña que configuraste
```

### Error de Memoria Java
```bash
# Aumentar memoria heap
export GRADLE_OPTS="-Xmx4g -XX:MaxPermSize=512m"
./gradlew run
```

### Error de Puerto Ocupado
```bash
# Cambiar puerto en axelor-config.properties
# O matar proceso en puerto 8080
# Windows: netstat -ano | findstr :8080
# Linux: lsof -i :8080
```

## 📝 Checklist de Instalación

- [ ] Java 11 instalado y configurado
- [ ] PostgreSQL instalado y corriendo
- [ ] Usuario `axelor` creado en PostgreSQL
- [ ] Base de datos `axelor_open_suite` creada
- [ ] Contraseña configurada en `axelor-config.properties`
- [ ] Repositorio clonado
- [ ] Estructura del proyecto verificada
- [ ] Proyecto compilado con `./gradlew build`
- [ ] Aplicación ejecutada con `./gradlew run`
- [ ] Acceso exitoso a http://localhost:8080

## ⚠️ Notas Importantes

1. **SOLO Java 11:** El proyecto NO funciona con Java 17
2. **Contraseña personalizada:** Debes cambiar la contraseña en el archivo de configuración
3. **Comando de arranque:** Usar `./gradlew run` (NO `./gradlew bootRun`)
4. **Base de datos limpia:** El sistema inicia sin datos de demostración
5. **Configuración Perú:** Timezone y idioma ya configurados para Perú
6. **Proyecto independiente:** No requiere submódulos externos, todos los módulos están incluidos

## 📞 Soporte

### Información del Proyecto
- **Versión:** 8.4.4
- **Autor:** Axelor (modificado para Perú)
- **Licencia:** GNU Affero General Public License v3.0

### Contacto
- **Desarrollador:** Ronny Vilchez
- **Email:** ronnyd.1994@gmail.com
- **Repositorio:** https://github.com/ronnypruebas123-alt/ERP-v1

---

**¡Sistema ERP listo para usar con configuración completa para Perú! 🇵🇪**
