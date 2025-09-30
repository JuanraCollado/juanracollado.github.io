# Repàs de Xarxes i Subxarxes

Aquest document resumeix els conceptes principals de xarxes i subnetting.  
Inclou teoria, exemples pràctics i exercicis resolts, així com enllaços a altre material d'interés.

---

## 1. Direccions IP

!!! info "Definició"
    Una **adreça IP** identifica un dispositiu dins d’una xarxa.  
    En **IPv4** són 32 bits → quatre octets (ex. `192.168.1.10`).

**Estructura:**

- **Part de xarxa**: determina la xarxa.  
- **Part d’host**: identifica el dispositiu dins de la xarxa.  

**Exemple:**  
`192.168.1.15` amb màscara `255.255.255.0` (= `/24`):

- Xarxa: `192.168.1.0`  
- Broadcast: `192.168.1.255`  
- Hosts vàlids: `.1 – .254`

---

## 2. Rangs privats i públics

!!! tip "Rangs privats (RFC1918)"
    * `10.0.0.0/8`  
    * `172.16.0.0/12`  
    * `192.168.0.0/16`

- **Privats**: només per a xarxes internes (no rutejables a Internet).  
- **Públics**: qualsevol altra adreça, accessible a Internet.  

---

## 3. Màscares i CIDR

!!! info "CIDR (Classless Inter-Domain Routing)"
    Indica amb `/N` quants bits són la part de **xarxa**.

| CIDR | Màscara decimal   | Nº adreces | Hosts vàlids |
|------|------------------|------------|--------------|
| /24  | 255.255.255.0    | 256        | 254          |
| /25  | 255.255.255.128  | 128        | 126          |
| /26  | 255.255.255.192  | 64         | 62           |
| /27  | 255.255.255.224  | 32         | 30           |
| /28  | 255.255.255.240  | 16         | 14           |
| /29  | 255.255.255.248  | 8          | 6            |
| /30  | 255.255.255.252  | 4          | 2            |

---

## 4. Subnetting

!!! info "Definició"
    **Subnetting** és dividir una xarxa gran en subxarxes més xicotetes.

### Exemple: Dividir en 4 subxarxes `/26`
Xarxa original: `192.168.1.0/24`  
- Nova màscara: `/26` → 64 adreces/subxarxa.  

Resultat:

- `192.168.1.0/26` (hosts .1 – .62)  
- `192.168.1.64/26` (hosts .65 – .126)  
- `192.168.1.128/26` (hosts .129 – .190)  
- `192.168.1.192/26` (hosts .193 – .254)

---

## 5. Routing i gateways

- Una **taula de rutes** indica per on enviar paquets.  
- **Ruta local**: comunicació dins la xarxa.  
- **Default route (0.0.0.0/0)**: tot allò desconegut ix pel *gateway*.  

### Exemple senzill

Host A (192.168.1.10) → Router/GW 192.168.1.1 → Host B (192.168.2.20)


**Passos:**

- Host A envia a 192.168.2.20 → veu que no és local → envia al GW 192.168.1.1.  
- El router coneix la xarxa 192.168.2.0/24 i envia a Host B.  

---

## 6. Exercicis resolts

### Subnetting

**Exercici 1**  
Donada `10.1.0.0/24`, cal subxarxes per a **30 hosts mínim**.  
- Càlcul: 2^N - 2 ≥ 30 → N=5 bits → `/27`.  
- Resultat: 8 subxarxes /27 amb 32 adreces (30 hosts cadascuna).

**Exercici 2**  
Quina és la broadcast de `172.16.8.0/21`?  
- /21 = blocs de 2048 adreces.  
- Rangs vàlids: 172.16.8.0 – 172.16.15.255.  
- ⇒ **Broadcast = 172.16.15.255**.

---

### Routing

**Exercici 1**  
Xarxes:  
- `192.168.1.0/24`  
- `192.168.2.0/24`  
- Router amb interfícies .1 en cada subnet.

Solució:  
- Hosts de 192.168.1.x → gateway 192.168.1.1  
- Hosts de 192.168.2.x → gateway 192.168.2.1  
- El router fa el pas entre ambdues.


---

## 7. Bones pràctiques

- Planificar l’adreçament abans de crear subxarxes.  
- Usar adreces privades quan siga possible.  
- Documentar sempre subxarxes, màscares i gateways.  
- Segmentar serveis crítics (ex. bases de dades en subxarxes diferents).  
- Mantenir **principi de mínim privilegi** en accessos i rutes.

---

## 8. Recursos addicionals

- [Video - Direccionamiento IPv4 y subredes](https://www.youtube.com/watch?v=SHbBso63X38)  
- [RFC 1918 – Direccions privades](https://datatracker.ietf.org/doc/html/rfc1918)  
- [CIDR Notation](https://en.wikipedia.org/wiki/CIDR_notation)  
- [Calculadora de subxarxes online](https://www.ipaddressguide.com/cidr)