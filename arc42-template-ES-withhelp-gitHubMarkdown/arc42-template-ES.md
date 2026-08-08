---
date: Agosto 2026
title: Arquitectura de Software - Sistema ERP
---

# 1. Introducción y Metas

## 1.1 Objetivo del Sistema ERP
El sistema ERP (Enterprise Resource Planning) tiene como objetivo centralizar, automatizar e integrar los procesos clave del negocio para optimizar la gestión operativa, mejorar la toma de decisiones y garantizar la trazabilidad de la información en tiempo real.

## 1.2 Requisitos de Negocio (Módulo de Inventario y Compras)
* **Gestión de Productos:** Registro, actualización y categorización de productos y materias primas en el catálogo centralizado.
* **Control de Stock y Niveles:** Monitoreo automatizado de los niveles de inventario para evitar desabastecimiento.
* **Gestión de Proveedores:** Mantenimiento de la información de proveedores y asociación con los productos que suministran.
* **Integración Contable:** Sincronización continua de transacciones y asientos contables con el Sistema Contable Externo para mantener la consistencia financiera.

---

# 2. Restricciones de Arquitectura

## Decisiones Tecnológicas Adoptadas
* **Frontend:** Single-Page Application (SPA) desarrollada con **React / JavaScript** para ofrecer una interfaz dinámica y con tiempos de respuesta óptimos.
* **Backend:** API Monolítica desarrollada en **Java con Spring Boot**, encargada de la lógica de negocio centralizada y la seguridad de los servicios.
* **Base de Datos:** **PostgreSQL**, una base de datos relacional robusta elegida por su soporte para transacciones ACID e integridad referencial.
* **Integración y Diagramación:** Adopción del estándar **C4 Model** e implementación con **PlantUML** para la documentación técnica en el repositorio de GitHub.

---

# 3. Alcance y Contexto del Sistema

## Contexto del Sistema (C1)
El Sistema ERP actúa como el núcleo operacional del negocio, interactuando directamente con los roles de gestión e integrándose con sistemas financieros externos.

![Diagrama de Contexto](https://raw.githubusercontent.com/Estela1404/erp-software-architecture/main/docs/images/c1_context.png)

### Explicación del Contexto:
* **Gestor de Inventario:** Interactúa con el ERP para registrar y consultar productos en el catálogo.
* **Administrador de Compras:** Gestiona los proveedores y asocia la oferta de productos correspondiente.
* **Sistema ERP:** Procesa la lógica operacional y almacena el catálogo de datos.
* **Sistema Contable Externo:** Recibe en tiempo real o por lotes las transacciones y asientos contables generados por el ERP.

---

# 5. Vista de Bloques de Construcción

## Vista de Contenedores (C2)
Esta vista hace "zoom" dentro de la frontera del Sistema ERP para detallar las tecnologías y contenedores principales que componen la solución.

![Diagrama de Contenedores](https://raw.githubusercontent.com/Estela1404/erp-software-architecture/main/docs/images/c2_container.png)

### Responsabilidad de los Contenedores:
* **Single-Page Application (React):** Interfaz gráfica accesible desde el navegador del usuario. Maneja la presentación y captura de datos.
* **API Monolítica (Spring Boot):** Recibe solicitudes HTTPS/JSON, aplica las reglas de negocio, valida los formularios y coordina el almacenamiento de datos.
* **Base de Datos (PostgreSQL):** Almacena de manera persistente las tablas de productos, proveedores y sus relaciones comerciales.

---

# 6. Vista Dinámica (Runtime)

## Escenario Crítico: Registrar un Nuevo Producto
Este flujo describe la interacción entre los componentes al ejecutar la historia de usuario: *"Como gestor de inventario, quiero registrar nuevos productos..."*.

![Diagrama de Secuencia](https://raw.githubusercontent.com/Estela1404/erp-software-architecture/main/docs/images/sequence.png)

### Flujo de Ejecución:
1. El **Admin/Gestor** ingresa la información en el formulario web y hace clic en "Guardar".
2. La **SPA** envía una petición `POST /api/productos` con el cuerpo en formato JSON a la **API**.
3. La **API** ejecuta la validación de los campos obligatorios (nombre, descripción, unidad).
4. Si los datos son válidos, la **API** realiza la inserción (`INSERT`) en la **Base de Datos**.
5. La **Base de Datos** responde confirmando la creación y asignando un ID.
6. La **API** retorna una respuesta `201 Created` a la **SPA**, la cual muestra un mensaje de éxito al usuario.

---

# 7. Vista de Despliegue

## Infraestructura en la Nube
El sistema está diseñado para un despliegue simple y escalable en la nube:
* **Frontend (SPA):** Alojado en un CDN o servicio de archivos estáticos para minimizar la latencia.
* **Backend (API):** Desplegado como un contenedor Docker en un servidor de aplicaciones.
* **Base de Datos:** Instancia administrada de PostgreSQL con respaldos automáticos y conexión restringida a la API.

---

# 8. Conceptos Transversales

## Modelo Entidad-Relación (MER)
Representación de la estructura de persistencia para el módulo de inventarios y proveedores.

![Modelo Entidad Relación](https://raw.githubusercontent.com/Estela1404/erp-software-architecture/main/docs/images/mer.png)

---

# 10. Glosario

* **Producto:** Bien o artículo comercializable gestionado en el catálogo del ERP, caracterizado por un código, nombre, descripción y unidad de medida.
* **Proveedor:** Entidad externa (persona jurídica o natural) que suministra productos o materias primas a la empresa.
* **Producto_Proveedor:** Entidad intermedia que representa la relación comercial entre un producto y los proveedores que lo ofrecen, incluyendo el precio unitario acordado.
* **Sistema ERP:** Plataforma de software para la planificación y gestión integral de los recursos empresariales.
* **Single-Page Application (SPA):** Aplicación web que se carga en una sola página de navegador para brindar una experiencia de usuario fluida.
