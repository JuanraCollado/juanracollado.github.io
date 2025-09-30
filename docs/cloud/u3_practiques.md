!!! note "IMPORTANT"
    Aquestes pràctiques no són obligatòries, però sí recomanables per a poder realitzar els entregables (Aules).
    
    Per començar a fer aquestes pràctiques, es imprescindible haver  completat les de la unitat 2.
---
## Juanra - Pràctica 1 - Disseny mínim amb VPC + Subnet + IGW

🎯 Objectiu: En aquesta pràctica inicial, farem un disseny mínim:

- Subxarxa pública: per a màquines que necessiten accés públic (servidors web, bastió). Un bastió és una porta d'entrada colocada en la subxarxa pública que permet connectar-se a una subxarxa privada. D'aquesta forma, quan un administrador vol entrar a, per exemple, un servidor d'aplicació o base de dades, primer es conecta al bastió i des d'allí ja es connectar a les màquines internet. Així sols obrim ports en una màquina i és més senzill monitoritzar.

- Subxarxa privada: per a màquines que no han d’estar exposades (servidors d’aplicació, bases de dades).

<img src="../../assets/u3/p1_9.png" alt="p1_9" class="centered-image-80"/>


### 1. Creació de la VPC

1. A la consola d'AWS Academy, anirem a VPC->Your VPC->Create VPC
2. Omplim els camps: 

    1. *VPC name*: Li indiquem el nom que volem posar-li a la VPC.

    2. *IPv4 CIDR block*: Li indiquem quines són les direccions de xarxa de les que disposarà.

3. Crear la VPC.

<img src="../../assets/u3/p1_1.png" alt="p1_1" class="centered-image-80"/>

### 2. Creació de les subxarxes
1. A la consola d'AWS Academy, anirem a VPC->Subnets->Create Subnet
2. Crearem primer la **subxarxa pública**. Omplim els camps:

    1. *VPC ID*: Li indiquem dins de quina xarxa volem tindre la subxarxa. En el nostre cas serà la que hem creat abans.
    
    2. *Subnet name*: Nom per a la subxarxa

    3. *AZ*: Triarem la AZ. Podem deixar que AWS ho faja per nosaltres (recomanat)
   
    4. *IPv4 CIDR block*. Indicarem quin bloc d'adreces de la xarxa ens quedarem per aquesta subxarxa. En el nostre cas, gastarem la 10.0.1.0/24 per a la pública i la 10.0.2.0/24 per a la privada.

<img src="../../assets/u3/p1_2.png" alt="p1_2" class="centered-image-80"/>

 3.Crearem ara la **subxarxa privada**.

<img src="../../assets/u3/p1_3.png" alt="p1_3" class="centered-image-80"/>

 4.Ja tenim la subxarxa privada i la subxarxa pública creades i disponibles per a utilitzar

<img src="../../assets/u3/p1_4.png" alt="p1_4" class="centered-image-80"/>

!!! tip "Connexió de la VPC"
    Si en aquest punt crearem una instància dins la VPC, aquesta no seria accesible publicament perquè la VPC no té accés al món exterior per defecte, necessite l'IGW.

### 3. Crear i associar l'Internet Gateway
1. VPC-> Internet Gateways -> Create Internet Gateway
    1. Li direm *Lab-IGW*, per exemple.
2. Una vegada creat, selecciona'l i fes clic en Actions->Attach to VPC per associar-lo a la nostra VPC. Selecciona la VPC que hem creat al pas 1.
3. Comprova que el IGW apareix associat al VPC correctament.

<img src="../../assets/u3/p1_5.png" alt="p1_5" class="centered-image-80"/>

!!! tip "Connexió de la VPC"
    Ara que ja tenim l'IGW associat a la VPC, si provem a connectar des de fora seguirà sense deixar-nos. Falta configurar la taula de rutes per a indicar-li a la subxarxa com ha de dirigir el tràfic.


### 4. Crear la taula de rutes per a la subxarxa pública
1. VPC-> Route Tables->Create route table
2. Li posem un nom i seleccionem la VPC a la que l'associarem.
3. Una vegada creada, la seleccionem i Actions->Edit Routes.
4. Afegirem una ruta cap a *Internet* (0.0.0.0/0) i li direm que per arrivar allí, tenim que dirigir el tràfic cap al IGW que hem creat en el pas anterior.
<img src="../../assets/u3/p1_6.png" alt="p1_6" class="centered-image-80"/>

5. Ara cal associar aquesta ruta a la subxarxa pública. Per fer-ho, cal seleccionar la *Route Table* que acabem de crear i anar a la pestanya *Subnet associations* i des de *Edit Subnet Associations* seleccionar la subxarxa pública creada al punt 2. 
<img src="../../assets/u3/p1_7.png" alt="p1_7" class="centered-image-80"/>


<img src="../../assets/u3/p1_8.png" alt="p1_8" class="centered-image-80"/>

!!! info "Route table a la subxarxa privada"
    Totes les subxarxes, per defecte, tenen accés als elements que estan dins de la mateixa VPC. És per això pel que no cal crear una RT a la privada, ja que aquesta no volem que tinga accés a Internet, sino que sols es puga accedir des de la privada.

### 5. Verificació de la pràctica
1. Comprova que la subxarxa pública té accés a internet. Per fer-ho, pots crear una EC2 i fer un ping cap a l'exterior.
2. Comprova que la subxarxa privada no té accés a internet. 
3. Comprova que entre la subxarxa pública i la privada tenen connexió. Recorda que per fer-ho, cal utilitzar les IP's privades ja que ambdues màquines estan dins de la mateixa VPC.
4. Prova a connectar des de el bastió al EC2 de la subxarxa privada per SSH. Si cal, dona-li permisos de SG per poder fer-ho.

