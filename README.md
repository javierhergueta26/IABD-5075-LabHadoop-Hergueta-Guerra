# Práctica de Laboratorio: Despliegue, Tolerancia a Fallos y Cómputo Distribuido en Apache Hadoop

## Autoría
- **Estudiante:** Javier Hergueta Guerra
- **Fecha:**  22 de Noviembre de 2026
- **Entorno:** Apache Hadoop 3.3.6 sobre Docker Compose

---

## Ficha Resumen de la Arquitectura Desplegada

El clúster distribuido se ha desplegado sobre una red puente interna de Docker (`docker_hadoop-net`), desacoplando la capa de almacenamiento (**HDFS**) de la capa de gestión de recursos y procesamiento (**YARN**):

| Contenedor / Servicio | Rol en el Clúster | Puerto / Interfaz Web | Configuración Principal |
| :--- | :--- | :--- | :--- |
| **`namenode`** | Maestro HDFS (*Namespace* y metadatos) | `http://localhost:9870` | Tamaño de bloque: `64 MiB` (`67108864` B) |
| **`datanode1`** | Nodo trabajador HDFS | Interno (`9864` / `9866`) | Factor de replicación: `dfs.replication=2` |
| **`datanode2`** | Nodo trabajador HDFS | Interno (`9864` / `9866`) | Factor de replicación: `dfs.replication=2` |
| **`datanode3`** | Nodo trabajador HDFS | Interno (`9864` / `9866`) | Factor de replicación: `dfs.replication=2` |
| **`resourcemanager`** | Maestro YARN (Planificador de trabajos) | `http://localhost:8088` | *Capacity Scheduler* |
| **`nodemanager`** | Trabajador YARN (Ejecución de contenedores) | Interno (`8042`) | Memoria: `2048 MB` RAM / `2 vCores` |

---

## Estructura del Repositorio

```text
├── docker/
│   ├── config
│   └── docker-compose.yml
├── evidencias/
│   ├── 01_namenode_ui_9870.png
│   ├── 02_hdfs_fsck_bloques.png
│   ├── 03_simulacion_fallo_datanode2.png
│   └── 04_yarn_wordcount_8088.png
├── informe/
│   └── Hergueta_Guerra_InformeLabHadoop.pdf
└── README.md
```

---

## Instrucciones de Arranque y Cierre

1. **Desplegar y arrancar el clúster** desde el directorio `docker/`:
   ```bash
   cd docker
   docker compose up -d
   ```

2. **Acceder al terminal del nodo maestro (`namenode`)** para operar con HDFS y lanzar trabajos MapReduce:
   ```bash
   docker exec -it namenode bash
   ```

3. **Detener el clúster y eliminar los volúmenes temporales** al finalizar la práctica:
   ```bash
   cd docker
   docker compose down -v
   cd ..
   ```