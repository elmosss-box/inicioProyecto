# 🚀 Proyecto Final: Infraestructura, Contenedores y Alta Disponibilidad

<img width="1024" height="822" alt="image" src="https://github.com/user-attachments/assets/7c481b94-6c72-43ce-80aa-a0cedf6cdea8" />


## 📖 Descripción del Proyecto
Este repositorio contiene la documentación y planificación de nuestro proyecto final de curso. El objetivo principal es diseñar, desplegar y administrar una infraestructura tecnológica completa que integre almacenamiento centralizado, virtualización, gestión de contenedores y salida segura a Internet.

## 👥 Equipo de Trabajo
* **Mossab**
* **Franco**
* **Yadir**

---

## 🏗️ Topología y Arquitectura del Sistema

Nuestra infraestructura se divide en tres bloques físicos/lógicos principales, diseñados para separar el cómputo del almacenamiento y asegurar el despliegue de servicios web.

### 1. Servidor SUPER MICRO (Búnker de Almacenamiento)
Actúa como el pilar de nuestros datos. 
* **Sistema Operativo:** Instalación de **TrueNAS**.
* **Estructura de Discos:** Creación de 3 RAIDs lógicos (Pools):
    * **Backup:** Destinado a copias de seguridad.
    * **Producción:** Almacenamiento activo para las máquinas virtuales.
    * **Libre:** Espacio de reserva para futuros despliegues.

### 2. Servidor HP (Nodo Principal de Cómputo)
Actualmente es el motor principal de la infraestructura, asumiendo la carga de trabajo tras la migración de las máquinas virtuales (V.M.) del servidor heredado.
* **Servicios Alojados:**
    * **Servidor Web Propio:** Despliegue de nuestras aplicaciones.
    * **Coolify:** Plataforma para la gestión simplificada de despliegues (PaaS).
    * **Bases de Datos:** Contenedores de **MySQL** gestionados mediante Docker a través de Coolify.
* **Red y Seguridad:** La salida a Internet está protegida y enrutada a través de **Cloudflare**.

### 3. Servidor DELL (Nodo Heredado / xx) y Clúster
* **Estado actual:** Inoperativo / Fase de migración.
* **Acción:** Extracción de sus Máquinas Virtuales (V.M.) y reubicación en el servidor HP.
* **Estrategia de Quórum:** Implementación de **QDevice Config** para establecer un clúster de 3 nodos (HP, Dell y QDevice), buscando sentar las bases lógicas para la Alta Disponibilidad (HA).

---

## 🛠️ Stack Tecnológico

| Categoría | Tecnología / Herramienta | Función en el proyecto |
| :--- | :--- | :--- |
| **Almacenamiento** | TrueNAS (ZFS) | Gestión NAS y RAIDs |
| **Cómputo / Virtualización** | Proxmox VE (o similar) | Hipervisor para VMs |
| **Orquestación** | Coolify / Docker | Despliegue de contenedores (MySQL) |
| **Redes / Proxy** | Cloudflare | DNS, Proxy Inverso y Seguridad |
| **Alta Disponibilidad** | Corosync / QDevice | Gestión del quórum del clúster |

---

## ⚠️ Desafíos Técnicos y Mitigaciones

Como equipo, hemos analizado críticamente nuestra arquitectura e identificado los siguientes retos operativos:

1.  **Fallo de Hardware (Servidor DELL):** La caída del servidor DELL nos ha obligado a pivotar. La "Alta Disponibilidad" física real está comprometida al depender del cómputo exclusivo del HP.
2.  **Solución Propuesta:** Nuestra estrategia pasa por enfocar los esfuerzos en la **Recuperación ante Desastres (DR)**. Aseguraremos que el almacenamiento (Super Micro) esté desvinculado del cómputo (HP) para garantizar que, ante cualquier fallo físico del nodo principal, no haya pérdida de datos de producción ni de backups.

---
<img width="862" height="692" alt="image" src="https://github.com/user-attachments/assets/86a3f542-d2c9-48ac-9587-19c92decf69c" />