!!! info "No borrar aquesta pràctica"
    La pràctica 2 partix de la pràctica 1 per tant s'aconsella no resetejar el laboratori encara.

---

## Pràctica 2 - Ús de NACL's en AWS

En aquesta pràctica treballarem amb Network ACLs (NACLs), un mecanisme de seguretat a nivell de subxarxa dins d’una VPC.

Encara que ja has vist com protegir instàncies concretes amb Security Groups (SG), aquests s’apliquen únicament als punts finals (instàncies). Ara, amb els NACLs, aplicarem una segona capa de seguretat que controla el tràfic per a totes les instàncies d’una subxarxa, independentment del seu SG.

Partim de la següent configuració:

<img src="../../assets/u3/p1_9.png" alt="p1_9" class="centered-image-80"/>

- VPC Lab-VPC (CIDR 10.0.0.0/16  dues subxarxes).
    - Subnet pública: 10.0.1.0/24 amb IGW i una EC2 accessible amb IP pública.
    - Subnet privada: 10.0.2.0/24 amb una EC2 a la qual es pot accedir via bastion.
- Security Groups predeterminats del laboratori.    


Per fer més interessant i didàctic l'exemple, instal·larem un Apache a l'EC2 de la subxarxa privada i autoritzarem el tràfic HTTP (podem fer-ho modificant el SG que ve per defecto o creant un nou.). Açò ja ho hem fet a la unitat anterior per tant anirem directament a la configuració del NACL. 
En el moment que fem que el servidor (subxarxa privada) tinga accés per HTTP, automàticament es converteix en una subxarxa pública. Com hem dit, es sols per e fer la pràctica menys llarga i més didàctica.

Imagina que la teua empresa rep un avís de seguretat: un rang d’IPs està intentant fer atacs SSH de força bruta contra els teu servidor web (l'Apache).

Els teus servidors estan en una subnet privada i ja té SG ben configurat (SSH només des del bastion, HTTP des de qualsevol lloc).

Problema: Necessitem assegurar que, encara que algú modifique per error un SG en el futur, cap instància de la subnet privada accepte tràfic de l’IP sospitosa (203.0.113.25/32).
Això només es pot aconseguir amb un NACL.

### 1. Crear el NACL

1. VPC->Network ACL's->Create NACL
2. Li posem nom: *NACL-Privada*
3. Seleccionem la VPC en la que ens trobem
4. Associem el NACL a la subxarxa privada

<img src="../../assets/u3/p2_1.png" alt="p2_1" class="centered-image-80"/>

### 2. Configuració inicial de les regles NACL

Per a que la nostra NACL no interferisca en el funcionament normal del servidor web com feia fins ara, permetrem tràfic d'entrada SSH sols des del bastió i HTTP des de qualsevol lloc i tràfic d'eixida cap a qualsevol lloc tal i com feia el SG.

1. Inbound rules -> Edit inbound rules -> Add new rule
    - ALLOW TCP/22 des de 10.0.1.0/24
    - ALLOW TCP/80 des de 0.0.0.0/0

<img src="../../assets/u3/p2_2.png" alt="p2_2" class="centered-image-80"/>


!!! info "Rule number"
    Les NACL's, a diferència de les SG, tenen ordre de prioritat. El número que decideix l'ordre de prioritat es el *Rule number*   , sent més prioritari el més xicotet.

2.Outbound rules -> Edit outbound rules -> Add new rule
    - ALLOW ALL -> 0.0.0.0/0

<img src="../../assets/u3/p2_3.png" alt="p2_3" class="centered-image-80"/>

### 3. Bloqueig de la IP sospitosa
Afegim una nova regla en Inbound:

- DENY All tràfic des de 203.0.113.25/32.

Aquesta regla té prioritat, per tant tenim que assegurar-se que el Rule number siga menor que les anteriors. D'aquesta forma
 encara que les altres regles diguen ALLOW, si el paquet ve d’aquesta IP es bloqueja.

<img src="../../assets/u3/p2_4.png" alt="p2_4" class="centered-image-80"/>

### 4. Comprovar el funcionament
1. SSH desde bastió a privada -> OK, estem dins del rang permés.
2. Des de qualsevol màquina, HTTP (per exemple, des del navegador) -> OK. El port 80 està permés per a tots.
3. Com que no podem simular una IP de forma fàcil, confiarem en que si vinguera una connexió d'aquesta direcció es bloquejaria.

!!! info "NACL o SG"
    Els SG són suficients per protegir cas a cas, però no poden fer DENY explícit.
    El NACL pot actuar com un tallafocs de subnet: encara que algú erròniament obri el SG a 0.0.0.0/0, la subnet té un mur per defecte contra IPs sospitoses.

---

## Pràctica 3 (opcional) - Servidor Web Apache + PHP + MySQL

Sobre una VPC nova, crea dos subxarxes públiques i una privada, cadascuna amb una EC2 amb Ubuntu dins.

- Una subxarxa pública serà utilitzada com a bastió i permetrà les connexions SSH de tots.
- L'altra subxarxa pública serà per el servidor web i permetrà les connexions HTTP de tots i el SSH sols des del bastió. Aquest servidor web tindrà instal·lat un Apache+PHP.
- La subxarxa privada tindrà el servidor de base de dades amb un mysql al qual sols es podrà accedir desde el servidor web al port de la BBDD (3306).

Pots trobar a l'apartat [d'instruccions d'interés](instruccions.md) un pas a pas sobre com instal·lar Apache+PHP, com instal·lar el servidor de base de dades MySQL, com connectar ambdues instàncies i com fer-ho des de PHP.

Si ho aconseguixes, tindràs una infraestructura web protegida simple, dinàmica, protegida i resilient.