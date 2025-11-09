# 🥩 Carnicería *El Buen Corte*

## 🎯 Objetivo general
Desarrollar una aplicación **Full Stack** usando **React (frontend)** y **Node.js con Express (backend)** que permita a los usuarios **registrarse, iniciar sesión y ver una lista de productos (tipos de carne)** obtenidos desde una base de datos PostgreSQL.

---

## 🧠 Contexto del proyecto
Una carnicería llamada **“El Buen Corte”** quiere digitalizar su negocio.  
Necesita una aplicación web donde:
- Los clientes puedan **registrarse e iniciar sesión**.
- Una vez dentro, puedan **ver los tipos de carne disponibles**, con su **nombre, precio y descripción**.
- (Opcional más adelante): permitir al administrador **agregar, editar o eliminar productos**.

---

## 🏗️ Requerimientos funcionales

### 🖥️ Frontend (React)
- Pantalla de **Registro** con los campos:
  - Nombre  
  - Email  
  - Contraseña  
- Pantalla de **Login**
  - Email  
  - Contraseña  
- Pantalla principal (**Home**) que:
  - Muestra la lista de productos (por ej. cortes de carne).  
  - Obtiene los datos desde una **API REST del backend**.  
- Navegación simple usando **react-router-dom**.

### ⚙️ Backend (Node.js + Express)
- **Endpoints:**
  - `POST /api/register` → crear usuario.  
  - `POST /api/login` → autenticar usuario.  
  - `GET /api/products` → listar productos (solo accesible si el usuario está logueado).  
- Conexión a una base de datos PostgreSQL.
- Uso de **bcrypt** para encriptar contraseñas.
- Uso de **JWT (JSON Web Token)** para manejar sesiones.

---

## 🗃️ Base de datos
En lugar de crear las tablas manualmente con SQL, el proyecto utiliza **Sequelize** como ORM, que define los modelos y sincroniza la base automáticamente con PostgreSQL.

### 📄 `models/User.js`
```
import { DataTypes } from "sequelize";
import sequelize from "../config/db.js";

const User = sequelize.define("User", {
  id: { type: DataTypes.INTEGER, autoIncrement: true, primaryKey: true },
  name: { type: DataTypes.STRING, allowNull: false },
  email: { type: DataTypes.STRING, unique: true, allowNull: false },
  password: { type: DataTypes.STRING, allowNull: false },
});

export default User;
```

### 📄 models/Product.js
```
import { DataTypes } from "sequelize";
import sequelize from "../config/db.js";

const Product = sequelize.define("Product", {
  id: { type: DataTypes.INTEGER, autoIncrement: true, primaryKey: true },
  name: { type: DataTypes.STRING, allowNull: false },
  description: { type: DataTypes.STRING },
  price: { type: DataTypes.DECIMAL(10,2), allowNull: false },
});

export default Product;
```

### ⚙️ config/db.js
```
import { Sequelize } from "sequelize";

// Conexión directa a PostgreSQL (sin .env)
const sequelize = new Sequelize("buen_corte", "postgres", "tu_contraseña", {
  host: "localhost",
  dialect: "postgres",
});

export default sequelize;
```

---

## 🧩 Tecnologías utilizadas

### Frontend
- Axios
- React Router DOM

### Backend
- Express
- Sequelize (ORM para PostgreSQL)
- bcrypt (hash de contraseñas)
- jsonwebtoken (autenticación JWT)
- cors (seguridad CORS)

### Base de datos
- PostgreSQL

---

## 📁 Estructura recomendada

### Backend
```
api/
│
├── server.js
├── package.json
│
├── /config
│   └── db.js              # Conexión a PostgreSQL usando Sequelize
│
├── /models
│   ├── User.js            # Modelo del usuario
│   └── Product.js         # Modelo del producto
│
├── /controllers
│   ├── authController.js
│   └── productController.js
│
├── /routes
│   ├── auth.js
│   └── products.js
│
└── /middlewares
    └── authMiddleware.js  # Verifica el token JWT
```

### Frontend
```
client/
│
├── src/
│   ├── App.jsx
│   ├── index.js
│   ├── /pages
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   └── Home.jsx
│   ├── /components
│   │   └── Navbar.jsx
```

---

## 🔄 Ejemplo de flujo
- El usuario abre la app y ve la pantalla de Login.
- Si no tiene cuenta, se registra.
- Luego inicia sesión.
- Al ingresar, ve una lista de carnes con su nombre, descripción y precio.
- Si se desloguea, vuelve al login.

---

## 🧭 Entregable
- App funcional con login, registro y listado de productos.
- Base de datos PostgreSQL sincronizada mediante los modelos Sequelize.
- Backend y frontend conectados correctamente mediante Axios.
