# U3 - 🔐 Xarxes virtuals i serveis de còmput

**🎯 RA's vinculats**: RA2, RA3

## VPC i subxarxes: concepte i desplegament bàsic

!!! tip "Repàs de xarxes"

    Aquesta unitat dona per assolits els coneixements respecte a xarxes, subxarxes i direccionament. Pots trobar un repàs  [ací](../../suport/repasXarxes). 

### Què és una VPC (Virtual Private Cloud)
En AWS, una **VPC** és una xarxa virtual aïllada dins del núvol on pots definir rangs d’adreces IP, subxarxes, rutes i policies de seguretat. És l’equivalent d’un centre de dades virtual, però amb la flexibilitat d’escala, integració i gestió que ofereix el núvol.

Les VPC serveixen per:

- Aïllar recursos (servidors, bases de dades, serveis) en capes segons les necessitats de seguretat i accés.

- Controlar el tràfic entre recursos i vers l’Internet.

- Aplicar polítiques de seguretat (SG, NACL) a nivell de màquina o de subxarxa.


---

### Subxarxes (Subnets)

Una vegada definida una VPC amb un rang CIDR (per exemple 10.0.0.0/16), aquesta es pot subdividir en subxarxes (subnets).

- **Subxarxa pública**: aquella que està associada a una ruta que passa per un Internet Gateway, de manera que les instàncies amb IP pública poden comunicar-se amb Internet.

- **Subxarxa privada**: no té ruta directa a Internet (o només surt via un NAT / instància NAT) i és ideal per recursos que no han d’estar exposats públicament, com aplicacions, lògica de backend o bases de dades.

Cal decidir quantes subxarxes i quins rangs reservar per a cada funció (web, app, base de dades, etc.).

---

### Internet Gateway (IGW) i rutes 

Un Internet Gateway (IGW) és un component que es connecta a la VPC i permet que les instàncies dins de subxarxes públiques facen tràfic cap a Internet (i reben tràfic, si tenen IP pública).
Sense un Internet Gateway, les VPC's no tenen accés a Internet.

!!! info "VPC default"
    La VPC que ve per defecte en AWS sí que ens permet accés a internet perquè **ja te un IGW adjunt**, no hem hagut de crear-lo explicitament però sí que està.

Els IGW van associats als VPC per possibilitar-los la comunicació a internet (que els permeta no vol dir que ho tinguen, cal habilitar-ho també a les instàncies.)

Per poder fer-ho, cada subxarxa d'AWS té associada una *Route table* que determinen com trauen el tràfic. Molt similar a l'enrutament que coneguem del curs passat.

Les taules de rutes (route tables) defineixen quin “destí” (CIDR) es dirigeix a quin “objectiu” (porta de sortida):

- Exemple: la subxarxa pública tindrà una ruta 0.0.0.0/0 → IGW (apareixerà com a igw-xxx) per enviar tot el tràfic no local a Internet.

- La subxarxa privada pot tenir només rutes internes (cap a altres subnets).

!!! tip "Pràctica sugerida"
    En aquest punt dels continguts, es recomana fer la [pràctica 1 de la unitat 3](u3_practiques.md).

!!! tip "Curs AWS Academy Cloud Foundations "
    Una vegada feta les pràctica 1 s'ha de realitzar el mòdul 5 fins al laboratori 2 (*Redes y entrega de contenido*) del curs d' *AWS Academy Cloud Foundations*.

---



---

!!! tip "Entregable"
    Recorda que en Aules tens l'enunciat de l'exercici entregable.


