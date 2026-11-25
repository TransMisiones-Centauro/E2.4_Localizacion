# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is a **documentation deliverable**, not a buildable software project: Entregable 2.4 ("Navegación y Localización") of the CENTAURO project (CDTI / AEI, Misiones Ciencia e Innovación 2023). The deliverable text is written in **Spanish** in Markdown. Keep new prose in Spanish unless told otherwise.

There is no build, lint or test step at the top level. The source code lives in git submodules under `LIO/` and is built inside containers, not in this repo.

## Layout

- `README.md`: the deliverable document itself. Numbered sections (Resumen, Metodología, Repositorio de métodos LIO, Conclusiones, Referencias). HTML comments such as `<!-- Responsable: UJI -->` and `<!-- Contenido: ... -->` mark who owns each section and what it should contain. Keep them when editing.
- `Resources.md`: a curated list of links to odometry and SLAM resources (3D LiDAR, visual, 2D LiDAR, SLAM). It was split out of the README.
- `LIO/`: git submodules, one per LiDAR-Inertial Odometry method being evaluated: `DLIO`, `FAST_LIO`, `RKO-LIO`, `Super-LIO`, `kiss-icp`. They are all forks under the `TransMisiones-Centauro` GitHub org. `LIO/README.md` (in Spanish) describes each method. Section 3 of the top-level README keeps only the introduction and a comparison table.
- `img/`: partner logos used in the README header.

## Submodules

```bash
git submodule update --init --recursive   # fetch all LIO method sources
git submodule status                       # check pinned commits
```

Changes to method code belong in the submodule's own fork repository. In this repo, only commit the updated submodule pointer.

## Container workflow (per LIO method)

The target platform is **ROS 2 Jazzy**, packaged with **Podman** (Docker also works). Some submodules have a `container/README.md` with the recipe: `LIO/DLIO/container/README.md`, `LIO/FAST_LIO/container/README.md`, `LIO/kiss-icp/container/README.md`, `LIO/RKO-LIO/container/README.md` and `LIO/Super-LIO/container/README.md`. The recipes follow the same pattern:

1. Start from `docker.io/osrf/ros:jazzy-desktop-full` and install the dependencies interactively, or `colcon build` the package in `/ros2_ws`.
2. Save the result with `podman commit` and push it to `ghcr.io/transmisiones-centauro/<method>:latest`.
3. Run it with X11 forwarding for RViz:
   ```bash
   xhost +local:$USER
   podman run -it --rm --name <method> \
       --env="DISPLAY=$DISPLAY" --env="QT_X11_NO_MITSHM=1" \
       --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
       --device /dev/dri \
       ghcr.io/transmisiones-centauro/<method>:latest
   ```

When you document a new method, follow this same `container/README.md` structure ("Create software image" / "Run container"). `LIO/RKO-LIO/container/README.md` is the most complete example: it uses `$ENGINE` so the same commands work with Podman or Docker.
