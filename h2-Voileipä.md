# x) Lue ja tiivistä

## Sudo without password

  - sudo oikeudet käyttäjille ilman salasanan käyttöä
  - ensin tulee luoda käyttäjä `sudo adduse [name]` ja uusi ryhmä `sudo groupadd [groupname]` sitten lisätään käyttäjä ryhmään `sudo adduser [name] [groupname]`
  - luodaan uusi sääntö joka mahdollistaa sudo oikeudet ilman salasanaa. `sudo visudo /etc/sudoers.d/sudoless` ja sinne komento `%sudoless ALL = (ALL) NOPASSWD: ALL`
  - testaus toimii kirjautumalla ulos ja sitten takaisin sisään. muista myös `sudo -k` jotta kone unohtaa sudo oikeudet. sitten sudo echo "[teksti]" ja jos kone ei kysy salasanaa, olet onnistunut.

## xkcd 149: Sandwich

  - sudo oikeudet yliajavat muut säännöt ja suorittavat annetut komennot ilman kysymyksiä

## Passwordless Sudo with Ansible

  - ennen aloitusta on hyvä tarkistaa, että ansible toimii ja osaat luoda Sudo without password
  - tulee luoda uusi hakemisto ja .yml tiedosto, `roles/[name]/tasks/main.yml`
  - sinne tulee lisätä group, user, authorized_key sekä copy joihin kaikkiin tarvittavat parametrit
  - become: true lisätään site.yml tiedostoon
  - `ansible-playbook site.yml` kysyy vielä salasanaa. lisätään siihen perään `--ask-become-password`

## Ansible-doc komennot

**ansible-doc copy:
  - `ansible.builtin.copy` käytetään kopioimaan tiedosto tai hakemisto koneelta (local or remote) toiselle koneelle.
  - content: kirjoittaa sisällön suoraan tiedostoon
  - dest: polku johon tiedosto kopioidaan
  - src: paikallisen koneen polku tiedostton, joka kopioidaan etäpalvelimelle
  - owner: määrittää käyttäjän, group: määrittää ryhmän ja mode: määrittää oikeudet
  - EXAMPLE:\
    <img width="751" height="148" alt="image" src="https://github.com/user-attachments/assets/c4e39bde-041f-4fa8-84bb-978743767325" />

**ansible-doc apt:
  - `ansible.builtin.apt` hallitsee APT paketteja
  - name: listaa pakettien nimet
  - state: valitsee paketin version esim. "latest" joka on uusin version
  - update_cache: ajaa saman komennon kuin `apt-get update`
  -EXAMPLE:\
    <img width="334" height="64" alt="image" src="https://github.com/user-attachments/assets/f6550b96-f781-4342-960e-d249b4802334" />

**ansible-doc file:
  - `ansible.builtin.file` asettaa atribuutit tiedostoille, hakemistoille tai symlinkeille
  - path: käsiteltävän teidoston tai hakemiston polku
  - recurse: ottaa määritetyt asetukset käyttöön hakemistossa
  - src: määrittää lähdepolun kun luodaan linkki
  - state: määrittää minkälaiset arvot polulle halutaan antaa
  - owner: omistajakäyttäjä group: omistajaryhmä mode: käyttöoikeudet
  - EXAMPLE:\
    <img width="529" height="118" alt="image" src="https://github.com/user-attachments/assets/78067ea1-d6e7-4a1e-a748-66e26e1314ad" />

**ansible-doc user:
  - `ansible.builtin.user` hallitsee käyttäjiä ja niiden atribuutteja
  - name: nimi joka lisätään, poistetaan tai muokataan
  - create_home: luo kotihakemiston käyttäjälle
  - comment: asettaa kuvauksen käyttäjälle
  - groups: listaa mihin ryhmiin käyttäjä kuuluu
  - shell: määrittää käyttäjän komentotulkin
  - state: kertoo pitääkö käyttäjä olla olemassa
  - system: tekee käyttäjästä järjestelmätilin jos state: present
  - EXAMPLE:\
    <img width="526" height="82" alt="image" src="https://github.com/user-attachments/assets/b0970ea2-8068-4668-bf7d-0da0318e707a" />

**ansible-doc autorized_key:
  - `ansible.builtin.authorized_key` lisää tai poistaa SSH avaimen kyseiseltä käyttäjältä
  - user: käyttäjänimi jonka avaintiedostoja muokataan
  - key: näyttää julkiset SSH avaimet
  - EXAMPLE:\
    <img width="439" height="103" alt="image" src="https://github.com/user-attachments/assets/bebf9ee6-e9f3-4417-b0a4-8195cc7712ee" />


