# Prednáška: Cloud Fuckups - 29.04.2025

## Úvod
Táto prednáška sa zaoberá najčastejšími problémami pri používaní cloudových služieb, ich možnými následkami a spôsobmi, ako im predchádzať alebo ich riešiť. Poznámky sú rozdelené do logických celkov s dôrazom na bezpečnostné riziko a možnosti ochrany.

## 1. Kritické bezpečnostné riziká a reakcia

### 1.1. 72-minútové okno na zastavenie útočníka
- **Prečo je to dôležité**: Po prieniku má administrátor typicky 72 minút na detekciu a zastavenie útočníka
- **Blast Radius**: Miera potenciálneho poškodenia organizácie po útoku závisí od:
  - Prístupových práv, ktoré útočník získa
  - Prepojenosti systémov (laterálny pohyb v sieti)
  - Implementovanej segmentácie siete
- **Odporúčanie**: Implementovať včasnú detekciu a automatizované reakcie

### 1.2. Public IP na VM s otvoreným RDP
- **Závažnosť**: Kritická bezpečnostná chyba
- **Dôsledky**: 
  - Aktívne skenovanie internetu na otvorené RDP porty
  - Rekordný čas prekonania pomocou brute force: približne 10 minút
  - 5-znakové heslo je už nedostatočné, aj silné heslo môže byť prelomené v horizonte minút
- **Riešenie**:
  - Používať VPN/Bastion host namiesto priameho prístupu
  - Implementovať viacfaktorovú autentifikáciu
  - Obmedziť prístup na konkrétne IP adresy
  - Použiť Just-In-Time prístup (JIT)

## 2. Problémy s infraštruktúrou a správou

### 2.1. Problém manažmentu vývojových/testovacích prostredí
- **Problém**: Vývojári/testeri často nedbajú na nákladovú efektivitu
  - Subskripcie bez nastavených politík alebo obmedzení
  - Nasadenie zbytočne výkonných a drahých VM
- **Riešenie**:
  - Implementácia Azure Policy na obmedzenie dostupných VM veľkostí
  - Nastavenie rozpočtových limitov na subskripcie
  - Pravidelný monitoring a automatické vypínanie nevyužívaných zdrojov

### 2.2. Nasadenie VM cez wizard vs. infraštruktúra ako kód
- **Problém**: Ručné nasadenie cez wizard vedie k nekonzistentným nastaveniam
- **Moderný prístup**: 
  - Infrastructure as Code (IaC) - Terraform, ARM templates, Bicep
  - Automatické vypínanie strojov po 7 hodinách nečinnosti
- **Odporúčanie**: Implementovať CI/CD pipeline pre nasadzovanie infraštruktúry

### 2.3. Breach Service problém pri vypínaní VM
- **Vysvetlenie**: Pri vypínaní stroja cez wizard môže dôjsť k "breach service" chybe
- **Príčina**: 
  - Konflikt medzi bežiacimi službami
  - Nezvládnuté závislosti, ktoré bránia správnemu ukončeniu
- **Riešenie**:
  - Správne postupné vypínanie závislých služieb
  - Používanie orchestrácie pre správne poradie vypínania

## 3. Monitoring a bezpečnostné nástroje

### 3.1. Microsoft Sentinel
- **Funkcia**: Pokročilý SIEM a SOAR nástroj pre vyhodnocovanie bezpečnostných udalostí
- **Princíp**: Analyzuje zhromaždené logy a aplikuje detekčné pravidlá
- **Dôležité nastavenia**:
  - Správne rozdelenie logov na operatívne a bezpečnostné
  - Optimalizácia nákladov (cena za 1 GB logov: 5.18 USD)

### 3.2. Správa logov a náklady
- **Problém**: Miešanie operatívnych a bezpečnostných logov
- **Dôsledky**:
  - Zbytočne vysoké náklady na logovanie
  - Sťažená analýza bezpečnostných udalostí
- **Best practice**:
  - Oddeliť operatívne logy od bezpečnostných
  - Implementovať rôzne retenčné politiky podľa typu logov
  - Používať filtrovanie na zníženie objemu dát

