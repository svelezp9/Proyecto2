# Informe proyecto 2 Telematica
### Integrantes
  - Sebastián Vélez Pulgarín (svelezp9@eafit.edu.co) (Disponibilidad, Rendimiento)
  - Antoine Chavane de Dalmassy Córdoba (achavaned@eafit.edu.co) (Seguridad)
  - Felipe Álvarez Benítez (falvarezb@eafit.edu.co) (Diseño)
## Github
https://github.com/svelezp9/Proyecto2
## Requerimientos del despliegue
1) Aplicación dockerizada, tanto back como front
2) Nombre de dominio para el aplicativo
3) Certificado ssl para la página web(HTTPS)(Pendiente)
4) Utilizar una aplicación de monitorio(Cloud Watch, servicio de AWS)
5) Implementar Load Balancer
6) Auto-Scaling group (Crecimiento Horizontal, back up, rendimiento, etc)
7) Alta disponibilidad
8) Conectarse a las instancias a traves del bastion Host
## Requerimientos no Funcionales de la App
1) Almacenamiento total de 1TB(Aumentar el storage de las bases de datos mongo a 1T, por el momento cada una son de 8GB para minimizar costo, son 3 instancias)
### Disponibilidad
1) Sistema que soporte un minimo de 1000 usuarios con una concurrencia del 10%
2)  Replicación de bases de datos para mejorar disponibilidad a los usuarios y back up
### Rendimiento
1) Tiempos de respuesta entre 1 y 2 segundos
2) CDN y velocidad(Cloudfare)
### Seguridad
1) Separación de las maquinas por subnets 2 publicas y 4 privadas
2) Permitir entrada de datos únicamente por puertos necesarios para el aplicativo, bloquear los otros
## Costo estimado
Para el proposito de la app web, al tener el auto-Scaling group activo para un máximo de 3 instancias para el front y 2 para el back, más las 3 bases de datos y el bastión host se tendrán un total de 9 maquinas al tiempo,
con esto se estima un consumo mensual de alrededor de unos 150 dolares (en 1 días se estimó un crecimiento de 6-7 dolares en aws Academy)
## Costo/beneficio
Según este costo estimado el equipo considera una muy buena relación Costo/beneficio, por tan solo 70 dolares se tiene la página web con alta disponibilidad, rapidez y confiabilidad
## Diseño de despliegue
![Diseño](https://user-images.githubusercontent.com/73863024/168914275-f678bc54-697d-4a06-87f6-b2a2058f0054.jpeg)
## Diseño de arquitectura 
![DiseñoArquitectura](https://user-images.githubusercontent.com/73863024/168915135-44f1bbfc-8859-4c28-b457-c467e19e990e.jpeg)
- Como podemos observar nuestro diseño para la arquitectura consistia en una arquitectura por capas (presentación, lógica del negocio y persistencia de datos), 2 de estas controladas por un ALB, para la escalabilidad se plantearon varias replicas de las instancias, los balanceadores de carga, las replicaciones en la BD, y para la seguridad se convertirtieron las capas en subnets, sin embargo al final se decidió meter la capa de presentación y de lógica del negocio en la misma subnet.
- De herramientas se utilizó AWS para el despliegue y se planteó Cloudfare para certificados y cdn, esto debido a que aws no permite a los usuarios de la universidad registrar el https con nuestros certificados
- El replica set de mongo automaticamente se encarga de distribuir peticiones en caso de que un nodo este sobrecargado o si se especifica el comando el read siempre se hará en las replicas secundarias
## Resumen
### Disponibilidad
- Load Balancer Front/back
- autoScaling-Group
- replicación BD Mongo
### Seguridad
- División por subnets
- Todas las capas de la aplicación en subnets privadas
- restricción de puertos
- Ingreso a las maquinas a través del bastión host
- Encryptación de la data en la web a través de Cloudflare
### Rendimiento
- Load Balancer
- CDN con cloudflare
- Aumento de velocidad con cloudflare 
![image](https://user-images.githubusercontent.com/73863024/168957653-aa661fab-deeb-4680-bd91-270dbd2e4101.png)


