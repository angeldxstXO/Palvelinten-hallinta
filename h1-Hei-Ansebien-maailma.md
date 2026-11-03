# x) Lue ja tiivistä
## SSH public key
  - korvaa tavallisen salasanan käytön, voi olla myös turvallisempi vaihtoehto
  - lataus ja päälle kytkeminen komennoilla `sudo apt-get -y install ssh` ja `sudo systemctl enable --now ssh`
  - julkinen SSH avain löytyy `$HOME/.ssh/id_*.pub` hakemistosta

## Hello Ansible
  - Ansible on palvelin hallinnan työkalu, joka toimii SSH:n kautta
  - Ohjattavat koneet tarvitsevat vain SSH daemonin sekä Pythonin toimiakseen
  - Ansiblen voi ladata `sudo apt-get install ansible` komennolla
  - Luomalla hosts.ini tiedoston, voidaan sinne lisättyjä käyttäjiä ohjata samaan aikaan käyttämällä komentoa `ansible all [command] hosts.ini`

tein kohdan a) jo tehtävssä h0: https://github.com/angeldxstXO/Palvelinten-hallinta/blob/main/h0-Oikeasti-netiss%C3%A4.md

# b) Sshecrets
Sisäänkirjautumisen jälkeen avasin Terminaalin ja aloitin SSH:n latauksen komennoilla `sudo apt-get update` ja `sudo apt-get -y install ssh`.

`sudo systemctl enable --now ssh` komento käynnistää SSH:n ja varmistaa, että se on päällä myös automattisesti jatkossa.

Kirjauduin sisään `ssh localhost` avulla, jonka jälkeen näemme, että painamalla `w` käyttäjällä on SSH yhteys.
<img width="961" height="94" alt="image" src="https://github.com/user-attachments/assets/8b609b1a-2fa7-4ac6-aaef-74402c544aa1" />

# c) Pubkey
Minulla ei ollut valmiiksi luotua avainparia, joten loin sen komennolla `ssh-keygen` ja 3x enter hyväksyäkseni oletusasetukset.

Lisäsin SSH avaimen localhostille `ssh-copy-id localhost` ja pääsin kirjautumaan sisälle ilman salasanaa.
<img width="1177" height="432" alt="image" src="https://github.com/user-attachments/assets/002b7ca9-bd06-43df-a86f-efabd3d16980" />

# d) Hei Ansible

Aloitin Ansiblen asennuksen `sudo apt-get install ansible` komennolla, lisäsin latasin vielä `micro` `bash-completion` `tree` koska nämä extrat auttavat Ansiblen käytössä.

Tein uuden hakemiston ansiblea varten kotihakemistooni `mkdir ansible/` ja siirryin sinne `cd ansible/`. Tämän jälkeen loin tiedoston hosts.ini micron avulla `micro hosts.ini`. Lisäsin tiedostoon "localhost" jotta pääsen sitä hallitsemaan ansiblen avulla. Lisäkis laitoin tiedostoon myös [all:vars]
ansible_python_interpreter=/usr/bin/python3 jotta tulevat komennot eivät valita python versiosta.

Kokeilin tämän jälkeen `ansible all -a uptime -i hosts.ini` joka toimi hyvin.

Halusin vielä tehdä ansible.cfg tiedoston johon laitoin koodin `[defaults] inventory = hosts.ini`. Tämän avulla ei tarvitse enää kirjoittaa `-i hosts.ini`. Sama uptime komento toimii nyt kun käytän vain `ansible all -a uptime`.

Seuraavaksi tein uuden tiedoston `micro site.yml` jonka tarkoituksena on antaa roolit tietokoneille. `- hosts: all` antaa roolin kaikille koneille ja `roles: - hello` on rooli, jonka teen myöhemmin.

Kokeilemalla `ansible-playbook site.yml` komento toimii, mutta koska hello rooli puuttuu, se antaa virheilmoituksen.

Luodaan rooli. Ensin tehdään uusi hakemisto `mkdir -p roles/hello/tasks/` ja sitten tehdään tiedosto `micro roles/hello/tasks/main.yml`.
Kirjoittamalla sinne `- copy: dest: /tmp/hello-ansible content: "Hello Ansible\n"` saadaan rooli luotua.

`ansible-playbook site.yml` komennolla nähdään, että muutokset ovat tallentuneet.

Nyt kun kokeillaan komenota `ssh localhost cat /tmp/hello-ansible` saamme tämän tulostuksen:
<img width="661" height="42" alt="image" src="https://github.com/user-attachments/assets/0158a360-2657-4bbd-9fc0-e8f999380d07" />




# References
Karvinen, T. 2026. SSH public key - Login without password. Luettavissa: https://terokarvinen.com/ssh-public-key-login-without-password/. Luettu: 3.11.2026
Karvinen, T. 2026. Hello Ansible. Luettavissa: https://terokarvinen.com/hello-ansible/. Luettue: 3.11.2026
