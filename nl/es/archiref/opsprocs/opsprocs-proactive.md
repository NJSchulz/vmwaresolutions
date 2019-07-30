---

copyright:

  years:  2016, 2019

lastupdated: "2019-07-02"

---

# Tareas proactivas
{: #opsprocs-proactive}

El objetivo de estas comprobaciones proactivas es permitir a los administradores del sistema mantener el entorno del servidor de vCenter en buen estado de salud. Si se llevan a cabo a diariamente, deberían impedir que muchos problemas comunes relacionados con el uso, la capacidad y el rendimiento, tengan impacto en sus cargas de trabajo. Estas comprobaciones proactivas se pueden clasificar en la siguiente estructura:

* Estado: Comprobaciones que indican problemas que afectan actualmente al estado del entorno y requieren atención inmediata para minimizar su impacto, por ejemplo, anomalías de hardware.
* Riesgo: Comprobaciones que indican problemas que no son inmediatos pero se deberían abordar pronto, por ejemplo, problemas de capacidad. Por ejemplo, problemas de capacidad.
* Eficiencia - Comprobaciones que indican áreas donde se podría mejorar el rendimiento o pedir más recursos. Por ejemplo, un dimensionamiento correcto de las máquinas virtuales (VM) y los clústeres.

Muchas de estas tareas proactivas se pueden simplificar con Operations Management on {{site.data.keyword.cloud}} y reducir el trabajo de gestión.

En esta utilización, es importante comprender el concepto "línea base". Esta línea base refleja las operaciones normales del entorno. Cada cliente tiene una línea base distinta según sus prácticas estándares, las cargas de trabajo que se ejecutan en la instancia de vCenter Server, etc. Estas tareas proactivas, pues, comparan el grado de capacidad/rendimiento/utilización con esta línea base. En resumen, estas comprobaciones de utilización dinámicas responden a cuatro preguntas:

1. ¿El grado de utilización es excesivo ahora mismo?
2. ¿Me voy a quedar sin capacidad pronto?
3. ¿El tamaño de mi clúster es demasiado pequeño para los picos de uso esperados?
4. ¿Puedo reclamar recursos no utilizados de mi infraestructura virtual?

## Directrices
{: #opsprocs-proactive-guidelines}

Las siguientes directrices le ayudarán a ser proactivo y a mantener un entorno estable:
* Planifique los despliegues de VM asignando recursos suficientes para todas las VM que tiene previsto ejecutar. No olvide reservar recursos para el propio vSphere ESXi.
* Cree sus VM con el tamaño correcto y asigne a cada VM sólo los recursos virtuales que necesitará. Dar a una VM más recursos de los que necesita puede reducir el rendimiento de esa VM y de otras del mismo host.
* En general, un uso de CPU de host de un 80 % es un límite razonable, y un 90 % debería ser un aviso de que las CPU se aproximan a la condición de sobrecarga. Si sus hosts se acercan al 80 % de uso de CPU, suministre hosts adicionales.
* En general, un uso de RAM del host del 80 % es un límite razonable, y un 90 % debería ser un aviso de que los hosts ya están totalmente utilizados. Si sus hosts se acercan al 80 % de uso de RAM, suministre hosts adicionales.
* Es muy importante no asignar memoria insuficiente, así que asigne memoria suficiente para mantener el conjunto de trabajo de las aplicaciones que tiene previsto ejecutar en las VM, para minimizar la hiperpaginación. La hiperpaginación puede tener un gran impacto en el rendimiento de la aplicación. No obstante, debe evitar también una sobreasignación de memoria.
* Utilice valores, reservas, comparticiones y límites de recursos sólo si se necesitan en su entorno. Si espera que haya cambios frecuentes en los recursos totales disponibles, utilice comparticiones, no reserva, para asignar recursos equitativamente entre las VM. Las comparticiones sólo tienen efecto cuando hay contención de recursos.
* Si necesita utilizar reservas, configúrelas para especificar la cantidad mínima aceptable de CPU o de memoria, no la cantidad que desearía tener disponible.
* Utilice agrupaciones de recursos para la gestión de recursos delegados y para aislar completamente una agrupación de recursos, establezca el tipo de agrupación de recursos en Fijo y utilice Reserva y Límite.
* Agrupe las VM de un servicio multinivel en una agrupación de recursos. Esto permite asignar recursos para todo el servicio en general. Seleccione cuidadosamente los valores de recursos (es decir, reservas, comparticiones y límites) para las VM. Si se establecen reservas demasiado altas, puede dejar pocos recursos no reservados en el clúster, limitando así las opciones que tiene DRS para equilibrar la carga.
* Si se establecen límites demasiado bajos, podría impedir que las VM utilicen recursos extra disponibles en el clúster para mejorar su rendimiento.
* Ejecute sus clústeres a un máximo de 80 % de uso para permitir una corrección de hosts utilizando VMware Update Manager. La corrección de clúster tiene más probabilidades de éxito cuando el clúster se ejecuta a un máximo del 80 % de uso.
* Hay muchos motivos para utilizar el suministro ligero de almacenamiento. No obstante, el suministro ligero aumenta la carga de trabajo de gestión del mantenimiento de su entorno, ya que debe tener cuidado en el proceso de gestión de la capacidad.
*  Analice su crecimiento de VM y entienda cómo se utiliza la infraestructura para soportar este crecimiento, pida capacidad adicional para facilitar este crecimiento.
* La realizar la planificación, utilice la capacidad utilizable, que tiene en cuenta la HA y los almacenamientos intermedios, y no la capacidad disponible total.
* Asegúrese de estar ejecutando la versión más reciente del BIOS disponible para el sistema.
* Asegúrese de estar ejecutando las actualizaciones de VMware más recientes en los productos instalados, esto incluye hardware de VM y herramientas de VM.

## Lista de tareas
{: #opsprocs-proactive-tasks}

Tabla 1. Tareas proactivas

| Título | Descripción |
|---|---|
| Estado  |  Utilizando vCenter, compruebe el estado de todos los hosts y objetos de VM. De forma metódica, revise: clústeres, hosts, almacenes de datos, VM y redes en búsqueda de Alarmas.  |
| Comprobación de alarmas y notificaciones | ¿Hay alguna alarma activa en vCenter de la que no se le haya informado? De forma predeterminada, vCenter tiene un número de alarmas definidas cuando se instala. Estas alarmas pueden alertarle de problemas, sin embargo, de forma predeterminada, no realizan ninguna acción más que un aviso o una alerta en el cliente web de VMware vSphere. Se pueden configurar para que envíen una notificación, como por ejemplo un correo electrónico. A medida que compruebe el estado de los hosts y de los objetos de VM, revise las alarmas, decida si requieren una notificación y configúrelas como considere necesario. Sólo es necesario que se le notifique de los problemas más importantes o críticos. Tengamos en cuenta el caso siguiente: ¿quiero recibir notificación de este asunto a las 2 AM o puede esperar hasta el horario normal de trabajo? Demasiadas notificaciones son consideradas como "ruido" y la naturaleza humana tiende a ignorar todas las alarmas.  |
| CPU del clúster y comprobación de capacidad/utilización de memoria | Revise las medidas de capacidad/utilización para la CPU y la memoria del clúster utilizando vCenter, navegando a cada clúster y seleccionando: Supervisar, Rendimiento. Revise los gráficos y las estadísticas para asegurarse de que el clúster tiene recursos suficientes para satisfacer la demanda. La demanda debe basarse en mantener suficiente capacidad para que las VM se expandan cuando sea necesario, para que falle un host vSphere ESXi y para que las VM se añadan según sus solicitudes de servicio conocidas. |
| Comprobación de capacidad/utilización del almacén de datos  |  Revise las métricas de capacidad/utilización de los almacenes de datos utilizando vCenter, navegando a cada almacén de datos y seleccionando: Supervisar, Rendimiento, Espacio. Revise los gráficos y las estadísticas para asegurarse de que el almacén de datos tiene espacio suficiente para satisfacer la demanda. La demanda debe basarse en mantener suficiente capacidad para que falle un host vSphere ESXi (para un clúster vSAN) y para que las VM se añadan según sus solicitudes de servicio conocidas. |
| Comprobación de rendimiento del almacén de datos  |  Revise las métricas de rendimiento de los almacenes de datos utilizando vCenter, navegando a cada almacén de datos y seleccionando: Supervisar, Rendimiento, Rendimiento, Tiempo real. Revise los gráficos y las estadísticas para asegurarse de que la línea base de rendimiento de su almacén de datos es la esperada y que los cambios se pueden justificar. Investigue cualquier anormalidad. |
| Firmware de servidor nativo | Se recomienda instalar las últimas actualizaciones de firmware para sus hosts de servidor nativo. Compruebe si hay actualizaciones de firmware en el servidor nativo yendo a la página de gestión remota en `control.softlayer.com` de cada host y seleccione recarga de SO. En la página que se visualiza, revise la versión actual y la versión de actualización para las unidades de placa del sistema y de disco duro, y compruebe si hay actualizaciones disponibles. Si las hay, prevea actualizar el firmware en su próximo período de mantenimiento. Para obtener más información, consulte [Preguntas frecuentes: servidores nativos](/docs/bare-metal?topic=bare-metal-bm-faq#what-if-my-bare-metal-server-has-out-of-date-firmware). |
| Parches de vSphere ESXi | Para obtener información sobre cómo comprobar la disponibilidad de los parches de vSphere ESXi, consulte [Creación de líneas base y conexión a objetos de inventario](/docs/services/vmwaresolutions?topic=vmware-solutions-vum-baselines#vum-baselines). |
| Actualizaciones de hardware de VM | Para obtener información sobre cómo comprobar la disponibilidad de las actualizaciones de hardware de VM, consulte [Creación de líneas base y conexión a objetos de inventario](/docs/services/vmwaresolutions?topic=vmware-solutions-vum-baselines#vum-baselines). |
| Actualizaciones de las herramientas de VM | Para obtener más información, consulte [Creación de líneas base y cómo adjuntarlas a objetos de inventario](/docs/services/vmwaresolutions?topic=vmware-solutions-vum-baselines#vum-baselines). |
| Parches de vSphere vSAN  | Para obtener información sobre cómo comprobar la disponibilidad de los parches de vSphere vSAN y el proceso de parches, consulte [Actualización de clústeres vSAN](/docs/services/vmwaresolutions?topic=vmware-solutions-vum-updating-vsan#vum-updating-vsan). |
| Parches de vCenter | Para obtener más información sobre cómo comprobar la disponibilidad de los parches de VCSA y aplicar la actualización, consulte [Actualización de VCSA y vCenters enlazados con SSO](/docs/services/vmwaresolutions?topic=vmware-solutions-vum-updating-vcsa#vum-updating-vcsa). |
| Actualización de NSX | Para obtener más información sobre cómo comprobar la disponibilidad de los parches de NSX y aplicar las actualizaciones, consulte [Parches de NSX](/docs/services/vmwaresolutions?topic=vmware-solutions-vum-updating-nsx#vum-updating-nsx). |
| Buscar VM sin herramientas de VM | Es una buena práctica instalar herramientas de VM ya que esto permite una mayor interacción con el sistema operativo. p.ej. apagado correcto de la VM. Puede utilizar vCenter para comprobar qué VM no tienen herramientas de VM instaladas. Vaya al clúster, seleccione
**Objetos relacionados**, **Máquinas virtuales** y, en la tabla, habilite las columnas
**Herramientas de VM en ejecución** y **Versión de herramientas de VM**. Revise la lista e instale las herramientas de VM según corresponda. |
| VM con instantáneas | Para obtener información sobre las mejores prácticas al trabajar con instantáneas, consulte [Mejores prácticas para utilizar instantáneas en el entorno vSphere (1025279)](https://kb.vmware.com/s/article/1025279){:new_window}. Es importante identificar la existencia de las VM con instantáneas ya que utilizar una instantánea durante más de 72 horas crea un archivo de instantánea que va creciendo de tamaño y puede hacer que se agote el espacio en la ubicación de almacenamiento de instantáneas y eso puede impactar en el rendimiento del sistema. Para revisar las VM con instantáneas, conéctese al vCenter utilizando el cliente web, seleccione vCenter Server y vaya al separador Objetos relacionados. Pulse con el botón derecho del ratón en los títulos de columna y vaya a la lista Mostrar/Ocultar columnas. En la lista de columnas seleccione la opción Necesita consolidación. Esta columna muestra un resumen de todas las VM que se están ejecutando actualmente. |
| Parches de sistema operativo de AD/DNS | Microsoft Active Directory (AD) / Domain Name Server (DNS) está configurado automáticamente para que sólo se descarguen las actualizaciones. Para obtener más información, consulte [Más limitaciones y consideraciones](/docs/services/vmwaresolutions?topic=vmware-solutions-trbl_limitations#trbl_limitations) para obtener más recomendaciones de actualización. |
| Comprobar latencia de almacenamiento  |  Compruebe la latencia de almacenamiento para entender los cambios de los hosts de vSphere ESXi para acceder a los almacenes de datos. Una latencia demasiado alta hace que se apaguen las aplicaciones que hay alojadas en las VM. En vCenter, acceda a cada uno de los almacenes de datos y, en el separador Rendimiento, revise la latencia media de escritura por VM. |
| Revisar las VM con dispositivos virtuales | Los dispositivos virtuales, como por ejemplo discos CD o disquetes, crean una sobrecarga, por lo tanto, elimine los dispositivos que no sean necesarios para una máquina virtual. |
| Recomendación de capacidad de vSAN | Cuando un dispositivo de capacidad del clúster llega al 80 % de su capacidad, vSAN reequilibra automáticamente el clúster, hasta que el espacio disponible en todos los dispositivos de capacidad está por debajo del umbral. Las siguientes operaciones pueden hacer que la capacidad de disco alcance el 80 % y se inicie el proceso de reequilibrio de clúster: fallos de hardware, hosts vSAN que se ponen en modo de mantenimiento con la opción Evacuar todos los datos, o hosts vSAN que se ponen en modo de mantenimiento con Garantizar la accesibilidad de los datos cuando los objetos asignados PFTT=0 residen en el host. Para proporcionar espacio suficiente para el mantenimiento y la reprotección, y para minimizar el reequilibrio automático de sucesos en el clúster vSAN, considere la posibilidad de mantener un 30 % de capacidad disponible en todo momento. |
| Comprobación de utilización de clúster | Utilizando vCenter, revise cada clúster e identifique los que están a un 50 % o más de utilización de CPU y RAM. Se elige el 50 % como nivel de aviso, para centrar la atención en la expansión potencial de este clúster con hosts adicionales o clústeres. La diferencia entre una utilización del 50 % y un máximo de 80 % o 90 % es el espacio de que dispone para VM adicionales debidas a solicitudes de servicio. Cuando se acerque al límite del 50 %, debe prever las posibles próximas solicitudes y hacer un pronóstico de cuándo se necesitará añadir nuevos recursos. |
| Revisión de consolidación de clústeres | Utilizando vCenter, revise cada clúster e identifique los que están a un 30 % o menos de utilización de CPU y RAM. Se elige el 30 % como nivel de aviso, para centrar la atención en cómo lograr un dimensionamiento correcto de este clúster eliminando hosts o eliminando este clúster y trasladando las VM a otro clúster. |
| Tamaño correcto de las VM sobredimensionadas | Utilice un enfoque simple para asignar el tamaño adecuado a las máquinas virtuales con tamaño excesivo: identificar, perfilar y ajustar, y supervisar las tendencias de la demanda. Utilizando vCenter, identifique las VM grandes a las que se puede dar un dimensionamiento correcto. Vaya a Supervisar, Rendimiento y ajuste el perfil de demanda promedio de CPU y RAM de la carga de trabajo y ajuste los recursos virtuales. Por último, continúe supervisando las cargas de trabajo para ver que el rendimiento es aceptable. Idealmente, la memoria consumida para la VM debería ser un valor cercano a la memoria utilizada por el SO invitado, más la sobrecarga para ejecutar la VM.|
| Tamaño correcto de las VM infradimensionadas | Utilice un sencillo método para dar un tamaño correcto de las VM infradimensionadas: a. identifique, b. analice y ajuste y c. supervise las tendencias de demanda. Identifique las VM pequeñas que necesitan un tamaño correcto. Ajuste el perfil de demanda promedio de CPU y RAM de la carga de trabajo y ajuste los recursos virtuales. Por último, continúe supervisando las cargas de trabajo para ver que el rendimiento es aceptable. Idealmente, la memoria consumida para la VM debería ser un valor cercano a la memoria utilizada por el SO invitado, más la sobrecarga para ejecutar la VM.|
| Comprobar la compatibilidad de dispositivos de hardware de VM | Utilizando el recurso en línea de compatibilidad de hardware de VMware, compruebe que los recursos de hardware, la red, los dispositivos de almacenamiento, etc. de sus máquinas virtuales están soportados para ese SO. Si no están soportados, cambie a un dispositivo soportado para mejorar la fiabilidad y el rendimiento.  |

## Enlaces relacionados
{: #opsprocs-proactive-links}

* [Operations Management on {{site.data.keyword.cloud_notm}}](/docs/services/vmwaresolutions/services?topic=vmware-solutions-opsmgmt-intro)