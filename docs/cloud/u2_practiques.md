!!! note "IMPORTANT"
    Aquestes pràctiques no són obligatòries, però sí recomanables per a poder realitzar els entregables (Aules).
    
    Per començar a fer aquestes pràctiques, es imprescindible haver  completat les de la unitat 1.
---
## Pràctica 1 - Regions i AZs 

🎯 Objectiu: veure regions i AZs en acció.

1. Entrar a la consola d’AWS Academy.

2. Observar el selector de regió (part superior dreta).

3.Identificar en quina regió estan treballant. 

<img src="../../assets/u2/regions_lab.png" alt="Regions lab" class="centered-image-50"/>

!!! note "Atenció"
    AWS Academy sols té disponible la regió US East (N. Virginia) – `us-east-1` i eventualment US West (Oregon) – `us-west-2` o EU (Ireland) – `eu-west-1` per a la majòria de pràctiques.

---

## Pràctica 2 - Creació de la primera instància EC2

Amb el laboratori en funcionament (recordeu que es va vore a la [pràctica 2 de la unitat 1](u1_practiques.md)), i una vegada dins de la consola gràfica, si fem click en la zona superior esquerra, es despleguen totes les categories de serveis disponibles (podeu consultar-les per curiositat).

<img src="../../assets/u2/p2_1.png" alt="captura 1" class="centered-image-50"/>

En la barra de búsqueda teclejarem EC2 i podrem marcar el servei com a favorit per a que es quede a la nostra barra de ferramentes ja que es un servei que gastarem durant tot el curs.

<img src="../../assets/u2/p2_2.png" alt="captura 2" class="centered-image-50"/>

Una vegada seleccionat el servei, se'ns desplegarà el panell de control associat a aquest. Per a començar, li donarem *launch instance*.

<img src="../../assets/u2/p2_3.png" alt="captura 3" class="centered-image-50"/>

El primer que hem de fer es donar-li un nom a l'EC2 i seleccionar la plantilla (AMI) en la qual basarem la nostra EC2. Ara selccionarem el SO i després una AMI entre les que ens ofereix del SO que hem triat. Per no gastar molts diners (ací hi ha que estar constantment pensar en ahorrar), seleccionarem una AMI de la capa gratuïta (free tier). 

<img src="../../assets/u2/p2_4.png" alt="captura 4" class="centered-image-50"/>

!!! info "Amazon Linux AMI"
    L'Amazon Linux AMI es una distribució Linux creada per Amazon basada en CentOS/RHEL. Si es trobeu més còmodes amb Ubuntu, basada en Debian, no hi ha cap problema en seleccionar una Ubuntu, sempre que siga la capa gratuïta.


Quan seleccionem l'AMI, vorem l'AMI ID (que de moment no el gastarem però en un futur si que ho farem) i l'usuari que ve preconfigurat a la màquina. **Cada SO té un nom d'usuari diferent**.

<img src="../../assets/u2/p2_5.png" alt="captura 5" class="centered-image-50"/>

Ara, triarem el tipus d'instància on ens fixarem principalment en la configuració de CPU, RAM i preu per hora. En el nostre cas, agafarem la **gratuita**.

<img src="../../assets/u2/p2_6.png" alt="captura 6" class="centered-image-50"/>

El següent pas es generar una clau privada d'accés o reutilitzar una. De forma predeterminada, al arrancar el laboratori ja se'ns crea una clau anomenada *vockey*. Aquesta clau es el fitxer *.pem* que trobem a l'apartat *AWS Details* en la finestra de creació del lab. Aquest fitxer és la part privada de la clau que necessita el cliente per connectar-se al client (per exemple, mitjançant ssh), ja que la clau pública està instal·lada a la instancia. 
De moment no crearem cap clau ja que gastarem la que ve per defect (vockey). En la següent pràctica s'estudia com accedir per ssh a la instància ja creada i vorem com descarregar-la.

Ara comença un aspecte molt important i és la configuració de seguretat. Es du a terme a través de **grups de seguretat*. Un grup de seguretat no es res més que un firewall que s'associa a la instància EC2 en la que, de forma gràfica, podem configurar regles d'entrada i d'eixida. 

<img src="../../assets/u2/p2_7.png" alt="captura 7" class="centered-image-50"/>

Podem veure que apareix per primera vegada el concepte **VPC** i **subxarxa** que estudiarem a la següent unitat amb detall. Tenint marcada l'opció *Allow SSH Traffic from* i després seleccionat *Anywhere* estem permetent que es puga connectar per SSH desde qualsevol lloc (va per IP). De moment ho deixarem així.

El següent pas és seleccionar l'emmagatzenament del que volem disposar. Per defecte son 8GB que de moment ens sobra.

A la dreta, es apareix el nombre d'instancies que volem llançar amb aquesta configuració. Per defecte és 1 i ho deixarem així.

<img src="../../assets/u2/p2_8.png" alt="captura 8" class="centered-image-50"/>

Una vegada tot configurat, podrem llançar la instància i si tot ha anat bé, ens eixira un missatge indicant-ho.

<img src="../../assets/u2/p2_9.png" alt="captura 9" class="centered-image-50"/>

I si fem click en *Instances* al menú lateral, podrem anar directament al panell de control per veure les nostres instàncies i el seu estat.

<img src="../../assets/u2/p2_10.png" alt="captura 10" class="centered-image-50"/>

Una vegada ha arrancat la instància (tarda una miqueta), podem veure els detalls d'aquesta així com configurar certes coses vem click en ella. Entre els detalls tenim l'adreça pública (està exposada a Internet i per tant serà la que gastarem per connectar-nos a ella), el seu DNS públic (si volem connectar-se utilitzant nom en lloc de IP) així com l'adreça privada (gastada dins de la xarxa), entre altres

<img src="../../assets/u2/p2_11.png" alt="captura 11" class="centered-image-50"/>

!!! tip "Pràctica sugerida"
    Crea una instància basada en Ubuntu, utilitzant sempre capa gratuita. Examina les opcions que tenim per parar-la, apagar-la, etc.

---

## Pràctica 3 - Connexió a una instància EC2


