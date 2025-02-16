pipeline => stages => steps

- (Chained) **Freestyle Jobs** (UI-based configuration) multiple jobs(like stages in pipeline jobs) **chained** together
  - The weakest king of a job with a lot of limitations
    - UI based, no scripting
    - Limited to functionality that plugins provide
    - Each job requires plugins setup even if the plugins are the same
  - Common practice to have a job for each step, and chaine them in order using post-build actions
    - step1/job1 - run tests, post-build action trigers step2/job2
    - step2/job2 - build jar, post-build action trigers step3/job3
    - step3/job3 - build image, post-build action trigers step4/job4
    - step4/job4 - push to repo
  - *Here a step is more like a stage in Pipeline jobs*
- **Pipeline Jobs** (**scripting - pipeline as code**) one job with stages, each stage can have multiple steps
  - We can think of a pipeline job like superset of freestyle jobs
  - 2 ways to keep/save scripts
    - Inside jenkins as a **Pipeline script**
    - Inside repository as a **Pipeline script from SCM(source code management)** in Jenkinsfile (**recommended**)
  - Jenkinsfile using Groovy:
    - pipeline syntax:
      - **scripted** (original pipeline syntax, advanced scripting capabilities, high flexibility, uses Groovy syntax)
        ```groovy
        node { // required
            stage('Build') {
                echo 'Building...'
            }
            stage('Test') {
                echo 'Testing...'
            }
            stage('Deploy') {
                echo 'Deploying...'
            }
        }
        ```
      - **declarative** (introduced to simplify pipeline creation, easy to start, has pre-defined structure, uses Groovy syntax)
        - *In declarative pipeline, you must specify an agent(it's mandatory). We can set `agent any` when not really using agents.*
        ```groovy
        pipeline { // attribute must be top-level
            agent any // attribute always must be set
            stages { // attribute required
                stage('Build') {
                    steps {
                        echo 'Building application...' // step 1
                        sh 'mvn clean package -DskipTests' // step 2
                        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true // step 3
                    }
                }
                stage('Test') {
                    steps {
                        echo 'Testing...'
                    }
                }
                stage('Deploy') {
                    steps {
                        echo 'Deploying...'
                    }
                }
            }
        }
        ```
        - Syntax features
          - conditions using `when`
          - environment variables using `environment`
          - credentials plugins using `withCredentials`
          - build tools using `tools`
          - build parameters using `parameters`
          - input parameters using `input`
          - load scripts using `script`
- Multibranch Pipeline Jobs
  - ALL baranches should have its own pipeline. A pipeline is created per a branch.
  - We need stages/steps common for all branches, like run tests, but skip deploy (Jenkins looks for a Jenkinsfile in a spesific branch and builds a pipeline for the branch based on it. But we can store Jenkinsfile in master branch and all other branches fork from it, will have the same Jenkinsfile. We can use conditions inside Jenkinsfile to skip some stages/steps based on branch name `BRANCH_NAME` - var avaliable only in Multibranch Pipeline. Pipelines are only build for branches that match regexp in Jenkins configuration)
  - We need some branch-specific stages/steps for some branches, like master to include deploy.

| Feature                | Freestyle Job         | Pipeline Job              | Multibranch Pipeline Job             |
|------------------------|-----------------------|---------------------------|--------------------------------------|
| **Definition**         | Basic, UI-configured  | Code-defined pipeline     | Auto-discovered pipelines per branch |
| **Configuration**      | Simple, in Jenkins UI | Jenkinsfile in repo       | Jenkinsfile in each branch           |
| **Flexibility**        | Limited               | High, supports branching  | High, supports multiple branches     |
| **Best For**           | Simple builds         | Complex CI/CD pipelines   | Multi-branch projects                |

- Use **Freestyle Jobs** for simple projects or quick setups.
- Use **Pipeline Jobs** for more complex workflows and projects that require “Pipeline as Code.”
- Use **Multibranch Pipeline Jobs** for repositories with many branches that need distinct pipelines for each branch.