# a) Sudoless
  Aloitin tekemällä uuden käyttäjän ja ryhmän, sekä lisäänällä käyttäjän ryhmään: `sudo adduser puolukka` `sudo groupadd sudoless` `sudo adduser puolukka sudoless`. Uutta käyttäjää tehdessä annoin sille salasanan mutta kaikki muut tiedot jätin oletusmuotoon painamalla ENTER pari kertaa.

  Tämän jälkeen tein uuden säännön joka antaa kaikille "sudoless" ryhmässä oleville sudo oikeudet ilman salasanaa.
  `sudo visudo /etc/sudoers.d/sudoless` luo uuden tiedoston johon laitetaan sääntö `%sudoless ALL = (ALL) PASSWD: ALL`
  Kirjautumalla uudelle käyttäjälle komennolla `ssh puolukka@localhosst` voimme testata toimiiko uusi sääntö vaikka echo komennolla:\
  <img width="385" height="58" alt="image" src="https://github.com/user-attachments/assets/90ec78b8-0978-43de-8c01-9a2f74bb3107" />\
  Ei kysy salasanaa, sudoless siis toimii.

# b) Antero
  Tehdään aluksi uusi hakemisto `mkdir -p roles/puolukka/tasks/` ja sinne uusi tiedosto `micro roles/puolukka/tasks/main.yml`
  Lisätään main.yml tiedostoon seuraava sisältö:\
  <img width="1093" height="325" alt="image" src="https://github.com/user-attachments/assets/86573a4c-149e-4d38-a2a2-62426845f04e" />

  site.yml tiedostoon pitää lisätä become: true sääntö seuraavaa kohtaa varten.
  Nyt kun ajamme `ansible-playbook site.yml` ohjelma kysyy sudo salasanaa:\
  <img width="1155" height="202" alt="image" src="https://github.com/user-attachments/assets/b3e33dd3-2c80-4c0e-9541-53aea0fdf02b" />
  
  Voimme antaa tähän oikeudet käyttämällä `ansible-playbook site.yml --ask-become-pass`.\
  <img width="1161" height="489" alt="image" src="https://github.com/user-attachments/assets/01549f6d-09ab-43a3-ab2b-86cdc97c7c64" />

  Kirjautumista voidaan nyt kokeilla, ja se menee läpi ilman salasanaa:\
  <img width="964" height="217" alt="image" src="https://github.com/user-attachments/assets/d0fbae6f-4b96-4b6f-9387-5c1ae9018200" />

# c) Package

  Luodaan jälleen kerran uusi hakemisto ja tiedosto. Tein uuden hakemiston nimeltä roles/pkg/tasks ja sinne roles/pkg/tasks/main.yml. Halusin tehtävää varten ladata `curl` ja `net-tools` työkalut. Myös site.yml pitää päivittää. Näiden vaiheiden jälkeen tulisi kone näyttää tältä:\
  <img width="577" height="292" alt="image" src="https://github.com/user-attachments/assets/0080a33d-59c0-4bc9-971d-1a5be662683a" />

  Ajetaan playbook komento ja nähdään että se menee läpi:\
  <img width="1159" height="546" alt="image" src="https://github.com/user-attachments/assets/e645e135-b6be-4eac-80bd-71e21428c821" />

  `dpkg -s curl` ja `dpkg -s net-tools` näyttävät, että paketit ovat latautuneet.\
  <img width="427" height="43" alt="image" src="https://github.com/user-attachments/assets/46b7169b-1c7e-4348-929a-ed12edb857f8" />\
  <img width="474" height="58" alt="image" src="https://github.com/user-attachments/assets/a1aa9453-7d86-4fb6-806a-a5deb5b2d1f4" />

# d) File

  Tein filetest.yml tiedoston microlla `micro filetest.yml`:\
  <img width="469" height="283" alt="image" src="https://github.com/user-attachments/assets/60b7856e-9063-45a4-854a-b5df50265ae6" />\
  "0644" tarkoittaa symbolisessa muodossa "-rw-r--r--". Omistaja(root) voi lukea ja muokata tiedostoa, Ryhmä(root) ja muut käyttäjät voivat vain lukea tiedostoa.

# e) Jotain muuta (Command)

Päätin kokeilla Command moduulia jonka avulla halusin kokeilla net-toolsin route -v komentoa.
Tässä tehtiin taas uusi hakemisto ja tiedosto, ja lisättiin rooli Route site.yml tiedostoon.
Tässä on kirjoittamani tiedoston sisältö:\
<img width="496" height="178" alt="image" src="https://github.com/user-attachments/assets/779188b7-c457-4033-bb08-c9fce6b7a947" />

Kun ajamme `ansible-playbook site.yml --ask-become-pass` näemme muutoksen:\
<img width="507" height="120" alt="image" src="https://github.com/user-attachments/assets/324fad2f-7dd3-40ea-b5e6-1fd9008d91f1" />

Ja uudelleen niin näkyy myös tuo Route:\
<img width="1284" height="103" alt="image" src="https://github.com/user-attachments/assets/526d529e-5886-4556-bffc-1fcc248f2c17" />



# References
Karvinen T. 2026. Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/. Luettu: 10/11/2026.\
Karvinen T. 2026. Passwordless Sudo with Ansible. Luettavissa: https://terokarvinen.com/passwordless-sudo-with-ansible/. Luettu: 10/11/2026\
Munroe. 2006. Sandwich. Luettavissa: https://xkcd.com/149/. Luettu: 10/11/2026\
ansible-doc sivut





