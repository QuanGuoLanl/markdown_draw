
# HiGHS Post-Process
## Output files
CPLEX: 'solution.sol', continuously updated during the optimization to reflect the current best solution, ensuring the file always contains only the latest optimal result. Solution in this format (.xml) can be displayed in SimCCS UI. 

HiGHS:
1. 'solution.highs': record the final best solution after the optimization is complete. No data will be written until the process has fully concluded.
2. 'improve.highs': throughout the optimization process, record all improving solutions. The optimizer may identify numerous intermediate solutions between the initial and final optimal solutions, potentially leading to a substantial number of recorded solutions.


```mermaid
flowchart TB
    subgraph Output Content
        direction TB
        A[CPLEX] --> B[solution.sol]
        C[HiGHS] --> D[solution.highs]
        C --> E[improving.highs]
    end

    subgraph Post-Process
        direction TB
        SD[Solution Directory] --> Ex{solution.sol exists?}
        Ex --> |Yes| DS[add to displayable solutions]
        DS --> DI[Select Solution In Results Pane]
        Ex --> |No| F[Highs Output]
        F[HiGHS Output] --> G{HiGHS Complete?}
        G -->|Yes| H[solution.highs]
        G -->|No| I[Investigate improve.highs]
        I --> L{Extractable solutions?}
        L --> |No| M[No Solution found]
        L --> |Yes| J[Extract solution]
        J --> H
        H --> K[convert to solution.sol format]
        K --> DS
    end
