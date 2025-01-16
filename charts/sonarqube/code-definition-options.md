# Code definition options

A la hora de crear de manera local o importar un proyecto te dejará configurar el repositorio de las siguientes maneras. <a href="https://docs.sonarsource.com/sonarqube-community-build/core-concepts/clean-as-you-code/about-new-code/">Documentación</a>

![image](https://github.com/user-attachments/assets/7548ddbe-fcb9-4f35-8031-4ed8ec9ac11b)

Dentro del archivo de configuración del proyecto (el archivo `properties`) se establecerá un atributo llamado `sonar.projectVersion=version` y `version` será la versión del proyecto. Ej: `1.0.0-SNAPSHOT` 

## Previus version

Esta es la forma determinada de cofigurar el código, básicamente a el análisis tiene en cuenta todo el código "nuevo y viejo", pero clasifica como "código nuevo" únicamente los cambios realizados desde el último incremento de versión, es decir, una vez la incremetes. Ej: pasar de  `1.0.0-SNAPSHOT` a  `1.1.0-SNAPSHOT`
