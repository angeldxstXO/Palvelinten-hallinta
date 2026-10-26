Tein tässä tehtävässä kaiken omalle virtuallikoneelleni!

# x) Lue ja tiivistä

## Palvelimen käyttöönotto
- valitse palvelimen tajoaja (UpCloud, DigitalOcean etc.)
- valitse haluttavat asetukset, tärkeimmät näistä ovat: Sijainti, Palvelimen speksit, Käyttöjärjestelmä ja Kirjautumismenetelmä
- yhteydenotto palvelimeen komennolla 'ssh root@[palveliment ip osoite]'

Tässä mielestäni UpCloud näyttää parhaalta vaihtoehdolta, ja se on suomalainen
  
### Alkuvaiheita
- luo uusi sudo käyttäjä palvelimelle 'adduser [nimi] sudo' komennon avulla
- kotihakemistoon lisääminen ja oikeuksien antaminen komennoilla 'mkdir -p /home/[nimi]/.ssh' ja 'chmod 700 /home/[nimi]/.ssh'

## Domain nimen hankkiminen
- valitse palveluntarjoaja, esim. NameCheap
- etsi haluamasi nimi ja valitse itsellesi sopiva hintaluokka. (HUOM. ehdot kannattaa aina lukea läpi, kaikista halvin ei takaa, että hinta pysyy alhaisena)
- kirjaudu ja maksa
- ostamasi domain nimi löytyy palveluntarjoajan käyttäjäsivuiltasi


# b) Debian 13 asennus VirtualBoxiin

Minulla on VirtualBox jo asennettu, koska käytimme sitä Linux-palvelimet kurssilla, joten käyn tässä läpi nyt vain Debian 13 asennuksen.

## Debian asennus

VirtualBoxissa annoin VM:lle nimen Debain13, 4096 MB RAM, 20 GB Disk Space, 2 CPU:ta ja EFI päälle.

Käynnistin luomani VM:n ja valitsin Graphical Install.
Setupissa pyydetään konfiguroimaan seuraavat teidot:
  - Language (English)
  - Location (US)
  - Keyboard layout (Finnish)
  - Hostname (debian)
  - Domain name (buhbuh)
  - Password 
  - Real name (Jami Väänänen)
  - Username (angeldxst)
  - Partiotion disks, valitsin Guided - use entire disk -> All files in one partition > Finish partitioning -> Write changes on disk? Yes
  - Package manager (Finland, ftp.fi.debian.org)
  - Desktop environment, valitsin tähän Xfce koska oma läppärini on vanha
  
Tämän jälkeen odotin kun Debian latautui koneelle.

# c) Alkutoimet

## Sudo oikeuksien antaminen käyttäjälle
Heti Debianin ladattua avasin Terminaalin ja ajoin komennon sudo apt update. Tässä vaiheessa minulle tuli pieni ongelma, sain virheilmoituksen "user is not in the sudoers file".
Käyttämällä 'su -' komentoa pääsin takaisin root käyttäjälle, ja pystyin siten ajamaan komennon 'sudo adduser angeldxst sudo' ja uudelleenkäynnistyksen jälkeen minulla oli sudo oikeudet omalla käyttäjälläni ja pystyin suorittamaan sudo apt update komennon.

## Palomuurin asennus
Aloitin palomuurin asennuksen komennolla 'sudo apt install ufw'.
Asennuksen jälkeen käynnistin palomuurin käyttämällä 'sudo ufw enable', ja päästämällä ssh:n läpi komennolla 'sudo ufw allow ssh'.
Sitten vielä tarkistin tilanteen 'sudo ufw status' avulla, joka näytti että palomuuri on päällä ja ssh sallittu.

## Apache webpalvelin asennus
Apachen latasin koneelleni komennolla 'sudo apt install apache2 -y'.
Katsoin 'systemctl status apache2' komennolla, että Apache oli päällä.
Vielä lopuksi annoin komennolla 'sudo ufw allow 80/tcp' oikeudet palomuuriille.



  # References
  Niemi 2025: H4 Tehtävät. Luettavissa: https://github.com/AONCODES/linux-course/blob/main/h4.md. Luettu 26.10.2026
  Partanen 2025: Harjoitus 5: Nimekäs: a) Nimi. Luettavissa: https://partanennn.github.io/linux-servers/practices/fifth-week.html. Luettu 26.10.2026
  IREX 2025. Installing Debian 13 on VirtualBox, a Complete Technical Walkthrough. Luettavissa: https://www.irex.aretex.ca/blog/devops/installing-debian-13-on-virtualbox-a-complete-technical-walkthrough. Luettu 26.10.2026
  
