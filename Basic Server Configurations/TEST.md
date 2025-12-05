
Apache2 virtual hosting med DNS server

Krav: DNS server med både forward och reverse lookup zones

Steg 1: Installation

apt update

apt install apache2 -y

systemctl enable apache2

systemctl status apache2

Steg 2: Konfiguration

mkdir -p /var/www/sida1

mkdir -p /var/www/sida2
---------------------------------------------------------------------
nano /var/www/sida1/index.html
	→ lägg in valfritt innehåll av html

nano /var/www/sida2/index.html
	→ lägg in valfritt innehåll av html
---------------------------------------------------------------------
nano /etc/apache2/sites-available/sida1.conf
	→ lägg till blocket nedan
<VirtualHost *:80>
    ServerName sida1.local
    DocumentRoot /var/www/sida1
</VirtualHost>

nano /etc/apache2/sites-available/sida2.conf
	→ lägg till blocket nedan
<VirtualHost *:80>
    ServerName sida2.local
    DocumentRoot /var/www/sida2
</VirtualHost>
---------------------------------------------------------------------
a2ensite sida1.conf

a2ensite sida2.conf

systemctl reload apache2





Innan vi kan nå respektive sida i webbläsaren behöver nya records läggas in i DNS-servern för zonen local.

I forward zon
	→ sida1	IN	A	ipadress
	→ sida2	IN	A	ipadress

I reverse zon
	→ ipadress	IN	PTR	sida1.local.
	→ipadress	IN	PTR	sida2.local.

! Glöm inte att ändra serienummer och applicera ändringar med rndc reload.

Steg 3: Verifiering

Använd valfri webbläsare och gå in på respektive sida!

! Kom ihåg att ändra klientens DNS till DNS serverns IP, annars går sidorna inte att nå
