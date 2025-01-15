# Implemetación de Community Branch Plugin en SonarQube

## Implementación en el chart

Para añadir el plugin al chart de SonarQube debemos añadir al archivo `values.yml` con el que despleguemos el chart el siguiente fragmento.

```yaml
plugins:
  install:
    - https://github.com/mc1arke/sonarqube-community-branch-plugin/releases/download/1.23.0/sonarqube-community-branch-plugin-1.23.0.jar
sonarProperties:
  sonar.web.javaAdditionalOpts: "-javaagent:/opt/sonarqube/extensions/plugins/sonarqube-community-branch-plugin-1.23.0.jar=web"
  sonar.ce.javaAdditionalOpts: "-javaagent:/opt/sonarqube/extensions/plugins/sonarqube-community-branch-plugin-1.23.0.jar=ce"
```

Siendo el `1.23.0` la versión del plugin siguiendo esta rúbrica:

| SonarQube Image Version | Plugin Version |
|--------------------------|----------------|
| 24.12 (10.8)            | 1.23.0         |
| 10.6 - 10.7             | 1.22.0         |
| 10.5                    | 1.20.0         |
| 10.4                    | 1.19.0         |
| 9.9 (LTS)               | 1.14.0         |

## Implementación del plugin en GitHub Actions

Para una explicación sencilla de como usar el plugin creare un proyecto en la pestaña de `Projects` en el desplegable `Create Project/Local project`

![image](https://github.com/user-attachments/assets/ab1b1b56-efff-490b-9c3d-8d1738d60558)

La configuración empezará pidiendo un nombre para nuestro proyecto.

![image](https://github.com/user-attachments/assets/7c366ed7-86f5-47d6-b2f8-ba8688b17272)

Y la configuración que deseemos, en mi caso la global.

![image](https://github.com/user-attachments/assets/6305838c-2788-4b9a-b1e3-251e6eb8e79f)

En el método que queremos usar el análisis elegiré GitHub Actions.

![image](https://github.com/user-attachments/assets/717580e7-6899-4e16-9718-35959840b13e)

### Primera parte - Crear secretos de GitHub

SonarQube nos especificará cómo debemos de crear 2 secretos de repositorio de GitHub con sus respectivos valores.

![image](https://github.com/user-attachments/assets/d8d1c5ce-040a-4d82-b290-c7e6084c32d5)

### Segunda parte - Crear workflow

El propio SonarQube nos creará un workflow para ejecutar el análisis del código de nuestro repositorio eligiendo `Other` en la primera opción.

La segunda opción nos dirá que debemos crear un archivo llamado `sonar-project.properties` en el directorio raíz del repositorio que incluya `sonar.projectKey=backend`

En la tercera opción elegiremos el tipo de sistema operativo que deseamos para ejecutar el workflow.

Y por último nos generará el wworkflow que debe ser creado en la ruta `.github/workflows/build.yml` de nuestro repositorio el cual usará los secretos creados en el primer paso.

![image](https://github.com/user-attachments/assets/0ad148d0-f862-4261-9dbe-93fd2928f194)

## Diferencias entre ramas usando el plugin

Una vez hecho todo esto podremos empezar a usar SonarQube diferenciando entre ramas dentro de nuestro repositorio de las siguientes maneras:

### Varias ramas a la vez

Usando `sonar.branch.name` podremos especificar las ramas que queremos que analice al ejecutar el workflow, esto analizará el código que le especifiquemos de esa rama.

```properties
sonar.branch.name=develop
sonar.branch.name=main
```

### Ramas de las Pull requests

De esta otra forma podemos analizar solo el código modificado de la la rama que va a mergearse con nuestra rama base de la pull request.

```properties
sonar.pullrequest.key=${{ github.event.number }}
sonar.pullrequest.base=${{ github.event.pull_request.base.ref }}
sonar.pullrequest.branch=${{ github.head_ref }}
```