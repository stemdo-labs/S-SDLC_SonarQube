# Implementación SonarQube en Workflows


# Método 1

## PreConfiguracion

1) Creamos un proyecto de manera "Manually"
2) Rellenamos los parametros con el nombre del projecto, la projectKey y la rama:
   
![image](https://github.com/user-attachments/assets/bad85f48-f8a8-427c-8469-ce24be766e17)

4) Seleccionamos ejecutal el CI con GHA

![image](https://github.com/user-attachments/assets/363d616e-db50-4625-b92d-51e2e337c87e)

5) Finalmente seguimos los pasos Mencionados:

    *Agregar Secretos al repositorio*

![image](https://github.com/user-attachments/assets/d4dcad84-ea70-4fce-aa56-d013f940795b)

*Luego creamos un fichro en el repositorio llamado sonar-project.properties*, metiendole (REQUIRED) lo siguiente:
`sonar.projectKey=prueba-sonar`

6) Finalmente añadimos al workflow los siguientes steps:
```yaml
steps:
  - uses: actions/checkout@v2
    with:
      fetch-depth: 0  # Shallow clones should be disabled for a better relevancy of analysis
  - uses: sonarsource/sonarqube-scan-action@master
    env:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
      SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
  # If you wish to fail your job when the Quality Gate is red, uncomment the
  # following lines. This would typically be used to fail a deployment.
  # - uses: sonarsource/sonarqube-quality-gate-action@master
  #   timeout-minutes: 5
  #   env:
  #     SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

En este método, para la configuración del análisis en sonar es necesario disponer de un fichero `sonar-project.properties`, donde dentro se va a poder definir ciertas variables que condicionan el análisis.

7) Probar el Workflow.

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



# Método 2

- **Para este método no es necesario crear un proyecto manualmente en la interfaz de SonarQube** y la configuración se puede realizar en el workflow directamente añadiendo los siguientes steps:

```yaml
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0  # Shallow clones should be disabled for a better relevancy of analysis
      - uses: sonarsource/sonarqube-scan-action@master
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
      # El uso del siguiente step determina si es bloqueante o no la pipeline, a partir del Quality Gate
      - uses: sonarsource/sonarqube-quality-gate-action@master
        timeout-minutes: 5
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

- La configuración se hace en el step 2 haciendo uso de la action `sonarsource/sonarqube-scan-action@master`, declarando variables de la siguiente manera:

```yaml
- uses: SonarSource/sonarqube-scan-action@<action version>
  env:
    SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
    SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
  with:
    projectBaseDir: app/src
    args: >
    -Dsonar.organization=my-organization # For SonarQube Cloud only
    -Dsonar.projectKey=my-projectkey
    -Dsonar.python.coverage.reportPaths=coverage.xml
    -Dsonar.sources=lib/
    -Dsonar.tests=tests/
    -Dsonar.test.exclusions=tests/**
    -Dsonar.verbose=true
```
***Si no se hace uso del fichero .properties es MANDATORY declarar la variable -Dsonar.projectKey=my-projectKey, luego todo lo demas es opcional**
