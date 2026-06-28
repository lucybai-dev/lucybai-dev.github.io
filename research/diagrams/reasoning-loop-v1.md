# Reasoning Loop v1

This is the earlier, more detailed research version of the reasoning loop. It is not currently used in the article. It is kept as a working model for future notes on possible worlds, pruning, simulation, and intervention.

```mermaid
flowchart TD
  O[Observation<br/>观测]
  W[Construct Possible Worlds<br/>构造多个可能世界]
  C[Constraint Propagation<br/>传播约束]
  P[Pruning<br/>剪枝]
  M{Still Multiple Worlds?<br/>还剩多个世界？}
  E[Adopt Current Best World<br/>暂时采用当前世界]
  S[Forward Simulation<br/>向未来模拟]
  G{Information Gap?<br/>还需要更多信息？}
  I[Choose Intervention<br/>主动制造信息<br/>PK / 打平衡 / 压票]
  N[New Observation<br/>新发言 / 新投票 / 新刀口]

  O --> W
  W --> C
  C --> P
  P --> M
  M -->|No| E
  M -->|Yes| S
  S --> G
  G -->|No| P
  G -->|Yes| I
  I --> N
  N --> C
```

## Notes

Core loop:

```text
Observation
  → Construct Worlds
  → Constraint Propagation
  → Pruning
  → Simulation
  → Intervention
  → New Observation
  → Constraint Propagation
```

This version is useful for internal reasoning because it preserves the decision points that the reader-facing diagram compresses away.