### 3.3. Defender for Endpoint
- **Rozdiely podľa OS**:
  - Windows: Plná funkcionalita
  - Linux/macOS: Obmedzená funkcionalita, možný prechod do Passive Mode
- **Passive Mode problém**:
  - Chýbajúca sieťová ochrana
  - Bez ochrany v reálnom čase
- **Riešenie**:
  - Ak je to možné, aktivovať Active Mode
  - Pri nekompatibilite implementovať dodatočné bezpečnostné vrstvy

## 4. Dátová bezpečnosť a zálohovanie

### 4.1. Redundancia a imutabilita záloh
- **Princíp**: Ochrana pred neúmyselným alebo škodlivým vymazaním dát
- **Kľúčové koncepty**:
  - Lock Immutability: Uzamknutie úložiska proti modifikácii
  - Soft Delete: Dočasné uchovávanie "vymazaných" dát (typicky 14 dní)
- **Odporúčanie**: Implementovať viacvrstvovú stratégiu zálohovania (3-2-1 pravidlo)

### 4.2. Problém anonymného prístupu k Storage Account
- **Riziko**: Nesprávna konfigurácia môže umožniť prístup k dátam bez autentifikácie
- **Detekcia**: Ak XML odpoveď obsahuje "ResourceNotFound", storage account je pravdepodobne otvorený
- **Prevencia**:
  - Kontrolovať nastavenie "Anonymous access" (predvolene disabled)
  - Pravidelný security audit
  - Implementovať Azure Policy pre vynútenie správnych nastavení

## 5. Sieťová bezpečnosť a vysoká dostupnosť

### 5.1. Application Gateway s Web Application Firewall (WAF)
- **Dôvody nasadenia**:
  - Ochrana pred bežnými webovými útokmi (OWASP Top 10)
  - Centralizovaná správa SSL/TLS
  - Load balancing
- **Režimy WAF**:
  - Detection: Len detekuje a loguje incidenty
  - Prevention: Blokuje škodlivé požiadavky
- **Best practice**: Začať v Detection mode pre elimináciu false positives, potom prejsť na Prevention

### 5.2. DNS v Azure - rôzne dizajny
- **Význam**: Kritický pre vysokú dostupnosť a redundanciu
- **Možnosti**:
  - Azure DNS (verejné a privátne zóny)
  - Custom DNS s Azure VM
  - Hybridné riešenia
- **Odporúčanie**: Implementovať geografickú redundanciu a traffic management

### 5.3. Azure Cluster Quorum a POOL Quorum
- **Cluster Quorum**: 
  - Rieši dostupnosť na úrovni serverov
  - Zabezpečuje, že cluster funguje aj pri čiastočnom výpadku
- **POOL Quorum**:
  - Súvisí s dostupnosťou storage subsystému
  - Ak zlyhá viac ako polovica diskov, cluster prestáva fungovať
- **Odporúčanie**: Správne dimenzovanie clustra a implementácia witness node

## 6. Identity a oprávnenia

### 6.1. Azure Policy Remediation Identity
- **Účel**: Identita potrebná pre automatickú nápravu nesúladu s politikami
- **Typy Managed Identity v Azure**:
  - System Assigned: Viazaná na životnosť konkrétneho zdroja
  - User Assigned: Nezávislá identita s vlastným životným cyklom
- **Best practice**: Používať princíp najmenších potrebných oprávnení (least privilege)

## Záver a odporúčania
- Implementovať automatizáciu nasadenia a správy cez IaC
- Pravidelne auditovať bezpečnostné nastavenia
- Monitorovať náklady a optimalizovať využitie zdrojov
- Dodržiavať osvedčené postupy pre jednotlivé služby
- Pravidelne testovať bezpečnosť a reakciu na incidenty
- Vzdelávať vývojové a operačné tímy o bezpečnostných rizikách v cloude

## Ďalšie zdroje
- Microsoft Azure Well-Architected Framework
- Azure Security Benchmark
- CIS Microsoft Azure Foundations Benchmark
- NIST Cybersecurity Framework
