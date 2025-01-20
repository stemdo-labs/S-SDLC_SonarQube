# Code Definition Options

A la hora de crear de manera local o importar un proyecto te dejará configurar el repositorio de las siguientes maneras. <a href="https://docs.sonarsource.com/sonarqube-community-build/core-concepts/clean-as-you-code/about-new-code/">Documentación</a>

![image](https://github.com/user-attachments/assets/7548ddbe-fcb9-4f35-8031-4ed8ec9ac11b)

Dentro del archivo de configuración del proyecto (el archivo `properties`) se establecerá un atributo llamado `sonar.projectVersion=version` y `version` será la versión del proyecto. Ej: `1.0.0-SNAPSHOT` 

## Previus version

Esta es la forma predeterminada de cofigurar el código, básicamente el análisis tiene en cuenta todo el código "nuevo y viejo", pero clasifica como "código nuevo" únicamente los cambios realizados desde el último incremento de versión. Ej: pasar de  `1.0.0-SNAPSHOT` a  `1.1.0-SNAPSHOT` . Esto es intencional para mantener un control claro basado en versiones.

## Number of days

Define código nuevo como cualquier código que haya cambiado en los últimos X días.

## Reference branch

Cualquier diferencia entre tu rama principal definida en SonarQube con la rama que quieras analizar será código nuevo, decir que está función analizará solo lo que sea código nuevo. Los beneficios que te proporciona esta opción son:

- Enfoque en los cambios recientes ya que no evalua todo el código.
- Calidad continua porque te garantiza que los cambios cumplan los estándares de calidad antes de realizar un merge con la rama principal.
- Control preciso a la hora de hacer un merge de ambas ramas debido al análisis de SonarQube.
