### Used for reusability

- Extension to the pipeline
- **Has own repository**
- **Written in groovy**
- Reference shared logic in Jenkinsfile

### Shared lib repo/project structure

- `vars` folder
  - Functions that we call from Jenkinsfile
  - Each function/execution step is its own groovy file
- `src` folder
  - Helper code
- `resources` folder
  - Use external libraries
  - Non groovy files

### Add shared lib to jenkins
- Global
  - Manage jenkins > systems > global pipeline libraries
  - Import the lib to Jenkinsfile as follows
    ```groovy
    @Library('jenkins-shared-library@master')
    ```
- Project
  - Import the lib to Jenkinsfile as follows
    ```groovy
    library identifier: 'jenkins-shared-library@master', retriver: modernSCM([
      $class: 'GitSCMSource',
      remote: 'https://gitlab.com/devopsbootcamp8550504/08-jenkins/jenkins-shared-library.git',
      credentialsId: 'gitlab-credentials'
    ])
    ```
