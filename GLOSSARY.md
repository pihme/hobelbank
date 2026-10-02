# Glossary

Words used the same way across all repos of the family.

- **Habitat.** The world an agent wakes up in: an OCI image it may use freely as root. Provided by Hermetarium. A common noun, not a product name.
- **Inhabitant.** An agent that lives in a habitat. Fregoli is one; any agent can be one.
- **Wall.** The isolation boundary around a habitat, outside the image. *Weak*: Docker/`runc`, shared host kernel. *Strong*: Firecracker microVM, own guest kernel.
- **Supervisor.** The Go process on the host that boots a habitat behind a wall, owns its only network path and writes the I/O log.
- **I/O log.** What crosses the wall, recorded on the supervisor (`io.jsonl`). The inhabitant cannot switch it off. Not the same as the session log, which records what the model sees and lives inside the habitat.
- **Key injection.** API keys stay in the host-side ACL; Squid adds them to requests for allowed destinations, so they never exist inside the image.
- **Wall service.** Anything that runs on the wall's network path rather than inside the habitat, such as the I/O log or a future package proxy.
- **Cordis.** The plugin kernel Fregoli is built on ("everything is a plugin"), from the paper *A Programming Paradigm for Spatiotemporal Composability* (arXiv:2608.25512).
