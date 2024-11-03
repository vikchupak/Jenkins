- (Chained) Freestyle Jobs (UI-based configuration) multiple jobs(like stages in pipeline jobs) chained together
- Pipeline Jobs (scripting - pipeline as code) one job with stages, each stage can have multiple steps
  - pipeline syntax:
     - scripted (original pipeline syntax, advanced scripting capabilities, high flexibility, uses Groovy syntax)
     - declarative (introduced to simplify pipeline creation, easy to start, has pre-defined structure, uses Groovy syntax)
- Multibranch Pipeline Jobs

| Feature                | Freestyle Job         | Pipeline Job              | Multibranch Pipeline Job             |
|------------------------|-----------------------|---------------------------|--------------------------------------|
| **Definition**         | Basic, UI-configured  | Code-defined pipeline     | Auto-discovered pipelines per branch |
| **Configuration**      | Simple, in Jenkins UI | Jenkinsfile in repo       | Jenkinsfile in each branch           |
| **Flexibility**        | Limited               | High, supports branching  | High, supports multiple branches     |
| **Best For**           | Simple builds         | Complex CI/CD pipelines   | Multi-branch projects                |

- Use **Freestyle Jobs** for simple projects or quick setups.
- Use **Pipeline Jobs** for more complex workflows and projects that require “Pipeline as Code.”
- Use **Multibranch Pipeline Jobs** for repositories with many branches that need distinct pipelines for each branch.
