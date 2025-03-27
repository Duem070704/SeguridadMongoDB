# Laboratorio de Seguridad en MongoDB

En este documento aprenderemos los pasos para habilitar la autenticación en MongoDB, crear usuarios con roles específicos y configurar conexiones seguras mediante SSL/TLS.

## 1. Habilitar la Autenticación en MongoDBB

MongoDB permite conexiones sin autenticación por defecto. Para cambiar esto:
#### 1.1. Abre PowerShell como Administrador y edita el archivo de configuración de MongoDB ```(mongod.cfg)```

#### 1.2. Agrega o modifica la siguiente configuración en la sección security:

```security:```
```authorization: enabled```

![Texto alternativo](/assets/1.png)

#### 1.3. Guarda los cambios y reinicia MongoDB con:
```net stop MongoDB```
```net start MongoDB```

![Texto alternativo](/assets/2.png)

#### Validación: MongoDB ahora requiere autenticación para acceder a las bases de datos.
![Texto alternativo](/assets/22.png)

## 2. Crear Usuarios con Roles y Permisos Específicos
Acceder a MongoDB con privilegios de administrador

#### 2.1. Abre PowerShell y ejecuta el shell de MongoDB:
```Mongosh```
![Texto alternativo](/assets/3.png)

#### 2.2. Cambia a la base de datos admin:
```use admin```
![Texto alternativo](/assets/4.png)

#### Crear un usuario administrador
```db.createUser({ user: "adminUser",pwd: "Admin1…!", roles: [{ role: "userAdminAnyDatabase", db: "admin" }]})```

![Texto alternativo](/assets/6.png)

#### 📌Nota
```userAdminAnyDatabase``` Sirve para crear un usuario administrador de usuarios.

#### Crear un usuario con permisos de solo lectura
```use (miBaseDatos) ```
```db.createUser({user: "readUser" pwd:"ReadPassword123!", roles: [{ role: "read", db: "miBaseDatos" }]});```

![Texto alternativo](/assets/8.png)


#### Crear un usuario con permisos de lectura y escritura
```db.createUser({user: "writeUser",pwd: "WritePassword123!",roles: [{ role: "readWrite", db: "miBaseDatos" }]});```

![Texto alternativo](/assets/9.png)

### Validación: Intenta conectarte con cada usuario y verifica que solo pueda realizar las acciones permitidas.

#### Usuario Administrador 
##### PowerShell
![Texto alternativo](/assets/12.png)
##### MongoDB
![Texto alternativo](/assets/10.png)
![Texto alternativo](/assets/14.png)

#### Usuario de Lectura 
##### PowerShell
![Texto alternativo](/assets/15.png)
##### MongoDB
![Texto alternativo](/assets/17.png)
![Texto alternativo](/assets/18.png)


#### Usuario Lectura-Escritura
##### PowerShell
![Texto alternativo](/assets/19.png)
##### MongoDB
![Texto alternativo](/assets/20.png)
![Texto alternativo](/assets/21.png)

## 3. Configurar Conexiones Seguras con SSL/TLS

### Generar un Certificado SSL con OpenSSL
#### 3.1.	Abre PowerShell y ejecuta:
```openssl req -newkey rsa:2048 -new -x509 -days 365 -nodes -out```
```C:\mongodb-cert.pem -keyout C:\mongodb-key.pem```

![Texto alternativo](/assets/23.png)

#### 3.2. Combina las claves en un solo archivo:
```type C:\mongodb-key.pem C:\mongodb-cert.pem > C:\mongodb.pem```
```Get-Content C:\mongodb-key.pem, C:\mongodb-cert.pem | Set-Content C:\mongodb.pem```

##### Error
![Texto alternativo](/assets/24.png)

##### Solución
EI error ocurre porque type en PowerShell no funciona como en la línea de comandos ( cmd ). En PowerShell, debes usar Get-Content en combinación con Set-content o Out-FiIe para concatenar los archivos correctamente.

```Get-Content C:\mongodb-key.pem, C:\mongodb-cert.pem | Set-Content C:\mongodb.pem```

![Texto alternativo](/assets/25.png)

### Configurar MongoDB para Usar SSL
#### 3.3.	  Abre mongod.cfg y agrega/modifica:
```net:```
    ```  ssl:```
     ```    mode: requireSSL```
    ``` PEMKeyFile: C:\mongodb.pem```

![Texto alternativo](/assets/26.png)

### 3.4. Reinicia MongoDB para aplicar los cambios:
```net stop MongoDB```
```net start MongoDB```

![Texto alternativo](/assets/27.png)

#### 📌Nota
Este laboratorio no se pudo completar pero no se logro estableces la conexión, pero se avanzo hasta esta parte.
