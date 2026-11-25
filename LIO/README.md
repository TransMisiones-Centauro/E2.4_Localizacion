# Métodos LIO

Descripción de los métodos de odometría LiDAR-inercial incluidos como submódulos en este directorio. Ver la [sección 3 del documento principal](../README.md#3-repositorio-de-métodos-lio) para la tabla comparativa y las instrucciones de descarga.

## 1. DLIO

[Direct LiDAR-Inertial Odometry](https://github.com/TransMisiones-Centauro/direct_lio)
([paper](https://ieeexplore.ieee.org/document/10160508), [arXiv](https://arxiv.org/abs/2203.03749))
es un algoritmo LIO ligero que construye trayectorias en tiempo continuo con un enfoque de grueso a fino
para corregir con precisión la distorsión por movimiento. Es la evolución de DLO
([Direct LiDAR Odometry](https://github.com/vectr-ucla/direct_lidar_odometry)).

* Compatible con LiDAR Ouster, Velodyne, Hesai y Livox, con entradas `sensor_msgs/PointCloud2` e `sensor_msgs/Imu`.
* Requiere que LiDAR e IMU estén correctamente sincronizados en el tiempo; se recomienda un LiDAR con IMU integrada.
* La calibración extrínseca e intrínseca se configura en `cfg/dlio.yaml`.
* La rama principal es para ROS 1; el soporte de ROS 2 está en la rama `feature/ros2`.

## 2. FAST-LIO

[FAST-LIO](https://github.com/TransMisiones-Centauro/FAST_LIO)
([FAST-LIO](https://arxiv.org/abs/2010.08196), [FAST-LIO2](./FAST_LIO/doc/Fast_LIO_2.pdf))
fusiona los puntos LiDAR con los datos de la IMU mediante un filtro de Kalman extendido iterado y fuertemente acoplado,
lo que lo hace robusto ante movimientos rápidos y entornos ruidosos o degenerados.

* FAST-LIO2 realiza odometría directa (*scan-to-map*) sobre los puntos en bruto, sin extracción de características,
  y mantiene el mapa de forma incremental con [ikd-Tree](https://github.com/hku-mars/ikd-Tree), superando los 100 Hz.
* Admite LiDAR rotativos (Velodyne, Ouster) y de estado sólido (Livox Avia, Horizon, MID-70, MID-360), así como IMU externa.
* Funciona en plataformas ARM (Khadas VIM3, NVIDIA TX2, Raspberry Pi 4B).
* Con sensores Livox necesita `livox_ros_driver` y el tipo de mensaje `CustomMsg`, que incluye la marca de tiempo de cada punto.
* La rama principal es para ROS 1; el soporte de ROS 2 está en la rama `ROS2`.

## 3. KISS-ICP

[KISS-ICP](https://github.com/TransMisiones-Centauro/kiss-icp)
([paper](https://ieeexplore.ieee.org/document/10015694))
es una odometría basada **solo en LiDAR** (sin IMU) que utiliza ICP punto a punto y funciona en la mayoría
de los casos sin ajustar parámetros. Se incluye como referencia frente a los métodos LiDAR-inerciales.

* Disponible como paquete de Python (`pip install kiss-icp`) y como paquete de ROS 2 (`colcon build --packages-select kiss_icp`).
* El soporte de ROS 1 está obsoleto desde la versión v0.3.0.
* El directorio `2D_LiDAR` contiene una configuración y scripts de ejemplo para ejecutarlo sobre datos de LiDAR 2D.

## 4. RKO-LIO

[RKO-LIO](https://github.com/TransMisiones-Centauro/rko_lio)
([paper](https://doi.org/10.1109/LRA.2026.3685966), [arXiv](https://arxiv.org/abs/2509.06593))
es un sistema LIO robusto que no requiere modelado específico del sensor, validado en distintas plataformas y entornos.

* Disponible como paquete oficial de ROS 2 (Humble, Jazzy, Kilted, Lyrical, Rolling): `apt install ros-$ROS_DISTRO-rko-lio`.
* También como paquete de Python (`pip install "rko_lio[all]"`), capaz de procesar directamente *rosbags* de ROS 1 y ROS 2.
* Requiere las transformaciones extrínsecas de la IMU y el LiDAR respecto a la base del robot, leídas del árbol TF o de un fichero de configuración.
* Instrucciones de construcción y ejecución con Podman o Docker: [`RKO-LIO/podman/README.md`](./RKO-LIO/podman/README.md).

## 5. Super-LIO

[Super-LIO](https://github.com/TransMisiones-Centauro/Super-LIO)
([paper](https://ieeexplore.ieee.org/document/11347459), [arXiv](https://arxiv.org/abs/2509.05723))
es un sistema LIO robusto y eficiente para navegación autónoma a gran escala y en tiempo real. Introduce una estrategia
de mapeado compacta y estructurada que permite una búsqueda de correspondencias predecible y una estimación de estado estable,
con menor consumo de recursos que otros métodos del estado del arte.

* Soporta ROS 2 (Humble, Iron, Jazzy) en la rama `ros2` y ROS 1 Noetic en la rama `ros1`.
* Compatible con plataformas x86_64 y ARM64.
* Instrucciones de construcción (incluyendo Livox-SDK2 y `livox_ros_driver2`) y ejecución con Podman o Docker:
  [`Super-LIO/podman/README.md`](./Super-LIO/podman/README.md).
