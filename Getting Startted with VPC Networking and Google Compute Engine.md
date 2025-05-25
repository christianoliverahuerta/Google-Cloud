# Challenge Lab


### [GSP101]

![](https://cdn.qwiklabs.com/GMOHykaqmlTHiqEeQXTySaMXYPHeIvaqa2qHEzw6Occ%3D)

---

Time: 40 minutes<br>
Difficulty: INtroductory<br>
Price: 5 Credits

Quest: [Google Cloud Fundamentals: Core Infrastructure]<br>

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

**Visualiza las subredes**

La red predeterminada tiene una subred en cada región de Google Cloud.

1. En el **menú de navegación** (Ícono del menú de navegación) de la consola de Cloud, haz clic en **Red de VPC** > **Redes de VPC**.

2. Haz clic en **predeterminada**.

3. Haz clic en **Subredes**.

Observa la red **predeterminada** con sus subredes.<br>
Cada subred está asociada con una región de Google Cloud y un bloque privado de CIDR conforme a RFC 1918 para su rango de direcciones IP internas y una puerta de enlace.


**Visualiza las rutas**

Las rutas informan a las instancias de VM y la red de VPC cómo enviar tráfico desde una instancia a un destino, dentro de la red o fuera de Google Cloud. Cada red de VPC incluye algunas rutas predeterminadas para enrutar el tráfico entre sus subredes y enviar tráfico desde instancias aptas a Internet.

1. En el panel izquierdo, haz clic en **Rutas**.

2. En **Rutas eficaces**, haz clic en **Red** y, luego, selecciona **predeterminada**.

3. Haz clic en **Región** y selecciona la región del lab que te asignó.

4. Haz clic en **Ver**.

Observa que hay una ruta para cada subred.<br>
Estas rutas las administras tú, pero puedes crear rutas estáticas personalizadas para dirigir algunos paquetes a destinos específicos. Por ejemplo, puedes crear una ruta que envíe todo el tráfico saliente a una instancia configurada como una puerta de enlace NAT.


**Visualiza las reglas de firewall**

Cada red de VPC implementa un firewall virtual distribuido que puedes configurar. Con las reglas de firewall, puedes controlar qué paquetes tienen permitido trasladarse a qué destinos. Cada red de VPC tiene dos reglas de firewall implícitas que bloquean todas las conexiones entrantes y permiten todas las conexiones salientes.

1. En el panel izquierdo, haz clic en **Firewall**.<br>
Observa que hay 4 reglas de firewall de **entrada** para la red **predeterminada**:

  a) default-allow-icmp<br>
  b) default-allow-rdp<br>
  c) default-allow-ssh<br>
  e) default-allow-internal<br>

