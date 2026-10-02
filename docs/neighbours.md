# Neighbours

Projects outside this family that solve parts of the same problem. Technical comparison only; figures are as of October 2026.

## NVIDIA OpenShell

[OpenShell](https://github.com/NVIDIA/OpenShell) (Apache-2.0, Rust) is a runtime for running autonomous agents in isolated sandboxes. Its architecture is close to Hermetarium's: a wall around the agent, one controlled way out through a proxy, credentials kept outside, an audit log.

Where it differs from Hermetarium:

- The agent never runs as root. Landlock and seccomp restrict it inside, on top of the wall. Hermetarium lets the agent be root and relies on the wall alone.
- Egress rules are OPA/Rego policies per host, port and calling binary, with L7 inspection (REST, GraphQL, MCP). That needs TLS termination with a per-sandbox CA. Hermetarium uses a Squid ACL and does not break TLS.
- It has a control plane (gateway with mTLS/OIDC, multi-tenant, SDKs, TUI) and an OCSF audit log. Hermetarium is a local tool without a control plane or telemetry.
- Isolation options: Docker, Podman, Kubernetes, libkrun microVMs. Hermetarium: Docker/`runc` and Firecracker.

Possible touch points: a package proxy or scanner as OpenShell middleware, Fregoli as an inhabitant of an OpenShell sandbox.

## sandcastle

[sandcastle](https://github.com/mattpocock/sandcastle) (MIT, TypeScript) orchestrates coding agents: one Git worktree or branch per task, the agent in a Docker/Podman container or a Vercel sandbox, commits merged back. It runs agents in parallel, driven by GitHub issues.

It is a workflow tool, not a security wall: there is no egress control, API keys reach the container as environment variables, and the host's `.git` is bind-mounted read-write ([sandcastle#1010](https://github.com/mattpocock/sandcastle/issues/1010)). The two complement each other. sandcastle answers "how do many agents work in parallel", Hermetarium "how does an agent not carry anything out". sandcastle has a documented sandbox-provider interface, so Hermetarium could in principle be one.

## DeepSeek Harness

[DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) (MIT) is the agent harness built on Cordis, the same plugin kernel Fregoli uses. Fregoli uses the Cordis library directly and does not wrap the harness. Its `ctx.sandbox` confines subprocesses on the real machine, the opposite of a habitat, where the agent lives inside.
