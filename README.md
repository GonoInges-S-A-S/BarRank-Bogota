# BarRank Bogota

API REST en Node.js para gestionar y consultar rankings de bares en Bogotá.



## Descripción

BarRank Bogota es una API REST que permite:

- Añadir nuevos bares  
- Consultar y mostrar ranking por calificación  
- Editar o eliminar registros  
- Obtener lista de bares ordenada por rating

---

## Requisitos
- Node.js v20.24.0
- npm o yarn  
- Base de datos MySql
- Nuxt js, tailwind

---

## Instalación

```bash

git clone https://github.com/GonoInges-S-A-S/BarRank-Bogota.git

# 2. Entra al directorio
cd BarRank-Bogota

# 3. Instala dependencias
npm install
# o con yarn:
yarn install
```
Nuxt:

```bash
# npm
npm install

# pnpm
pnpm install

# yarn
yarn install
# bun
bun install
```
## Configuración
Crea un archivo .env en la raíz con las variables:

```bash
PORT=xxxx
DB_URI=tu_conexion_a_base_de_datos
API_KEY=clave_secreta_opcional
Ajusta según tu entorno local o de producción.
```

## Ejecución

```bash
# Inicia en modo desarrollo
npm run dev
# o
yarn dev
La API correrá en http://localhost:3000 (o el puerto del .env).
```

## Endpoints

| Método | Ruta               | Descripción                         |
| :----: | ------------------ | ----------------------------------- |
|   GET  | `/bars`            | Lista todos los bares               |
|   GET  | `/bars/:id`        | Muestra un bar específico por ID    |
|  POST  | `/bars`            | Crea un nuevo bar                   |
|   PUT  | `/bars/:id`        | Actualiza los datos de un bar       |
| DELETE | `/bars/:id`        | Elimina un bar                      |
|   GET  | `/bars/top`        | Bares ordenados por calificación    |
|   GET  | `/bars/city/:city` | Bares filtrados por ciudad (Bogotá) |


## Ejemplo
Crear un bar
```bash
curl -X POST http://localhost:3000/bars \
  -H "Content-Type: application/json" \
  -d '{
        "name": "Bar La Candelaria",
        "rating": 4.8,
        "location": "Bogotá"
      }'
```




