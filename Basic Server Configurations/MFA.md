# Google MFA för autentisering med SSH

Krav: Google Authenticator behöver vara installerad på en annan enhet

## Steg 1: Installation

apt update && apt upgrade -y

apt install libpam-google-authenticator

## Steg 2: Konfiguration från root

nano /etc/pam.d/sshd
	→ lägg till “auth required pam_google_autenticator.so” i slutet av filen

nano /etc/ssh/sshd_config
	→ lägg till “ChallengeResponseAuthentication yes” 

systemctl restart ssh

Gå ut från root och logga in till användaren som ska erhålla MFA

skriv in "google-authenticator" i terminalen

→ svara med y och skanna QR-kod för att lägga in servern på den andra enheten

→ fortsätt genom prompten, förslagsvis y för alla inställningar

! Om konfiguration redan görs via SSH, säkerställ att den primära inloggningen fortfarande är aktiv för att förhindra utelåsning när MFA används för första gången

## Steg 3: Verifiering

I Windows CMD, använd ssh användernamn@IP-adress
	→ svara yes för fingerprint
		→ logga in med lösenord och verifikationskod

