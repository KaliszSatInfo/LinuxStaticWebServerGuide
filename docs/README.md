# LinuxStaticWebServerGuide
## 1.
- Instalcace linuxového webového serveru se statickou IP adresou 
- Obchodní Akademie Uherské Hradiště | Jan Fryšták, Daniela Jarošová, Jan Kalisz 
- Datum zpracování: Květen 2025


## 2.
- Tento projekt se zaměřuje na práci s webovým serverem Apache na systém GNU/Linux.
Hlavním cílem je zprovoznit jednoduchý webový server, který bude dosažitelný na lokální síti pod staticky nastavenou IP adresou. 
- Projekt byl zvolen kvůli své praktické využitelnosti, a poměrné jednoduchosti zpracování

- proč Apache?:
    - zpětná kompabilita a dlouhodobá podpora
    - široce používaný a dobře dokumentovaný
    - dobrá podpora pro statické weby
    - umožňuje rozjet více serverů pomocí virtuálních hostů
    - Open source
    
 Dále popíšeme potřebné předpoklady, kroky instalace, jak ověřit funkčnost a rozdělení práce naší skupiny

Cíl projektu
-zprovoznit Apache serveru v Linuxovém prostředí
-nastavit statickou IP adresu pro přístup 
-vytvořit jednodhucou HTML stránku
-ověřit funkčnost přístupu ke stránce

Potřebné materiály
-Virtuální stroj s Linux distribucí využívající správce bálíčků `apt`
-textový editor (např. nano)
-správná síťová konfigurace ve VirtualBoxu
-připojení k internetu pro instalaci balíčku apt

## 3
1. Instalace apache web serveru
    1.1. Samotná instalace
        ``sudo apt update ``                 <-- aktualizuje dostupné balíčky
        ``sudo apt install apache2``         <-- stáhne balíček apache2
        ``sudo systemctl status apache2``    <-- zkontroluje jestli byl balíček úspěšně instalován

    1.2 Kontrola funkčnosti a řešení problémů
        Pokud apache vrací "active - running" můžeme se přesunout na tvorbu samotné webové stránky
        Pokud ne skusíme

        ``sudo systemctl start apache2``

        A jestli chceme automaticky spouštět službu na staru:

        ``sudo systemctl enable apache2``


2.  Vytvoření HTML stránky
    ``cd  /var/www/html``                   <-- přesun do správné pod-složky
    ``sudo nano index.html``                <-- vytvoření a editování naší HTML stránky

        Príklad jednoduché stránky:
    ``
    <!DOCTYPE html>
    <html>
    <head>
        <title>Testovací web</title>
    </head>
    <body>
        <h1>Apache běží!</h1>
    </body>
    </html>``

3. Nastavení statické IP adresy
    3.1 Zjistíme naše síť. rozhraní
        ``ip a``                                <-- vypíše název našeho síťového rozhraní
        - výstup např. __en0s3__

    3.2 Upravíme síťovou konfiguraci 
        ``sudo nano /etc/network/interfaces``   <-- otevřeme konfiguraci

        Doplníme podle následujícího vzoru (pro rozhraní s jmnénem __en0s3__ ):

        ``
        source /etc/network/interfaces.d/*          

        auto lo
        iface lo inet loopback

        iface enp0s3 inet static
            address 192.168.1.100                       <--
            netmask 255.255.255.0                       <-- Změníme podle našich potřeb
            gateway 192.168.1.1                         <--

        iface enp0s3 inet6 auto                 
        ``                                  
        A uložíme (ctrl o)

    3.3 Restartujeme síťové služby a ověříme změny
        ``sudo systemctl restart networking``   <-- restartuje síť služby
        ``ip a``                                
        ``hostname -I``  

    Alternativně můžeme použít                        
        
4. Testování v síti
- Na jiném zařízení v síťi otevřeme prohlížeč a zadáme IP adresu našeho serveru
- Pokud se zobrazí naše HTML stránka, server je úspěšně funkční

## 4
<checked emoji>zprovoznit Apache serveru v Linuxovém prostředí
<checked emoji>nastavit statickou IP adresu pro přístup 
<checked emoji>vytvořit jednodhucou HTML stránku
<checked emoji>ověřit funkčnost přístupu ke stránce

## 5
Rozdělení práce
[Tabulku bitte]

Jan Fryšták         Sepsání dokumentace
Daniela Jarošová    Grafické a gramatické zkreslení dokumentace, generování souboru PDF
Jan Kalisz          Technické vypracování řešení

## 6
Závěr

- Projekt byl úspěšně dokončen. 

- Překvapení při práci:
    - Stáhnutí špatného balíčku apache (apache místo apache2)

- Alternatiy:
    -Místo výchozího správce rozhraní System-Network, by jsme mohli využít jeho novější verzi, Systemd-Networkd
        +Modernější architektura s více funkcemi
        -O trochu komplexnější
    -Místo Apache mohl být použit jiný webserver, např. Nginx
        +nižší spotřeba paměti
        -menší podpora dokumentace pro začátečníky

- Doporučení: Zkontrolovat umístění síťové konfigurace na vaší distribuci a ujistit se, že adresa je skutečně statická

- Možná rozšíření:
    - HTTPS certifikase (pomocí např. Let`s Encrypt)
    - Zkusit nasadit více webů najednou
    - Automatizovat deployment pomocí skriptu

## 7
Zdroje:

DEBIAN WIKI. Network Configuration. Online. 2004, 2025-04-17. Dostupné z: https://wiki.debian.org/NetworkConfiguration. [cit. 2025-05-29].

DEBIAN WIKI. Systemd-Networkd. Online. 2018, 2025-04-09. Dostupné z: https://wiki.debian.org/SystemdNetworkd. [cit. 2025-05-29].

UBUNTU WIKI. Install and Configure Apache. Online. 2024, 2025-04-09. Dostupné z: https://ubuntu.com/tutorials/install-and-configure-apache#2-installing-apache. [cit. 2025-05-29].






