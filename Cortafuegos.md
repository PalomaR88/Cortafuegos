# Iptables
## Filtrado de paquetes
- Técnica que permite anañozar el tráfico de red al atraversar un dispositivo y tomar decisiones acerca del mismo: permitir, denegar paso, modificar el tráfco...
- Se usa por seguridad y mejorar el rendimiento.

> Ejemplo de filtrado:
- Analizar el tráfico de un nodo de nuestra red que está provocando problemas.
- Bloquear el tráfico de un nodo de internet que está atacando.
- Bloqueamos el tráfico web de una dirección IP que está haciendo una descarga masiva.
- Permitir la respuesta de ping desde el experior solo a un nodo de nuestra red y limitamos la tasa de solicitud máximo de 3 por segundos.

## Niveles de filtrado
El filtrado de paquetes puede realizarse utilizando parámetros dela diferentes capas TCP/IP.
- Filtrado a nivel de enlace (MAC).
- Filtrado a nivel de red (IP).
- Filtrado a nivel de transporte (puerto, estado de la conexión TCP).
- Filtrado a nivel de aplicación (DNS, FTP, HTTP).

## Filtrado de paquetes en Linux
**iptables**
- Sucesor de ipfwadm e ipchains
- Puede trabajar conjuntamente con:
    * ip6tables (para usar el cortafuego con IPv6)
    * ebtables
    * arptables
- No se desarrollan ya nuevas funcionalidades. 

**bpfilter**
- Permite la utilización de bpf en linux.
- Alto rendimiento.
- Poco maduro.



# Cortafuegos
- Sistema de seguridad que inspecciona el tráfico de red, seleccionando el tráfico conforme a unas reglas establecidas.
- Las aplciaciones de filtrado de paquetes se utilizan como sistema cortafuegos (firewall).

## Cortafuegos con iptables
- Una de las opciones mas extendidas de cortafuego.
- En un equipo convencional con GNU/Linux o en un dispositivo específico.
- iptables proporciona más funcinoes que no son estrictamente cortafuegos:
    * NAT
    * Modificación de los paquetes.


# Esquema de cortafuegos
- **Por nodo**: Ubicamos un cortafuegos delante de cada nodo o en el propio nodo (interno), protegido cada nodo individualmente.
    * Puede proteger tráfico externo e interno.
    * No ha sido la opción en el ámbito corporativo.
    * Vuelve a serlo gracias a la automatización de la configuración y tecnologías asociadas en cloud computing.
    * INPUT, OUTPUT: lo que entra y sale del equipo.
- **Perimetral**: Ubicamos un cortafuegos en cada nodo del perímetro de la red, protegiendo la red del exterior.
    * Centramos la configuración en pocos nodos, en aquel o aquellos que nos conectan a otras redes.
    * Nodos fuertemente configurados y monitorizados.
    * Elección tradicional en ámbito corporativo.
    * FORDWARD: se le llama al paquete que atraviesa un cortafuegos.

**DMZ**
- Zona desmilitarizada.
- Red separada físicamente o lógicamente en la que se ubican los servidores que deben ser accesibles desde otra red.


## Políticas de cortafuego
- Políticas por defecto DROP (se deniega todo y se acepta lo que se quiere)
    * La más utilizada salvo casos excepcionales.
    * Se va habilitando tráfico a medida que se necesita.
    * Cualquieres tráfico nuevo está prohibido.
    * Hay que actualizar las reglas de cortafuego habitualmente.
- Políticas por defecto ACCEPT (se acepta todo y se deniega lo que no se quiere)
    * Inicialmente se permite todo
    * Modo por defecto
    * Se prohíbe cierto tráfico inadecuado
    * Fácil de implementar inicialmente
    * Difícil de implementar de forma segura


## Elementos de iptables
- Modulo del kérnel linux
- Herramientas del espacio de usuario:
    * Paquetes iptables
    * CLI
- Se pueden hacer las siguientes clasificación del tipo de tráfico:
    * Entrante con destino el equipo
    * Saliente, originado en el equipo
    * Que atraviesa el equipo (entra por una interfaz de red y sale por otra)
- Esto, junto a lo que queramos hacer, nos permite agrupar las regas en iptables.
- Existen diferentes tablas (tables) dentro de las cuales peude haber varias cadenas (chains).
- Cada cadena conssite en una lista de reglas con las que se comparan los paquetes que pasan por el cortafuegos. Las reglas específicas quñe se hace por los paquetes que se ajustan a ellas (target): DROP o ACCEPT.
- Las reglas se van evaluando secuenciamente, hasya que el paquete se ajusta a una de ella. Si se llega a final, se ejecuta la política por defecto.

>Ejemplo: Regla para aceptar peticiones tcp en el destino 172.22.0.200 en el puerto destino 80 y la interfaz eth0:
-s/--sport: puerto de origen. Esto es un número alto aleatorio.
-d/--dport: puerto de destino
-o: se puede poner la interfaz

~~~
INPUT -p tcp
    -d 172.22.0.200
    --dport 80
    -o eth0
    -j ACCEPT
~~~

> Regla para permitir que cualquier máquina de fuera pueda acceder a nuestro servido web:
~~~
INPUT -tcp
    --dport 80
    -i eth0 --> esto indica la interfaz de salida.
~~~

~~~
OUTPUT -p tcp
    --sport 80
    -o eth0
    -j ACCEPT
~~~

## Tabla filter:filtro de paquetes
Contiene 3 chains predefinidas:
- INPUT: se consulta par alos paquetes que van dirigidos al propio cortafuegos.
- OUTPUT: PAra paquetes generados localmente.
- FORWARD: La atraviesan los paquetes enrutados a través de esta máquina, es decir, aquellos paquetes en los que el orgien y el destino son equpos de redes diferentes. 

## Establecimiento de una política por defecto
~~~
iptables [-t tabla] -P cadena ACCEPT|DROP
~~~

Establece el taget que se ejecutará para los paquetes que no cumplan con ninguna regla de la chain especificada.

En este apartado utilizamos la tabla filter que es la tabla por defecto y por tanto no es necesario especificarlo.

~~~
iptables -L -nv
~~~
-L: listar
-n: aparece el número del puerto, no el servicio.
-v: verbose. 

1. Limpieza de las reglas previas
~~~
iptables -F
iptables -t nat -F
iptables -Z ---> pone los contadores a 0
iptables -t nat -Z
~~~

2. Permitir ssh
~~~
iptables -A INPUT -s 172.22.0.0/16 -p tcp --dport 22 -jACCEPT
iptables -A OUTPUT -d 172.22.0.0/16 -p tcp --dport 22 -j ACCEPT
~~~

3. Política por defecto
~~~
iptables -p INPUT DROP
iptables -p OUTPUT DROP
~~~



