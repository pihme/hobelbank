# Hobelbank

A map of the workshop: how Hermetarium, Fregoli, grok-budget-mcp, claude-budget-mcp and the ideas that do not have a repo yet fit together.

This repo holds no code. It explains the pieces, where each one sits, and how they are meant to connect. Each piece's own repo stays the source of truth for what it does; this map only links them up.

## The problem

Agents come with a trade-off between autonomy and control. To do their best work they need as many rights as possible, and for coding agents shell access matters most. But an agent with a shell can step around other safety measures, for example by calling the Azure CLI directly instead of going through the Azure MCP server.

Many approaches limit the tools an agent may use. In practical work, though, it keeps turning out that nobody can predict which tool an agent will need to solve a problem. That argues for giving it as many rights as possible, and doing that without any check would be reckless.

So control is still needed: control the agent cannot get around, and control that needs no human. It decides on its own and never waits for anyone. The wall blocks by itself, and you hear about it afterwards; you are told, not asked to approve.

## The picture

The pieces form three layers around one rule: **an agent may do anything inside its world, and everything that controls it lives outside, where it cannot reach.**

1. **Habitat.** Hermetarium boots an agent inside an OCI image, behind a wall. Inside, the agent is root.
2. **Inhabitants.** Agents that live in a habitat and build software there. Fregoli is one of them; others can move in too.
3. **Wall services.** Everything that sits on the wall, on the only network path: the I/O log today, a package proxy with a scanner later.

Around the habitat sit tools the agents use, like grok-budget-mcp and claude-budget-mcp, and guards on the way to GitHub, like Git Warden.

![The essential pieces: a supervisor starts an inhabitant behind a wall, and all its traffic leaves through Squid](docs/images/overview.svg)

The full map with every piece follows after the table below.

## The pieces

| Piece | Layer | State | Where |
| --- | --- | --- | --- |
| **Hermetarium** | Habitat | Released (`hermetarium` v0.4.0, `porter` v0.1.0) | [pihme/hermetarium](https://github.com/pihme/hermetarium) |
| Sandbox logging | Wall service, part of Hermetarium | Built: Squid access log turned into `io.jsonl`, metadata only | [Hermetarium](https://github.com/pihme/hermetarium) |
| **Fregoli** | Inhabitant | Released (v0.3.2) | [pihme/fregoli](https://github.com/pihme/fregoli) |
| **grok-budget-mcp** | Tool for agents | Released (v1.0.0) | [pihme/grok-budget-mcp](https://github.com/pihme/grok-budget-mcp) |
| **claude-budget-mcp** | Tool for agents | Released (v0.1.0), not yet tested live | [pihme/claude-budget-mcp](https://github.com/pihme/claude-budget-mcp) |
| Package proxy and AI security scanner | Wall service (or tool inside) | Idea with an architecture sketch | [Ideengarten](https://github.com/pihme/ideengarten/blob/main/wiki/security-scanner.md) |
| Tree-shaker | Tool or inhabitant | Idea | [Ideengarten](https://github.com/pihme/ideengarten/blob/main/wiki/tree-shake.md) |
| Git Warden | Guard between agents and GitHub | Spec draft | [Ideengarten](https://github.com/pihme/ideengarten/blob/main/wiki/git-warden.md) |

Ideengarten is private, so those links only work for collaborators.

![The full map: habitat with Fregoli and other inhabitants, Squid with a package proxy, a watching agent next to the supervisor, and Git Warden in front of GitHub](docs/images/map.svg)

Dashed lines and boxes are not built yet.

## How they connect

- **Fregoli lives in Hermetarium.** Fregoli's own image serves its web app, and its docs recommend Hermetarium as the place to run it, but do not require it. It has not moved in yet: the server only binds `127.0.0.1` ([fregoli#1](https://github.com/pihme/fregoli/issues/1)).
- **Keys never enter the habitat.** Hermetarium's Squid injects API keys from the host-side ACL. grok-budget-mcp inside a habitat would work the same way, with a placeholder `auth.json` in the image and the real token added on the wall.
- **claude-budget-mcp is the same tool for Claude Code.** It reads the 5-hour and weekly limits from Claude Code's own login and never refreshes the token, so everything said here about grok-budget-mcp applies to it too. Inside a habitat its placeholder would be a `.credentials.json`.
- **grok-budget-mcp has two possible places.** Inside a habitat, an agent asks how much of its weekly budget is left. Next to the supervisor, a watching agent could put a habitat to sleep when the budget is empty and wake it after the reset. Hermetarium has no sleep and wake yet, and keeping a changed world is still open ([hermetarium#9](https://github.com/pihme/hermetarium/issues/9)).
- **The scanner shares the wall.** Every package fetch already crosses Squid. A package proxy there would store, scan and re-serve what agents download, using the same choke point and log as the I/O log.
- **The tree-shaker uses what the proxy saw.** It finds dependencies that are barely used and rewrites that code, so less foreign code comes in. It could work from exactly what the proxy served.
- **Git Warden keeps GitHub credentials out of habitats.** Agents push to the warden instead of GitHub; only the warden holds a write credential. That is the same idea as key injection, applied to Git.

## Design philosophy

1. **Keep it simple.** Each piece does one job with as few moving parts as it can. A small piece is easier to check, to replace and to trust.
2. **Set and forget.** You set up an agent's world once, and after that the agent never stops to ask you for permission. That does not mean everything is allowed: the decisions live outside, where the agent cannot reach them. Inside the habitat the agent may do anything. What leaves it is checked on the way out, by the wall and its log today and by Git Warden later. When something looks wrong, the agent does not ask for an exception: the wall says no, or Git Warden holds the change and alerts you.
3. **Batteries included, assembly required.** Together the pieces make a working setup, but it is meant to be adapted to your own needs: your images, your allowlists, your agents.
4. **Bricks, not a stack.** The pieces fit together, and each one can be swapped for something else that does the same job: another habitat, another inhabitant, another proxy.
5. **The good side of Pareto.** Aim for the solution that solves 80% of the problems with a fraction of the effort, and leave the rest out rather than make everything heavier.
6. **The show must go on.** A check never stops the job. When Git Warden finds a suspicious change, it does not mirror it to GitHub and warns you, but the agent keeps working in its habitat. You look at the warning when it suits you, and the mirror catches up later. What leaves may be held back; the work itself goes on.

## Shared principles

- Freedom inside, control outside. The agent cannot switch off the wall or its log.
- One logged network path. If it cannot be set up, the habitat does not start.
- Secrets stay outside the image and are added on the way out.
- Things are reversible: habitats can be thrown away, Fregoli's plugins can be unloaded.
- Less unchecked foreign code: agents write most of their own code, and what they download is logged.

## Neighbours

Projects outside this family that solve parts of the same problem are compared in [docs/neighbours.md](docs/neighbours.md).

## Vision

_Not written yet._

## More

- [GLOSSARY.md](GLOSSARY.md): the words used across the family (habitat, inhabitant, wall, supervisor).
- [CONTRIBUTING.md](CONTRIBUTING.md): issues are welcome.
