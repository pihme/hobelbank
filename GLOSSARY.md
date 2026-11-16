# Hobelbank

The words used the same way across the workshop.

## Language

**Hermetarium**:
The product that starts a habitat.
_Avoid_: habitat

**Habitat**:
One running place: an image, a wall, and the network path. A common noun, not a product name.
_Avoid_: box, Hermetarium

**Image**:
The OCI image. It is the Linux the inhabitant sees.
_Avoid_: root disk, habitat

**Wall**:
The isolation around the image. A weak wall shares the host kernel. A strong wall is a Firecracker microVM with its own kernel.
_Avoid_: sandbox

**Inhabitant**:
The process that lives inside the image. Fregoli is one.
_Avoid_: assistant

**Supervisor**:
The control plane that starts and stops a habitat, owns the network path, and writes the I/O log.
_Avoid_: orchestrator

**Gate**:
Squid, the only network path between the habitat and the outside. On the strong wall it is not the Firecracker container.
_Avoid_: helper

**I/O log**:
The record of traffic that crosses the wall. Not the session log, which records what the model sees and lives inside the habitat.
_Avoid_: access log

**Key injection**:
API keys stay outside the image. The gate adds them to requests for allowed destinations.

**Wall service**:
Anything that runs on the wall's network path rather than inside the habitat, such as the I/O log or a future package proxy.

**Wall agent**:
A wall service that uses a model. Trusted more than an inhabitant, and allowed to do less: no shell and no tools.

**Cordis**:
The plugin kernel Fregoli is built on. From the paper *A Programming Paradigm for Spatiotemporal Composability* (arXiv:2608.25512).

## Strong wall

**Firecracker container**:
The container on the strong wall that holds the microVM.
_Avoid_: helper

**Firecracker scripts**:
The strong-wall scripts in Hermetarium's `firecracker-helper/` directory. They are not the Firecracker container.
_Avoid_: helper, Firecracker helper

## Inside the habitat

**Porter**:
The HTTP adapter in the inhabitant image. It listens on port 8080 and carries the operator's question to the inhabitant's CLI.
_Avoid_: helper, host program

## Host programs

**Build image**:
The small Alpine image the supervisor uses for its own jobs, outside any habitat. In Hermetarium the Dockerfile is `supervisor/helper/Dockerfile`.
_Avoid_: helper
