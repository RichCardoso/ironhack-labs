# Designing and Simulating AWS Cloud Architectures

## Part 1: Designing Cloud Infrastructure

### Arquitectura
   
  - Usaremos instancias EC2 para hospedar la aplicación web y cualquier otro componente necesario.
  - Configuraremos un Auto Scaling Group para escalar automaticamente las instancias basadas en metricas como la CPU o la utilizacion de la memoria.
  - Usaremos Amazon S3 para almacenar archivos estaticos como imagenes, videos, css y scripts.
  - Configuraremos S3 para estos archivos sean consumidos a través de una CDN como Amazon CloudFront para mejorar la velocidad de carga de la aplicacion.
  - Configuraremos una VPC para aislar nuestra infraestructura en la nube y controlar el acceso a los recursos tales como la data sensible en las bases de datos.

    > ![Diagrama sin título drawio](https://github.com/RichCardoso/ironhack-labs/assets/129906460/e90fa4b7-1d91-436b-9609-9f5449da4d39)


    
## Part 2: IAM Configuration

### Roles y Politicas de IAM

  - ***Desarrolladores***: Acceso limitado a recursos como S3 y EC2 para desarrollo y pruebas sinedo restringido el acceso a configuraciones de red o bases de datos sensibles.
  - ***Administradores***: Acceso completo a todos los recursos de AWS para administracion y configuracion asegurando que asegurara de que sigan el principio de privilegio
    mínimo y limitando el acceso solo a los recursos necesarios para realizar sus tareas.
  - ***Servidores de aplicaciones***: Acceso restringido a recursos especificos previamente identificados para la ejecución de la aplicacion, como instancias EC2 y S3
    restringiendo el acceso a recursos de administración como IAM o VPC. 

## Part 3: Resource Management Strategy

### Administracion de Recursos

  - Configuraremos Auto Scaling para escalar automaticamente las instancias EC2 en funcion de la demanda del tráfico.
  - Utilizaremos Elastic Load Balancing (ELB) para distribuir el trafico entrante entre las instancias EC2 y mejorar la disponibilidad y la tolerancia a fallos.
  - Configuraremos AWS Budgets para establecer limites de gastos y recibir alertas cuando los costos superen ciertos umbrales. Adiconalmente, podriamos utilizar
    AWS Cost Explorer para analizar y optimizar el uso de recursos.

## Part 4: Theoretical Implementation

### Interaccion entre Componentes

  - Los usuarios podran acceder a la aplicacion web usando de la URL publica, esta estara definida en Load Balancer.
  - El Load Balancer distribuye el trafico entre las instancias EC2 disponibles en el Auto Scaling.
  - Las instancias EC2 acceden a los archivos estaticos almacenados en S3 para exponer el contenido a los usuarios.
  - Las instancias EC2 prodan acceder a otros servicios de AWS como bases de datos, sistemas de colas o servicios de terceros para funcionalidades adicionales.

## Concluciones

Las instancias EC2 sirven como el nucleo de la logica de la aplicacion, mientras que Amazon S3 proporciona almacenamiento seguro y escalable para archivos estáticos.
La VPC asegura la segregacion y control de la red para la infraestructura en la nube, siendo una arquitectura eficiente entre los componentes de la aplicación web y 
garantizando un flujo de datos seguro entre los servicios.
