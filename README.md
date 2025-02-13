# 📌 Configuración de Oracle XE en Docker con PowerShell

## **1️⃣ Registro en Oracle Container Registry**
Antes de descargar la imagen de Oracle XE, es necesario registrarse e iniciar sesión.

### **1.1 Registrarse en Oracle Container Registry**
1. Ir a [Oracle Container Registry](https://container-registry.oracle.com/)
2. Iniciar sesión con tu cuenta de Oracle
3. Aceptar los términos de la licencia en la sección `database/express`

### **1.2 Iniciar sesión en Docker**
Ejecuta el siguiente comando en PowerShell:

```powershell
 docker login container-registry.oracle.com
```

Si la autenticación es correcta, verás el mensaje:

```
Login Succeeded
```

---

## **2️⃣ Descargar y ejecutar el contenedor de Oracle XE**

### **2.1 Descargar la imagen de Oracle XE**
```powershell
 docker pull container-registry.oracle.com/database/express:latest
```

### **2.2 Ejecutar el contenedor**
```powershell
 docker run -d -p 1521:1521 -p 5500:5500 --name oracle-xe `
   -e ORACLE_PWD=tu_contraseña_secreta `
   container-registry.oracle.com/database/express:latest
```

### **2.3 Verificar que el contenedor está corriendo**
```powershell
 docker ps
```
Salida esperada:
```
CONTAINER ID   IMAGE                                              STATUS              PORTS                          NAMES
xxxxxxxxxxxx   container-registry.oracle.com/database/express   Up (healthy)        0.0.0.0:1521->1521/tcp, 0.0.0.0:5500->5500/tcp   oracle-xe
```

---

## **3️⃣ Conectarse a Oracle XE y Crear la Tabla**

### **3.1 Ingresar al contenedor y conectarse con SQL*Plus**
```powershell
 docker exec -it oracle-xe sqlplus system/tu_contraseña_secreta@localhost:1521/XE
```

Si `sqlplus` no está disponible, primero accede al contenedor:
```powershell
 docker exec -it oracle-xe bash
 sqlplus system/tu_contraseña_secreta
```

### **3.2 Crear la tabla `estudiantes`**
```sql
CREATE TABLE estudiantes (
    id NUMBER PRIMARY KEY,
    nombre VARCHAR2(100),
    edad NUMBER
);
```

### **3.3 Insertar datos en la tabla**
```sql
INSERT INTO estudiantes (id, nombre, edad) VALUES (1, 'Juan Pérez', 20);
INSERT INTO estudiantes (id, nombre, edad) VALUES (2, 'Ana Gómez', 22);
COMMIT;
```

### **3.4 Consultar los datos**
```sql
SELECT * FROM estudiantes;
```

---

✅ **¡Listo! Con esto tienes Oracle XE funcionando en Docker, con conexión en PowerShell y la base de datos configurada correctamente.** 🚀
