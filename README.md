# CENTAURO: Entregable 2.4 - Navegación y Localización

Este repositorio constituye el Entregable 2.4 del proyecto CENTAURO, enmarcado en el programa Misiones Ciencia e Innovación 2023 (TransMisiones) subvencionado por el CDTI y la Agencia Estatal de Investigación (MCIN/AEI/10.13039/501100011033).

<p align="center">
  <img width="250" src="./img/LOGOS-PARTNERS-CENTAURO.jpg" /><br>
  <img width="500" src="./img/partners.png" />
</p>

## 1. Resumen

<!-- Responsable: UJI -->

<!--Contenido: contexto del proyecto, objetivos del entregable, estructura del documento -->

El proyecto CENTAURO (Investigación en nuevas tecnologías para impulsar una nueva industria nacional de soluciones autónomas robóticas) 
tiene el objetivo de desarrollar soluciones autónomas robóticas con capacidad de moverse en un entorno productivo para realizar 
distintos tipos de tareas:
* manipulación flexible y versátil
* navegación autónoma en interiores y exteriores
* interacción segura con personas

En estas tareas se requiere la localización y navegación del robot de forma autónoma y segura. En este entregable se
describen componentes hardware y software (dispositivos y algoritmos) así como su integración en el proyecto mediante ROS 2 y Docker.

## 2. Metodología

<!-- Responsable: UJI -->

<!-- Contenido: integración de algoritmos y dispositivos en ROS 2 y Docker -->

Tras casi dos décadas, [ROS (Robot Operating System)](https://www.ros.org/) se ha consolidado como el entorno de referencia para el desarrollo 
de aplicaciones robóticas de código abierto. Su amplio conjunto de herramientas, bibliotecas y funcionalidades lo ha convertido, 
desde su lanzamiento en 2007, en una plataforma esencial para ingenieros y desarrolladores en todo el mundo, desde la fase de prototipado 
hasta soluciones industriales o producto finales.

ROS 2 es la segunda versión del Sistema Operativo para Robots, desarrollado para abordar algunas de las limitaciones de la primera 
y ofrecer mejoras en varios aspectos como, por ejemplo, en lo relativo a la comunicación, compatibilidad con otros sistemas operativos 
o en cuanto al rendimiento.

El proyecto CENTAURO ha apostado por el desarrollo y la integración de componentes en el entorno 
[ROS 2 Jazzy](https://docs.ros.org/en/jazzy/index.html), la versión LTS
(Long Time Support) lanzada en Mayo de 2024 con soporte garantizado hasta el fin del proyecto (Mayo 2029).

Para facilitar el intercambio y la integración de software se usarán [contenedores](https://es.wikipedia.org/wiki/Contenedorizaci%C3%B3n_(inform%C3%A1tica)) 
(unidades estandarizadas que empaquetan una aplicación junto con todas sus dependencias). 
[Docker](https://docs.docker.com/) es una plataforma de código abierto que automatiza el despliegue 
de aplicaciones dentro de contenedores de software, proporcionando una capa adicional de abstracción y automatización 
de virtualización a nivel de sistema operativo. El entorno de ejecución de los contenedores puede configurarse para
[acceder a dispositivos hardware](https://docs.docker.com/engine/containers/run/#runtime-privilege-and-linux-capabilities), 
seleccionando adecuadamente los privilegios y permisos de los procesos.

Como alternativa a Docker puede usarse [Podman](https://podman.io/). Su principal diferenciador es su arquitectura sin demonio. 
Los comandos de Podman interactúan directamente con el tiempo de ejecución de OCI - típicamente [runc o crun](https://www.geeksforgeeks.org/devops/docker-runc-and-crun/) y el sistema. 
Cuando se ejecuta un contenedor, se convierte en un proceso hijo del usuario que lo llama, gestionado por [systemd](https://en.wikipedia.org/wiki/Systemd) - no un demonio centralizado.
Esta arquitectura permite ejecutar contenedores como un usuario normal, no root, dentro de un espacio de nombres de usuario. 
La adherencia al [Principio de Mínimo Privilegio](https://en.wikipedia.org/wiki/Principle_of_least_privilege) 
mejora significativamente la seguridad del sistema anfitrión, 
ya que un escape del contenedor sólo concedería acceso a los permisos limitados del usuario.

## 3. Repositorio de métodos LIO

<!-- Responsable: UJI -->

<!-- Contenido: descripción de los métodos LIO incluidos como submódulos en el directorio LIO/ -->

La odometría LiDAR-inercial (LIO, *LiDAR-Inertial Odometry*) estima la trayectoria del robot fusionando las nubes de puntos
de un sensor LiDAR con las medidas de una unidad inercial (IMU). Los métodos seleccionados se incluyen como
[submódulos de git](https://git-scm.com/book/en/v2/Git-Tools-Submodules) en el directorio [`LIO/`](./LIO), apuntando a
copias (*forks*) en la organización [TransMisiones-Centauro](https://github.com/TransMisiones-Centauro).
Para descargarlos junto con este repositorio:

```bash
git clone --recursive https://github.com/TransMisiones-Centauro/E2.4_Localizacion.git
# o bien, en un repositorio ya clonado:
git submodule update --init --recursive
```

| Método | Sensores | Soporte ROS 2 | Contenedor | Publicación |
|---|---|---|---|---|
| [DLIO](https://github.com/TransMisiones-Centauro/direct_lio) | LiDAR + IMU | Rama `feature/ros2` | - | IEEE ICRA 2023 |
| [FAST-LIO](https://github.com/TransMisiones-Centauro/FAST_LIO) | LiDAR + IMU | Rama `ROS2` | - | IEEE RA-L 2021, IEEE T-RO 2022 |
| [KISS-ICP](https://github.com/TransMisiones-Centauro/kiss-icp) | Solo LiDAR | Sí | [Podman / Docker](https://github.com/TransMisiones-Centauro/kiss-icp/blob/main/container/README.md) | IEEE RA-L 2023 |
| [RKO-LIO](https://github.com/TransMisiones-Centauro/rko_lio) | LiDAR + IMU | Paquete oficial Jazzy | [Podman / Docker](https://github.com/TransMisiones-Centauro/rko_lio/blob/master/container/README.md) | IEEE RA-L 2026 |
| [Super-LIO](https://github.com/TransMisiones-Centauro/Super-LIO) | LiDAR + IMU | Rama `ros2` (Humble/Iron/Jazzy) | [Podman / Docker](https://github.com/TransMisiones-Centauro/Super-LIO/blob/ros2/container/README.md) | IEEE RA-L 2026 |

La descripción detallada de cada método se encuentra en [`LIO/README.md`](./LIO/README.md).

## 4. Conclusiones

En este entregable se han revisado diferentes métodos de LIO para el proyecto Centauro, incluyendo la información necesaria para su utilización en ROS 2 con contenedores. 

## 5. Referencias

<!-- Responsable: UJI -->

<!-- Contenido: enlaces de interés -->

* [ROS 2 Documentation: Jazzy](https://docs.ros.org/en/jazzy/index.html)
* [Docker Documentation](https://docs.docker.com/)
* [Podman Documentation](https://docs.podman.io/)

