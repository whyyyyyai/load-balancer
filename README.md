# load-balancer

**The task board for [comms-deck](https://github.com/whyyyyyai/comms-deck): one
task, one holder.** When the owner asks several agents for something, this
program, not the agents, decides who does it, so the same work isn't done
twice.

- A note from the owner to two or more agent seats becomes a task (`t1`, `t2`,
  ...) with **one holder**: the most available of those seats, by the deck's
  own ranking. Every copy of the note says who holds it.
- A second claim is refused.
- The holder keeps the task while it's busy. If it goes spent or away it loses
  the task at once. If it sits idle past the lease (30 minutes) it gets one
  reminder, then loses it a lease later. The next most available seat is told.
- `done_task` closes a task. `release_task` hands it on.

Plain, deterministic code: no AI and no network. If it's missing or broken,
comms-deck carries on exactly as before, just without tasks.

## Install (on a machine that already runs comms-deck)

    git clone https://github.com/whyyyyyai/load-balancer ~/load-balancer
    ~/comms-deck/comms-deck install --yes     # links ~/.local/bin/load-balancer
    systemctl --user restart comms-deck        # the service loads it

Check it: `load-balancer self-test` must end with `all passed`.

## Use

Agents use the deck's MCP tools `tasks`, `claim_task`, `release_task` and
`done_task`. Open tasks also show under ACTIVITY on the chat screen. From a
shell:

    load-balancer tasks [--all]
    load-balancer claim|release|done <id> --as <seat>
    load-balancer tick          # one maintenance pass; the deck's service runs it every 30 s

## If it goes wrong

Remove the link (`rm ~/.local/bin/load-balancer`) and restart comms-deck: the
deck runs without tasks. The board's state is
`~/.local/share/comms-deck/tasks/` (one JSON file per task). Deleting that
folder clears the board and loses nothing else.

## Where it fits

| Repo | What it's for |
| --- | --- |
| [comms-deck](https://github.com/whyyyyyai/comms-deck) | The program the agents talk through. Loads this one. |
| [quantum-kittens](https://github.com/whyyyyyai/quantum-kittens) | Easy start on a machine that's up, and the no-AI launcher. |
| [navigation-control-system](https://github.com/whyyyyyai/navigation-control-system) | Rebuilding the desktop and the deck from nothing. |
| **load-balancer** (this one) | Who does which task. |

The design for routing reviews as tasks too is Lumo's spec
([comms-deck #40](https://github.com/whyyyyyai/comms-deck/pull/40)). That's
the next step for this repo.

---

Written by: Claude (2026-11-25), at the owner's request.
