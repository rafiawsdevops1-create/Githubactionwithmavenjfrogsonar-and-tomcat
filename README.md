
# Maven + JFrog + SonarQube + Tomcat example pipeline

This repository contains a minimal Java Servlet WAR built with Maven and an example GitHub Actions workflow that:
- builds and runs unit tests
- runs SonarQube analysis
- publishes artifacts/build-info to JFrog Artifactory (via jfrog CLI)
- deploys the WAR to Tomcat Manager

## Files added
- pom.xml — Maven project with plugins and dependencies
- src/main/java/com/example/helloworld/HelloServlet.java — simple servlet
- src/main/webapp/WEB-INF/web.xml — minimal web descriptor
- .github/workflows/ci.yml — GitHub Actions workflow

## Required GitHub Secrets
You need to add the following repository secrets (Settings → Secrets and variables → Actions):
- SONAR_HOST_URL — your SonarQube base URL (e.g. https://sonar.example.com)
- SONAR_TOKEN — token with permissions to run analysis
- JFROG_URL — https://your-jfrog-instance
- JFROG_USER — Artifactory user
- JFROG_PASSWORD — Artifactory password / API key
- JFROG_REPO — target repository/key in Artifactory (used by your Artifactory config or distributionManagement)
- TOMCAT_HOST — hostname or IP of Tomcat Manager
- TOMCAT_PORT — Tomcat port (usually 8080)
- TOMCAT_USER — Tomcat manager username
- TOMCAT_PASS — Tomcat manager password

## Local commands
- Build: mvn clean package
- Run Sonar locally: mvn sonar:sonar -Dsonar.host.url=... -Dsonar.login=...
- Deploy locally with Tomcat manager: curl -u user:pass -T target/your.war "http://host:8080/manager/text/deploy?path=/app&update=true"

## Notes
- The workflow uses jfrog/setup-jfrog-cli + jfrog rt mvn to publish to Artifactory and publish build-info.
- You can instead configure Maven `settings.xml` with server credentials and use regular `mvn deploy`.
- The Tomcat deployment in the workflow uses the Tomcat Manager text API; ensure manager is enabled and credentials have the manager-script role.
- Adjust sonar.projectKey to match your Sonar configuration if desired.
