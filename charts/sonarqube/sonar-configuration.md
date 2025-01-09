# Implementación SonarQube en Workflows

## PreConfiguracion

Para la configuracion del análisis en sonar es necesario disponer de un fichero `sonar-project.properties`, donde dentro se va a poder definir ciertas variables que condicionan el análisis.

## Configuracion Básica 
```Bash
# Clave única del proyecto
sonar.projectKey=Mi proyecto
# Nombre del proyecto
sonar.projectName=Mi Proyecto
# Versión del proyecto
sonar.projectVersion=1.0
# Ruta de las fuentes
sonar.sources=src
# Rutas de los tests
sonar.tests=tests
# Directorios excluidos del análisis
sonar.exclusions=**/node_modules/**,**/dist/**
```

## Configuracion para integracion de CI/CD
- Sonar puede analizar cambios en las Pull Requests y reportar problemas directamente en GitHub.
```Bash
# Configuración de Pull Request
sonar.pullrequest.key=${{ github.event.number }}
sonar.pullrequest.base=${{ github.event.pull_request.base.ref }}
sonar.pullrequest.branch=${{ github.head_ref }}

# URL de la instancia de Sonar
sonar.host.url=https://sonarcloud.io
```

## Configuracion Avanzada

- Definir Quality Profile personalizado
```Bash
sonar.profile=Perfil_Personalizado
```

- Exclusión de ciertos tipos de archivos o reglas
```Bash
sonar.exclusions=**/*.spec.js
sonar.issue.ignore.multicriteria=e1,e2
sonar.issue.ignore.multicriteria.e1.ruleKey=squid:S00103
sonar.issue.ignore.multicriteria.e1.resourceKey=**/*.spec.js
```

- Configuración de Logs
```Bash
sonar.log.level=DEBUG
sonar.log.file=sonar.log
```
