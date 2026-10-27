# Proceso de Normalización - Proyecto AutoMarket

## 1. Entidad / Tabla Principal sin Normalizar
Antes de aplicar la normalización, el sistema contemplaba una única tabla conceptual donde se almacenaba toda la información del vehículo junto con los datos del vendedor y la categoría en un mismo registro redundante:

| id_auto | titulo | precio | anio | vendedor_nombre | vendedor_email | categoria | sucursal |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Sedán Familiar | 8500.00 | 2022 | Juan Pérez | juan@mail.com | Autos y Camionetas | Salta Capital |
| 2 | Camioneta 4x4 | 14000.00 | 2021 | Juan Pérez | juan@mail.com | Autos y Camionetas | Salta Capital |

## 2. Proceso de Normalización
* **Primera Forma Normal (1FN):** Se eliminan los grupos repetitivos o listas de valores múltiples. Cada celda contiene un único valor atómico (indivisible).
* **Segunda Forma Normal (2FN):** Se eliminan las dependencias parciales sobre la clave primaria. Los datos personales del vendedor (nombre, email) no dependen directamente de las características del auto, por lo que se desglosan en una tabla independiente llamada `usuarios`.
* **Tercera Forma Normal (3FN):** Se eliminan las dependencias transitivas. Los atributos que describen clasificaciones (como la categoría del vehículo) se extraen a una tabla maestra independiente (`categorias`) para evitar redundancias.

## 3. Diagrama y Listado de Tablas Resultantes (Esquema Relacional)
* **`usuarios`** (Tabla de usuarios)
  * `id` (PK, INT, Auto-incremental)
  * `nombre`, `email`, `password`, `rol`, `created_at`, `updated_at`
* **`categorias`** (Tabla maestra de categorías)
  * `id` (PK, INT, Auto-incremental)
  * `nombre_categoria`, `created_at`
* **`vehiculos`** (Tabla principal de negocio)
  * `id` (PK, INT, Auto-incremental)
  * `usuario_id` (FK hacia `usuarios.id`)
  * `categoria_id` (FK hacia `categorias.id`)
  * `titulo`, `precio`, `anio`, `kilometros`, `transmision`, `combustible`, `descripcion`, `imagen`, `created_at`, `updated_at`
* **`favoritos`** (Tabla de relación N:M entre usuarios y vehículos)
  * `id` (PK, INT, Auto-incremental)
  * `usuario_id` (FK hacia `usuarios.id`)
  * `vehiculo_id` (FK hacia `vehiculos.id`)
  * `created_at`
