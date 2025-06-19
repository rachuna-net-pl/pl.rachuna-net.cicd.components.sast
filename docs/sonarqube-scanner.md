**Wymagania**
- Obraz kontenera dostępny jest [tutaj](https://gitlab.com/pl.rachuna-net/containers/sonar-scanner/container_registry/8446311).
- Dostęp do platformy `sonarqube`

### Jak działa komponent?

#### Uruchomienie statycznego skanowania `💪 sonarqube scanner`
Job uruchamia statyczne skanowanie kodu i wysyła do sonarqube w celu jej analizy

### Przykładowe użycie w `.gitlab-ci.yml`
```yml
default:
  tags:
    - onprem

include:
- local: .gitlab/stages.yml
- local: .gitlab/workflow.yml
- component: $CI_SERVER_FQDN/pl.rachuna-net/cicd/components/versioning/versioning@main
- component: $CI_SERVER_FQDN/$CI_PROJECT_PATH/sonarqube@$CI_COMMIT_SHA
  inputs:
    docker_image: "registry.gitlab.com/pl.rachuna-net/containers/sonar-scanner:1.0.0"
    sonar_host_url: "${SONAR_HOST_URL}"
    sonar_organization: "${SONAR_ORGANIZATION}"
    sonar_project_name: ""
    sonar_project_key: "${SONARQUBE_CLOUD_PROJECT_ID}"
    sonar_token: "${SONAR_TOKEN}"
    sonar_project_description: "${CI_PROJECT_DESCRIPTION}"
    sonar_project_version: "${RELEASE_CANDIDATE_VERSION}"
    sonar_sources: "."
```

### Inputs
|Input  |env                        |Description                        | Default Value                                                          |
|-------|---------------------------|-----------------------------------|------------------------------------------------------------------------|
|`image`|                           |Obraz z `sonar-scanner-cli`        |`registry.gitlab.com/pl.rachuna-net/containers/sonar-scanner:v1.0.0`    |
|       |`SONAR_USER_HOME`          |Katalog roboczy dla `sonar-scanner`|`${CI_PROJECT_DIR}/.sonar`                                              |
|       |`SONAR_HOST_URL`           |Adres do platformy `sonarqube`     |                                                                        |
|       |`SONAR_ORGANIZATION`       |Klucz organizacji                  |                                                                        |
|       |`SONAR_PROJECT_KEY`        |Klucz projektu w `sonarqube`       |`${SONARQUBE_CLOUD_PROJECT_ID}`                                         |
|       |`SONAR_PROJECT_NAME`       |Nazwa projektu w `sonarqube`       |                                                                        |
|       |`SONAR_PROJECT_DESCRIPTION`|Opis projektu w `sonarqube`        |                                                                        |
|       |`SONAR_PROJECT_VERSION`    |Wersja kodu projektu w `sonarqube` |`${RELEASE_CANDIDATE_VERSION}`                                          |
|       |`SONAR_SOURCES`            |Źródło kodu                        |`.`                                                                     |
|       |`SONAR_TOKEN`              |Token do `sonarqube`               |                                                                        |