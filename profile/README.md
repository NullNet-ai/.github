<div align="center">

# NullNet

### No standing connections. No attack surface.

**Networks that don't exist until they're needed.**

[nullnet.ai](https://nullnet.ai) · *silence by default — every connection is earned, then erased*

</div>

---

## The idea

Most networks are **default-open**: everything can talk to everything, and a firewall claws the attack surface back down rule by rule. The surface starts as the *entire network*.

**NullNet flips the logic.** By default, **nothing can talk to anything**. A service can't reach another until the moment it genuinely needs to — the connection is built right then, just for that one conversation, and torn down once it's done.

> There is no pre-built network waiting to be attacked — because there is no pre-built network at all.

Break in somewhere and there's no internal network to roam. The only paths that exist are the narrow, temporary ones carrying legitimate traffic right now.

## How it works

A request comes in. The server walks the **entire chain** it will travel — if A needs B, and B needs C, that one request opens every link *atomically*, so the full path is ready at once. Each link is a dedicated **VXLAN tunnel**, private between two machines and alive only while in use; go idle past the timeout and the server tears it down.

Three components coordinate over a **gRPC control plane**:

| Component | Role |
| --- | --- |
| 🧠 **nullnet-server** | The brain — the only piece that sees the whole picture. Holds the topology of who *may* talk to whom, and decides when to build each connection. |
| 🛰️ **nullnet-client** | The agent on each machine. Announces local services and watches for them reaching out — pausing the first request just long enough to have the path built. |
| 🚪 **nullnet-proxy** | The front door. The ingress edge for outside traffic, asking the server to build whatever's needed to deliver each request. |

<div align="center">
<img src="https://raw.githubusercontent.com/nullnet-ai/nullnet/main/docs/architecture.png" alt="NullNet architecture" width="720">
</div>

## Why it's different

- ⚡ **On-demand tunnels** — connections are blueprints, not roads.
- ⏱️ **Millisecond setup** — paths built the instant a request appears, erased when idle.
- 🎯 **Millisecond attack surface** — nothing standing to scan, pivot through, or persist on.
- 🔒 **A private network per client** — never a shared fabric.
- 🦀 **Rust performance & safety** — fast and memory-safe end to end.

---

<div align="center">

**Stop carving down an always-on attack surface.**
**Start with one that never exists in the first place.**

[**nullnet.ai**](https://nullnet.ai)

</div>
