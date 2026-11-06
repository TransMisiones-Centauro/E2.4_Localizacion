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

## 3D LiDAR

* [KISS-ICP](https://github.com/TransMisiones-Centauro/kiss-icp) ([paper](https://ieeexplore.ieee.org/document/10015694))

* FAST LIO
  * [fast_lio_ros2](https://github.com/TransMisiones-Centauro/FAST_LIO_ROS2/pkgs/container/fast_lio_ros2)
  * [fast_lio_slam_ros2](https://github.com/TransMisiones-Centauro/FAST_LIO_SLAM_ros2/pkgs/container/fast_lio_slam_ros2)
  * [mars_fast_lio](https://github.com/TransMisiones-Centauro/hku-mars_FAST_LIO/pkgs/container/hku-mars_fast_lio)

* Point-LIO
  * [Point-LIO for Unitree Lidar L1](https://github.com/TransMisiones-Centauro/point_lio_unilidar)
  * [ROS2 version of Point-LIO](https://github.com/TransMisiones-Centauro/Point-LIO/tree/ros2)
 
* GLO
  * [GLO - General LiDAR-only Odometry](https://github.com/TransMisiones-Centauro/GLO) ([paper](https://ieeexplore.ieee.org/document/10891038))

## Visual

* [Visual Odometry with the ZED Stereo Camera](https://kapernikov.com/visual-odometry-with-the-zed-stereo-camera/)
* [ORB_SLAM2 with ZED stereo camera](https://github.com/Pooja71/Visual-SLAM-using-Stereo-Camera-for-Autonomous-Vehicles)

## 2D LiDAR

* [Simple-2D-LiDAR-Odometry](https://github.com/TransMisiones-Centauro/Simple-2D-LiDAR-Odometry)
* [rf2o_laser_odometry](https://github.com/TransMisiones-Centauro/rf2o_laser_odometry)
* [Does kiss-ICP work with 2D LIDARs ?](https://github.com/PRBonn/kiss-icp/issues/37)

# SLAM

* RTAB-Map
  * [Real-Time Appearance-Based Mapping](https://introlab.github.io/rtabmap/)
  * [Example with DepthAI OAK-D](https://wiki.ros.org/rtabmap_ros/Tutorials/StereoHandHeldMapping#Bring-up_example_with_DepthAI_OAK-D)
  * [Example with Zed/Zed-Mini/Zed2 cameras](https://wiki.ros.org/rtabmap_ros/Tutorials/StereoHandHeldMapping#Bring-up_example_with_Zed.2FZed-Mini.2FZed2_cameras)
