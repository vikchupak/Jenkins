When jenkins run as a docker container and needs build tools to be available for pipelines
- Intall required tools as plugins
  - `Docker Agent Plugin` in Jenkins uses host's `Docker socket binding`
- Install required tools inside running jenkins container
- With docker we can use `Docker socket binding`
