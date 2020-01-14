### netfilter
Netfilter se encarga de:
- El filtrado de paquetes que afectan al kérnel Linux.
- Herramientas del espacio de usuario. Esta normalmente se utiliza para definir el **ruleset** (conjunto de reglas a aplicar en un equipo). Pero estas reglas las aplica al final el kérnel.

Iptables ha ido evolucionando, pero hay ciertas cosas en las que se ha quedado anticuado. Sobre todo cuando se tiene que encargar de gestionar grandes cantidades de tráfico. Es por lo que se desarrolla nftables.

Proyectos de netfilter:
#### ipfwadm
Software obsoleto.

#### ipchains
Software obsoleto.

#### iptables
Esto, sigue existiendo porque iptables se ha utilziado durante mucho tiempo.

Proporciona funcionalidades sobre todo a nivel de red y un poco a nivel de transporte. Pero en principio, el filtrado que proporciona a niveles más bajos son muy liminados. Por eso surgieron apolicaciones más limitadas pero menos genéricas que amplían a iptables: **ebtables** (para filtrado a nivel de enlace) y **arptable** (arp), dos proyectos que no eran de netfilter.

**ip6tables** iptables para IPv6.

#### nftables
Aunque se sigue manteniendo iptables, se sigue avanzando por aquí, es donde se añaden grandes cambios y funcionalidades. 

Esta herramienta sustituye a iptables, arptables, ebtables e ip6tables. Es mucho más rápido y lo agrupa todo. Los contras son que la sintaxis es totalmente diferente. 

Para facilitar las migraciones de iptables a nftables se ha creado una herramienta de traducción, con limitaciones.

Actualmente se utiliza nftables, aunque a nivel usuario las reglas sean de iptables, porque el directorio iptables está mirando a la alternativa iptables (las alternativas es algo propio de debian que, cuando tiene varias aplicaciones similares, todas miran a una que será la "dominante"). Es decir, que iptables, se inicia sobre nft (que esté mirando a nft es una actualización de Buster, antes miraba a iptables-legacy, que actualmente se puede seguir usando, pero configurando las alternativas de forma manual). 

En el caso de Ubuntu, utiliza los paquetes de iptables/nftables de Debian. Por lo tanto ha tenido que migrar a nftables cuando Debian ha decidido cambiar a nftables.

Y RedHat tiene nftables por defecto en su última versión.


