  # Proyecto Final - Docker & Kubernetes

   **Alumno:** LUIS ABON FLORES
   **Fecha:** 31/10/2025
   **Curso:** Docker & Kubernetes - i-Quattro

   ## Links de Docker Hub
   - Backend v2.1: https://hub.docker.com/r/abflores/springboot-api/tags?name=v2.1
				   https://hub.docker.com/r/abflores/springboot-api/tags	
   - Frontend v2.2: https://hub.docker.com/r/abflores/angular-frontend/tags?name=v2.2 
					https://hub.docker.com/r/abflores/angular-frontend

   ## Parte 1: Setup del Ambiente

   **Ambiente utilizado:** 

		**WSL2 (Windows Subsystem for Linux sobre Windows 11)**
		(Equivalente a una VM liviana manejada directamente por Windows, sin VirtualBox o nube)
		
		**Nombre de VM/Instancia:**  
			WSL no usa nombres de instancias por defecto.		
		**Sistema operativo:**
			Ubuntu 24.04 LTS (WSL2)
		**Recursos:**
		
		4 GB RAM, 2 CPU cores (configurados mediante .wslconfig o valores por defecto)
		(WSL usa una parte dinámica de los recursos del sistema host.)
		
		Red configurada:
		
		NAT (predeterminada en WSL2)
		 WSL2 crea una interfaz virtual y usa NAT para salir a Internet;  
		
		Rango MetalLB:
		
		eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
		link/ether 00:15:5d:c7:68:10 brd ff:ff:ff:ff:ff:ff
		inet 172.29.69.152/20 brd 172.29.79.255 scope global eth0

   ### Screenshots
   ![microk8s status](screenshots/parte1-microk8s-status.png)
   **Archivo:** `parte1-microk8s-status.png`
   ![Pods running](screenshots/parte1-pods-running.png)
   **Archivo:** `parte1-pods-running.png`
   ![Frontend via MetalLB](screenshots/parte1-frontend-browser.png)
    **Archivo:** `parte1-frontend-browser.png`
	maquina local
	**maquinaHostLflores.png**
	parte1-maquinaHostLflores.png

   ## Parte 2: Backend v2.1
   [Descripción de cambios realizados]
	Se añadió un nuevo endpoint REST en la clase GreetingController.java con la ruta /api/info.
	Este endpoint tiene como propósito exponer información general del entorno y del alumno, retornando un objeto JSON con datos dinámicos y estáticos.
	Debido a que existía previamente un método que utilizaba la misma ruta (/api/info), fue necesario eliminar o comentar el método anterior para evitar conflictos de mapeo en el controlador. 
	
   ### Código Agregado
   [Snippet del endpoint /api/info]
   ```
   @GetMapping("/api/info")
	public ResponseEntity<Map<String, Object>> getInfo() {
		Map<String, Object> info = new HashMap<>();
		info.put("alumno", "LUIS ABDON FLORES");
		info.put("version", "v2.1");
		info.put("curso", "Docker & Kubernetes - i-Quattro");
		info.put("timestamp", LocalDateTime.now().toString());
		info.put("hostname", System.getenv("HOSTNAME"));
		return ResponseEntity.ok(info);
	}
	```
	Descripción funcional:
	```
		Ruta expuesta: /api/info
		Método HTTP: GET
		Respuesta: JSON con los siguientes campos:
			alumno: nombre del estudiante
			version: versión actual del servicio
			curso: nombre del curso asociado
			timestamp: fecha y hora en que se realiza la solicitud
			hostname: nombre del host o contenedor donde se ejecuta la aplicación (útil para entornos Docker/Kubernetes)
			Este cambio permite verificar la información del despliegue y la identidad del contenedor de forma sencilla mediante una petición HTTP, facilitando pruebas y monitoreo del servicio en entornos virtualizados o orquestados.
	```
   ### Screenshots
   ![Docker build](screenshots/parte2-docker-build.txt)
   **Archivo:** `parte2-docker-build.txt`
   ![Rollout](screenshots/parte2-rollout.png)
      **Archivo:** `parte2-rollout.png`
   ![API Info](screenshots/parte2-api-info.png)
    **Archivo:** `parte2-api-info.png` 
	 **Archivo:**`parte2-http_dockerHub.png`
	 
   ## Parte 3: Frontend v2.2
   [Descripción de cambios en Angular]

   ### Screenshots
   ![Frontend build](screenshots/parte3-frontend-build.txt)
    **Archivo:** `parte3-frontend-build.txt`
   ![Frontend UI](screenshots/parte3-frontend-ui.png)
     **Archivo:** `parte3-frontend-ui.png`
   ![System info display](screenshots/parte3-system-info.png)
     **Archivo:** `parte3-system-info.png`	 
	 **Archivo:** `parte3-htmlCambio.png`
	  **Archivo:** `parte3-get pods.png`
	  **Archivo:** `parte3- dockerhub.png`
	  **Archivo:** `parte3-CambioComponente.png`
   ## Parte 4: Gestión de Versiones

   ### ¿Qué hace kubectl rollout undo?
   
   ```
	kubectl rollout undo revierte el despliegue de un recurso controlado por el Deployment Controller (u otros controladores compatibles), volviendo al estado anterior registrado en el historial de despliegues.
	```
	``Sintaxis básica``
	
	```
	kubectl rollout undo deployment/<nombre-del-deployment>
	
	```
	Qué hace internamente
	Kubernetes mantiene un historial de revisiones (revisions) de cada Deployment cada vez que cambias su especificación (por ejemplo, imagen, replicas, etiquetas, etc.).
	Cuando ejecutas kubectl rollout undo, Kubernetes:
	Busca la última revisión anterior.
	Actualiza el Deployment para que vuelva a usar la configuración de esa revisión.
	Crea una nueva revisión (por ejemplo, pasa de la rev. 5 a la 6, aunque la 6 sea igual a la 4).
	Los pods antiguos se terminan y se crean nuevos pods con la configuración revertida.
	
   ### Screenshots
   ![Rollback](screenshots/parte4-rollback.png)
   **Archivo:** `parte4-rollback.png`
   ![Rollforward](screenshots/parte4-rollforward.png)
 **Archivo:** `parte4-rollforward.png`  
  **Archivo:** `parte4-kubectl rollout history.png`
  **Archivo:** `parte4-kubectl rollout undo n2.png`
  
  
   ## Parte 5: Ingress + MetalLB

   **IP del Ingress:** [Tu IP de MetalLB]

   ### Screenshots
   ![Ingress config](screenshots/parte5-ingress.png)
    **Archivo:** `parte5-ingress.png`
   ![Acceso externo](screenshots/parte5-external-access.png)
	**Archivo:** `parte5-access-apiInfo.png`
	**Archivo:** `parte5-actuator_healt.png`
	**Archivo:** `parte5-external-access-metalib_user_greeint.png`
	**Archivo:** `parte5-ipmetalLB.png`
	**Archivo:** `parte5-metalib_front.png`
 
 ## Conclusiones

   ### Aprendizajes principales
		   1. Configuración básica de MicroK8s y servicios
		**	MicroK8s te permite levantar un cluster Kubernetes local en Ubuntu.
		**	Habilitaste addons importantes:
			dns
			ingress
			metallb (con rango 172.29.79.200-172.29.79.250)
		**	MetalLB asigna IPs externas a servicios de tipo LoadBalancer.
		**	kubectl get svc -n <namespace> te muestra:
			ClusterIP → accesible solo dentro del cluster 
		 
		2. Deployments y Pods
		**	Creamos deployments para API, frontend, postgres y redis.
		**	Los Pods se crean a partir de los deployments y pueden reiniciarse si se eliminan:
			microk8s kubectl delete pods -l app=api -n proyecto-integrador
		**	Para verificar que los pods estén listos:
			microk8s kubectl get pods -n proyecto-integrador -w
		**	Para ver detalles de un deployment:
			microk8s kubectl describe deployment api -n proyecto-integrador

		3. Versiones de la imagen Docker y problemas comunes
		**	Cuando actualizabas el código y construías la imagen, a veces MicroK8s seguía usando la versión cached de la imagen.
		**	Para forzar que se use la nueva versión:
			microk8s ctr images rm docker.io/abflores/springboot-api:v2.1
			microk8s kubectl rollout restart deployment/api -n proyecto-integrador
		**	Después de esto, el curl hacia la API mostraba la nueva versión correctamente (v2.1).
		4. Servicios y cómo acceder a ellos
		**	Servicios tipo ClusterIP solo son accesibles desde dentro del cluster o usando kubectl exec:
			microk8s kubectl exec -it <pod> -n proyecto-integrador -- curl http://api-service:8080/api/info
		**	Servicios tipo LoadBalancer (con MetalLB) permiten hacer curl desde Ubuntu o cualquier host externo:
			curl http://<IP-METALLB>:8080/api/info
		 
		5. Ingress
		**	Habilitar Ingress permite enrutar múltiples servicios en la misma IP y puerto 80 usando rutas (/api/info, /api/greeting, / para frontend).
		**	Problema común: kubectl get ingress mostraba 127.0.0.1 porque el Service del Ingress Controller era ClusterIP.
		**	Solución: cambiarlo a LoadBalancer para que MetalLB le asigne una IP externa.
			microk8s kubectl edit svc ingress-nginx-controller -n ingress-nginx
		  
		6. Curl para probar rutas
		**	Frontend: curl http://<IP-METALLB>/
		**	API Users: curl http://<IP-METALLB>/api/users
		**	API Greeting: curl http://<IP-METALLB>/api/greeting
		**	API Info: curl http://<IP-METALLB>/api/info
		**	Health Actuator: curl http://<IP-METALLB>/actuator/health
			Esto te permite probar todo desde Ubuntu sin entrar a los pods.
		 
		7. Buenas prácticas aprendidas
		**	Siempre revisar la versión de la imagen y el cache local de MicroK8s.
		**	Para cambios en la API, hacer kubectl rollout restart deployment o borrar los pods.
		**	Verificar el tipo de servicio antes de intentar acceder desde fuera (ClusterIP vs LoadBalancer).
		**	Para Ingress, MetalLB debe estar habilitado y el controller debe ser LoadBalancer para obtener IP externa.
		 
		  Conclusión:
		**	MicroK8s + MetalLB + Ingress + Deployments + Pods forman un flujo completo de desarrollo y pruebas locales en Kubernetes.
		**	El truco más importante que aprendi es cómo forzar la actualización de imágenes y asegurarte de que los pods estén usando la versión correcta.

   ### Dificultades encontradas
   - [Dificultad 1]
		Durante el despliegue y rollback de versiones en Kubernetes con MicroK8s, se identificaron dos problemas comunes:
		Uso de imágenes cacheadas, que impedía que los cambios se reflejaran correctamente. Se resolvió eliminando la imagen local antes del despliegue o rollback.
		Pods que no se reinician automáticamente, solucionado forzando su eliminación manual mediante etiquetas para que Kubernetes los recree con la versión correcta.
	 **Problemas:**
		1.	MicroK8s usa imagen cacheada
			Aunque hacia rollback, si la versión anterior ya no está local y MicroK8s encuentra la misma imagen en cache, puede no actualizar correctamente.
			Solución: eliminar la imagen local antes de hacer rollout:
		 	```
			microk8s ctr images rm docker.io/abflores/springboot-api:v2.0
			```
		3.	Pods no se reinician
			Después de un rollback, Kubernetes debería crear nuevos pods automáticamente.
			Si los pods antiguos permanecen con la versión vieja, puedo forzar el reinicio:
		 ```
		 microk8s kubectl delete pods -l app=api -n proyecto-integrador
		```
		Este proceso permitió entender cómo interactúan MicroK8s, MetalLB, Ingress, Deployments y Pods dentro de un entorno local de pruebas y cómo mantener control sobre las versiones desplegadas.
   ### Reflexión
    
   ```
   En un proyecto real, estos aprendizajes son muy valiosos para mantener la consistencia entre el código, las imágenes Docker y los pods en ejecución.
	**Aplicaría estos conocimientos para:**
	
	Asegurar despliegues confiables en entornos de integración o preproducción.
	Automatizar la actualización y limpieza de imágenes con pipelines CI/CD.
	Diagnosticar rápidamente si un problema proviene de una imagen obsoleta o de un despliegue mal aplicado.
	Implementar buenas prácticas de versionado y control de rollouts para evitar interrupciones en servicios productivos.
	En resumen, dominar estos comandos y flujos me permite trabajar de forma más profesional con Kubernetes, asegurando que los entornos locales reflejen fielmente lo que se ejecutará en producción.
	```
	
3. **Directorio screenshots/**
   - Crear directorio `screenshots/` en la raíz
   - Guardar TODOS los screenshots ahí
   - Nombrarlos siguiendo el patrón: `parteX-descripcion.png`
