# Challenge Lab


### [GSP101]

![](https://cdn.qwiklabs.com/GMOHykaqmlTHiqEeQXTySaMXYPHeIvaqa2qHEzw6Occ%3D)

---

Tiempo: 40 minutos<br>
Dificultad: Introductorio<br>
Precio: 5 créditos

Curso: [Google Cloud Fundamentals: Core Infrastructure]<br>

Last updated: 2022  

---

## Descripción general 

La nube privada virtual (VPC) de Google Cloud ofrece funcionalidad de red a las instancias de máquinas virtuales (VMs) de Compute Engine, los contenedores de Kubernetes Engine y el entorno flexible de App Engine. En otras palabras, sin una red de VPC, no podrás crear instancias de VM, contenedores ni aplicaciones de App Engine. Por lo tanto, cada proyecto de Google Cloud tiene una red predeterminada para ayudarte a comenzar.

Una red de VPC se asemeja a una red física, con la excepción de que se virtualiza dentro de Google Cloud. Es un recurso global que consta de una lista de subredes virtuales regionales en centros de datos, todas conectadas mediante una red de área extensa (WAN) global. Las redes de VPC están aisladas de forma lógica unas de otras dentro de Google Cloud.

En este lab, crearás una red de VPC de modo automático con reglas de firewall y dos instancias de VM. Luego, explorarás la conectividad de las instancias de VM.

**Objectivos**

En este lab, aprenderás a realizar las siguientes tareas:

1. Explorar la red de VPC predeterminada.<br/>
2. Crear una red de modo automático con reglas de firewall.<br/>
3. Crear instancias de VM mediante Compute Engine.<br/>
4. Explorar la conectividad de las instancias de VM.<br/>



## Tarea 1. Explora la red predeterminada
Cada proyecto de Google Cloud tiene una red **predeterminada** con subredes, rutas y reglas de firewall.

### Visualiza las subredes

La red predeterminada tiene una subred en cada región de Google Cloud.

1. En el **menú de navegación** (Ícono del menú de navegación) de la consola de Cloud, haz clic en **Red de VPC** > **Redes de VPC**.

2. Haz clic en **predeterminada**.

3. Haz clic en **Subredes**.

Observa la red **predeterminada** con sus subredes.<br>
Cada subred está asociada con una región de Google Cloud y un bloque privado de CIDR conforme a RFC 1918 para su rango de direcciones IP internas y una puerta de enlace.


### Visualiza las rutas

Las rutas informan a las instancias de VM y la red de VPC cómo enviar tráfico desde una instancia a un destino, dentro de la red o fuera de Google Cloud. Cada red de VPC incluye algunas rutas predeterminadas para enrutar el tráfico entre sus subredes y enviar tráfico desde instancias aptas a Internet.

1. En el panel izquierdo, haz clic en **Rutas**.

2. En **Rutas eficaces**, haz clic en **Red** y, luego, selecciona **predeterminada**.

3. Haz clic en **Región** y selecciona la región del lab que te asignó.

4. Haz clic en **Ver**.

Observa que hay una ruta para cada subred.<br>
Estas rutas las administras tú, pero puedes crear rutas estáticas personalizadas para dirigir algunos paquetes a destinos específicos. Por ejemplo, puedes crear una ruta que envíe todo el tráfico saliente a una instancia configurada como una puerta de enlace NAT.


### Visualiza las reglas de firewall

Cada red de VPC implementa un firewall virtual distribuido que puedes configurar. Con las reglas de firewall, puedes controlar qué paquetes tienen permitido trasladarse a qué destinos. Cada red de VPC tiene dos reglas de firewall implícitas que bloquean todas las conexiones entrantes y permiten todas las conexiones salientes.

1. En el panel izquierdo, haz clic en **Firewall**.<br>
Observa que hay 4 reglas de firewall de **entrada** para la red **predeterminada**:

-  default-allow-icmp<br>
-  default-allow-rdp<br>
-  default-allow-ssh<br>
-  default-allow-internal<br>

**Nota:** Estas reglas de firewall permiten el tráfico de entrada **ICMP, RDP y SSH** desde cualquier parte (0.0.0.0/0), y todo el tráfico **TCP, UDP e ICMP** dentro de la red (10.128.0.0/9). En las columnas **Destinos, Filtros, Protocolos/puertos y Acción** se explican estas reglas.

### Borra las reglas de firewall

1. Selecciona todas las reglas de firewall de red predeterminadas.
2. Haz clic en **Borrar**.
3. Haz clic en **Borrar** para confirmar la eliminación de las reglas de firewall.

### Borra la red predeterminada

1. En el **menú de navegación** (Ícono del menú de navegación) de la consola de Cloud, haz clic en **Red de VPC > Redes de VPC**.
2. Selecciona la red **predeterminada**.
3. Haz clic en **Borrar la red de VPC**.
4. Haz clic en **Borrar** para confirmar la eliminación de la red **predeterminada**. Espera a que se borre la red antes de continuar.
5. En el panel izquierdo, haz clic en **Rutas**. Observa que no hay rutas.
6. En el panel izquierdo, haz clic en **Firewall**. Observa que no hay reglas de firewall.

**Nota:** Sin una red de VPC, no hay rutas ni reglas de firewall.

### Intenta crear una instancia de VM
Verifica que no puedas crear una instancia de VM sin una red de VPC.

1. En el **menú de navegación** (Ícono del menú de navegación), haz clic en **Compute Engine > Instancias de VM**.
2. Haz clic en **Crear instancia**.
3. Acepta los valores predeterminados y haz clic en **Crear**. Se muestra un error en la pestaña **Redes**.
4. Haz clic en **Ver los problemas**.
5. En **Interfaces de red**, observa los errores que indican que no hay redes disponibles.
6. Haz clic en **Cancelar**.

**Nota:** Como se esperaba, no puedes crear una instancia de VM sin una red de VPC.


## Tarea 2: Crea una red de VPC e instancias de VM
Crea una red de VPC para que puedas crear instancias de VM.

### Crea una red de VPC de modo automático con reglas de firewall

Crea una red de modo automático para replicar la red predeterminada.

1. En el **menú de navegación** (Ícono del menú de navegación), haz clic en **Red de VPC > Redes de VPC**.
2. Haz clic en **Crear red de VPC**.
3. En **Nombre**, escribe **mynetwork**.
4. En **Modo de creación de subred**, haz clic en **Automático**. Las redes de modo automático crean subredes en cada región automáticamente.
5. En **Reglas de firewall**, selecciona todas las reglas disponibles. Estas son las mismas reglas de firewall estándar que tenía la red predeterminada. También se muestran las reglas **deny-all-ingress y allow-all-egress**, pero no puedes marcarlas ni desmarcarlas porque están implícitas. Estas dos reglas tienen una prioridad más baja (números enteros más altos indican **prioridades** más bajas), de modo que permiten que se consideren primero las reglas personalizadas, y de SSH, ICMP y RDP.
6. Haz clic en **Crear**. Cuando esté lista la red nueva, observa que se creó una subred para cada región.
7. Explora el rango de direcciones IP de las subredes en **Region 1 y Region 2**.

**Nota:** Si alguna vez borras la red predeterminada, puedes volver a crearla rápidamente. Para ello, deberás crear una red de modo automático como lo acabas de hacer. Después de volver a crear la red, allow-internal cambiará a la regla de firewall allow-custom.


### Crea una instancia de VM en Region

Crea una instancia de VM en la región Region. Seleccionar una región y una zona determina la subred y asigna la dirección IP interna del rango de direcciones IP de la subred.

1. En el **menú de navegación** (Ícono del menú de navegación), haz clic en **Compute Engine > Instancias de VM**.
2. Haz clic en **Crear instancia**.
3. Especifica lo siguiente:

| Propiedad  | Valor | 
|---|---|
| Nombre | mynet-us-vm | 
| Región | Region 1 |
| Zona | Zone 1  |

4. En **Serie**, selecciona **E2**.
5. En **Tipo de máquina**, selecciona **e2-micro (2 CPU virtuales, 1 GB de memoria).**
6. Haz clic en **Crear**.


### Crea una instancia de VM en Region 2

Crea una instancia de VM en la región Region 2.

1. Haz clic en Crear instancia.
2. Especifica lo siguiente y deja los parámetros de configuración restantes con sus valores predeterminados:

| Propiedad  | Valor | 
|---|---|
| Nombre | mynet-r2-vm | 
| Región | Region 2 |
| Zona | Zone 2  |

3. En **Serie**, selecciona **E2**.
4. En **Tipo de máquina**, selecciona **e2-micro (2 CPU virtuales, 1 GB de memoria)**.
5. Haz clic en **Crear**.

**Nota:** Las **direcciones IP externas** de ambas instancias de VM son efímeras. Si se detiene una instancia, las direcciones IP externas efímeras asignadas a la instancia se devuelven al grupo general de Compute Engine y quedan disponibles para que las usen otros proyectos.

Cuando se vuelve a iniciar una instancia detenida, se le asigna una nueva dirección IP externa efímera. De manera alternativa, puedes reservar una dirección IP externa estática, que asigna la dirección a tu proyecto de forma indefinida hasta que la liberes de manera explícita.


## Tarea 3: Explora la conectividad de las instancias de VM

Explora la conectividad de las instancias de VM. Específicamente, trata de acceder con SSH a tus instancias de VM mediante tcp:22 y haz ping a las direcciones IP internas y externas de tus instancias de VM con ICMP. Luego, explora los efectos de las reglas de firewall en la conectividad. Para ello, quita las reglas individualmente.

### Verifica la conectividad de las instancias de VM
Las reglas de firewall que creaste con **mynetwork** permiten el tráfico de entrada ICMP y SSH desde dentro de **mynetwork** (IP interna) y fuera de esa red (IP externa).

1. En el **menú de navegación** (Ícono del menú de navegación), haz clic en **Compute Engine > Instancias de VM**. Observa las direcciones IP internas y externas de **mynet-r2-vm**.
2. En **mynet-us-vm**, haz clic en **SSH** para iniciar una terminal y conectarte.
3. Si aparece una ventana emergente de **autorización**, haz clic en **Autorizar**.

**Nota:** Puedes establecer una conexión SSH debido a la regla de firewall allow-ssh, que permite el tráfico entrante desde cualquier parte (0.0.0.0/0) para tcp:22. La conexión SSH funciona a la perfección porque Compute Engine genera una clave SSH para ti y la almacena en una de las siguientes ubicaciones:

- De forma predeterminada, Compute Engine agrega la clave generada a los metadatos del proyecto o de la instancia.
- Si tu cuenta está configurada para usar Acceso al SO, Compute Engine almacenará la clave generada con tu cuenta de usuario.

De manera alternativa, puedes controlar el acceso a instancias de Linux creando claves SSH y editando los metadatos públicos de claves SSH.


4. Para probar la conectividad con la IP interna de mynet-r2-vm, ejecuta el siguiente comando, en el cual deberás ingresar la IP interna de mynet-r2-vm:

**ping -c 3 <Enter mynet-r2-vm's internal IP here>**

Puedes hacer ping a la IP interna de **mynet-r2-vm** debido a la regla de firewall **allow-custom**.

5. Para probar la conectividad con la IP externa de **mynet-r2-vm**, ejecuta el siguiente comando, en el cual deberás ingresar la IP externa de **mynet-r2-vm**:

**ping -c 3 <Enter mynet-r2-vm's external IP here>**

**Nota:** Puedes acceder con SSH a **mynet-us-vm** y hacer ping a las direcciones IP interna y externa de **mynet-r2-vm** como se esperaba. Otra alternativa es acceder con SSH a **mynet-r2-vm** y hacer ping a las direcciones IP internas y externas de mynet-us-vm, lo cual también funciona.

### Quita las reglas de firewall allow-icmp

Quita la regla de firewall **allow-icmp** y, luego, intenta hacer ping a las direcciones IP interna y externa de **mynet-r2-vm**.

1. En el **menú de navegación** (Ícono del menú de navegación), haz clic en **Red de VPC > Firewall**.
2. Selecciona la regla **mynetwork-allow-icmp**.
3. Haz clic en **Borrar**.
4. Haz clic en **Borrar** para confirmar esta acción. Espera hasta que se borre la regla de firewall.
5. Regresa a la terminal SSH de **mynet-us-vm**.
6. Para probar la conectividad con la IP interna de **mynet-r2-vm**, ejecuta el siguiente comando, en el cual deberás ingresar la IP interna de **mynet-r2-vm**:
