When jenkins runs as a docker container and needs build tools to be available for pipelines
- Intall required plugins
  - `Docker Agent Plugin` in Jenkins uses host's `Docker socket binding`. It relies on Docker **already being installed on the host machine**
- Install required tools
  - Use `Jenkins tools configuration` to install Maven inside the Jenkins container.
- Install required tools inside running jenkins container yourself
- With docker we can use `Docker socket binding`

Notes about installations:
- To unstall nodejs we need to first install NodeJS plugin and then NodeJS tool.
  - It will install NodeJS and npm bin in `/var/jenkins_home/tools/jenkins.plugins.nodejs.tools.NodeJSInstallation/NodeJS-23.7.0/bin` and add it to $PATH env
