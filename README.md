Documentación del Proyecto: Despliegue de Aplicación en Clúster de Kubernetes en GCP
1. Configuración Inicial del Proyecto en GCP
Creación de Proyecto
Ingresamos a la Google Cloud Console y creamos un nuevo proyecto para alojar nuestro clúster de Kubernetes.
Aseguramos que la facturación estuviera habilitada para el proyecto.
Configuración del CLI
Instalamos y configuramos Google Cloud SDK en nuestras máquinas locales.
Nos autenticamos usando el comando gcloud auth login y establecimos el proyecto con gcloud config set project reto4.
2. Creación del Clúster de Kubernetes
Usamos Google Kubernetes Engine (GKE) para configurar un clúster de Kubernetes ejecutando el siguiente comando:
gcloud container clusters create "cluster-reto4" --num-nodes "3" --region "us-central1" --enable-ip-alias
Obtenemos las credenciales del clúster para que kubectl pueda interactuar con él:
gcloud container clusters get-credentials cluster-reto4 --region us-central1
3. Instalación de Helm
Instalamos Helm en nuestras máquinas locales siguiendo las instrucciones de la página oficial.
Añadimos el repositorio de Bitnami a Helm para acceder a sus charts:
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
4. Despliegue de la Aplicación
Desplegamos WordPress utilizando el chart de Bitnami con configuraciones para alta disponibilidad:
helm install wordpress bitnami/wordpress \
  --set global.storageClass="standard-rwo",mariadb.primary.persistence.storageClass="standard-rwo" \
  --set wordpressUsername=admin,wordpressPassword=password \
  --set service.type=LoadBalancer,ingress.enabled=true,ingress.hostname=reto4.dominio.tld
5. Configuración de Alta Disponibilidad
Configuramos la alta disponibilidad para los componentes de la aplicación asegurando que la aplicación, la base de datos y el almacenamiento tengan réplicas en diferentes zonas de disponibilidad.
6. Configuración de HTTPS y Dominio
Configuramos un certificado SSL utilizando Let's Encrypt y el chart de Helm para cert-manager.
Configuramos el dominio reto4.dominio.tld para que apunte a la dirección IP proporcionada por el balanceador de cargas de GKE.
7. Monitoreo y Mantenimiento
Establecimos políticas de monitoreo y alertas utilizando Prometheus y Grafana, desplegados mediante Helm.
8. Documentación Adicional y Soporte
Documentamos todos los pasos, comandos y configuraciones usadas durante el despliegue.
Preparamos guías de usuario y mantenimiento para facilitar la operación y las actualizaciones futuras del sistema.