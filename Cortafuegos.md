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


