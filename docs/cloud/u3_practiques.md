!!! note "IMPORTANT"
    Aquestes pràctiques no són obligatòries, però sí recomanables per a poder realitzar els entregables (Aules).
    
    Per començar a fer aquestes pràctiques, es imprescindible haver  completat les de la unitat 2.
---
## Pràctica 1 - Disseny mínim amb VPC + Subnet + IGW

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


---

## Pràctica 2 - XXX
