# LinuxStaticWebServerGuide
## 1.
Instalace linuxového webového serveru se statickou IP adresou 
- Obchodní Akademie Uherské Hradiště | Jan Fryšták, Daniela Jarošová, Jan Kalisz 
- Datum zpracování: Květen 2025

## 2.
- Tento projekt se zaměřuje na práci s webovým serverem Apache na operačním systému GNU/Linux
- Hlavním cílem je zprovoznit jednoduchý webový server, který bude přístupný na lokální síti pod staticky nastavenou IP adresou. 
- Projekt byl zvolen kvůli své praktické využitelnosti a poměrně jednoduchému zpracování

- Proč Apache?
    - Zpětná kompabilita a dlouhodobá podpora
    - Široce používaný a podrobně dokumentovaný 
    - Podpora statických webů 
    - Umožňuje progoz více serverů zároveň pomocí virtuálních hostů
    - Je Open source


<b>Cíl projektu</b>

- Zprovoznit Apache na Linuxovém serveru
- Nastavit statickou IP adresu pro přístup k webu
- Vytvořit jednodhucou HTML stránku
- Ověřit funkčnost přístupu ke stránce

Potřebné materiály
- Virtuální stroj s Linuxovou distribucí využívající správce bálíčků `apt`
- Textový editor (např. nano)
- Správná síťová konfigurace ve VirtualBoxu
- Připojení k internetu pro instalaci balíčku apt

## 3
1. Instalace apache web serveru
    1.1. Samotná instalace
   <br>
        ``sudo apt update ``                 <-- Aktualizuje dostupné balíčky
   <br>
        ``sudo apt install apache2``         <-- Stáhne balíček apache2
        ``sudo systemctl status apache2``    <-- zkontroluje, jestli byl balíček úspěšně nainstalován

    1.2 Kontrola funkčnosti a řešení problémů
        Pokud apache vrací "active - running", můžeme se přesunout na tvorbu samotné webové stránky.
        Pokud apache neběží, zkusíme:

       sudo systemctl start apache2

    A jestli chceme automaticky spouštět službu na startu:

        sudo systemctl enable apache2

3.  Vytvoření HTML stránky
    ``cd  /var/www/html``                   <-- Přesun do správné pod-složky
    ``sudo nano index.html``                <-- Vytvoření a editace naší HTML stránky

Příklad jednoduché stránky:
    
    <!DOCTYPE html>
    <html>
    <head>
        <title>Testovací web</title>
    </head>
    <body>
        <h1>Apache běží!</h1>
    </body>
    </html>

3. Nastavení statické IP adresy
    3.1 Zjistíme naše síť. rozhraní
        ``ip a``                                <-- Vypíše název našeho síťového rozhraní
        - výstup např. __en0s3__

    3.2 Upravíme síťovou konfiguraci 
        ``sudo nano /etc/network/interfaces``   <-- Otevřeme konfiguraci

    Doplníme podle následujícího vzoru (pro rozhraní s jmnénem __en0s3__ ):

        
        source /etc/network/interfaces.d/*          

        auto lo
        iface lo inet loopback

        iface enp0s3 inet static
            address 192.168.1.100                       <--
            netmask 255.255.255.0                       <-- Změníme podle našich potřeb
            gateway 192.168.1.1                         <--

        iface enp0s3 inet6 auto                 
                                          
   A uložíme (ctrl o)

    3.3 Restartujeme síťové služby a ověříme změny
        ``sudo systemctl restart networking``   <-- Restartuje síť služby
        ``ip a``                                
        ``hostname -I``  
                       
        
4. Testování v síti
- Na jiném zařízení v síťi otevřeme prohlížeč a zadáme IP adresu našeho serveru
- Pokud se zobrazí naše HTML stránka, server je úspěšně funkční

## 4
- ✅ Zprovoznit Apache serveru v Linuxovém prostředí
- ✅ Nastavit statickou IP adresu pro přístup 
- ✅ Vytvořit jednodhucou HTML stránku
- ✅ Ověřit funkčnost přístupu ke stránce

## 5
Rozdělení práce

| Jméno               | Odpovědnost                                                                 |
|---------------------|------------------------------------------------------------------------------|
| Jan Fryšták         | Sepsání dokumentace                                                          |
| Daniela Jarošová    | Grafické a gramatické zkreslení dokumentace, generování souboru PDF         |
| Jan Kalisz          | Technické vypracování řešení                                                 |

## 6
Závěr

- Projekt byl úspěšně dokončen. 

- Překvapení při práci:
    - Stáhnutí špatného balíčku apache (apache místo apache2)

- Alternativy:
    - Místo výchozího správce rozhraní System-Network, by jsme mohli využít jeho novější verzi, Systemd-Networkd
        + Modernější architektura s více funkcemi
        - Komplexní řešení 
    -Místo Apache mohl být použit jiný webserver, např. Nginx
        + Nižší spotřeba paměti
        - Menší podpora dokumentace pro začátečníky

- Doporučení: Zkontrolovat umístění síťové konfigurace na vaší distribuci a ujistit se, že adresa je skutečně statická

- Možná rozšíření:
    - HTTPS certifikase (pomocí např. Let's Encrypt)
    - Zkusit nasadit více webů najednou
    - Automatizovat deployment pomocí skriptu

## 7
Zdroje:

DEBIAN WIKI. Network Configuration. Online. 2004, 2025-04-17. Dostupné z: https://wiki.debian.org/NetworkConfiguration. [cit. 2025-05-29].

DEBIAN WIKI. Systemd-Networkd. Online. 2018, 2025-04-09. Dostupné z: https://wiki.debian.org/SystemdNetworkd. [cit. 2025-05-29].

UBUNTU WIKI. Install and Configure Apache. Online. 2024, 2025-04-09. Dostupné z: https://ubuntu.com/tutorials/install-and-configure-apache#2-installing-apache. [cit. 2025-05-29].
