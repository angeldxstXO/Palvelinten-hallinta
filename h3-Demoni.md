# x) Lue ja tiivistä

## Apache installed with Ansible
  - Kaikki Daemonit toimivat samalla package-file-service periatteella.
  - lataus = sudo apt-get install apache2
  - konfiguraation muokkaus = sudoedit /etc/apache2/...
  - päälle kytkeminen = sudo systemctl restart apache2
  - muutosten jälkeen on tärkeää aloittaa ohjelma uudestaan, jotta muutokset toimivat

## Handlers: running operations on change
  - jos halutaan, että ohjelma käynnistyy uudelleen vain kun siihen tehdään muutoksia, voidaan käyttää `handler` tehtäviä
  - handlers toimivat vain kun niin ilmoitetaan
  - `notify` avainsana ajaa tehtäviä
  - handlers ajetaan siinä järjestyksessä, miten ne on kirjoitettu

## ansible-doc service
  - halltisee etäkoneilla olevia pelveluita
  - toimii välityspalvelimena taustalla olevalle palvelunhallintamoduuliile
  - enabled: halutaanko palvelun käynnistyvän koneen käynnistyessä
  - name: palvelun nimi
  - state: kertoo missä tilassa palvelu tulisi olla. mahdolliset vaihtoehdot `started` `stopped` `restarted` `reloaded`
  - EXAMPLE:\
    <img width="445" height="82" alt="image" src="https://github.com/user-attachments/assets/fdbd64d1-c5c1-498a-9c01-f59a603d6f4c" />

  # y) Hacker News [uutinen](https://news.ycombinator.com/item?id=47168500) 
  - terminaalikäyttöliittymä playbookkia varten
  - jäsentää .yml tiedostot
  - tarjoaa jaetun ruudun isäntäkoneen ja tehtävän välille
  - reaaliaikainen playbook komennon rakennus
  - kaikki terminaalin sisällä

# a) Apassi
  Aloitin apache2 latauksen `sudo install apache2`. Sen jälkeen laitetaan apache päälle `sudo systemctl start apache2`. Tarkistetaan tämä vielä `sudo systemctl status apache2` komennolla. (Joku meni GitHub puolella rikki ja en pysty nyt suoraan liittämään kuvakaappauksia tähän joten käytin prnt.sc nettisivu tästä eteenpäin)\
 https://prnt.sc/EZZtYvrx6mjZ 

  Nettisivun .html tiedosto löytyy /var/www/html/index.html mutta tällä hetkellä se sisältää Apachen oletussivut.  
  Muokataan ensin käyttäjien oikeuksia, jotta voidaan muokata html sivu ilman sudo oikeuksia. Ajetaan komennot: `sudo chown -R "$angeldxst":"$puolukka" /var/www/html`, `sudo find /var/www/html -type d -exec chmod 755 {} +` ja `sudo find /var/www/html -type f -exec chmod 644 {} +`. Nyt tavalliset käyttäjät voivat muokata sivuja ilman sudo oikeuksia.  

  Nyt voidaan mennä muokkaamaan html tiedostoa `micro /var/www/html/index.html`. Poistetaan kaikki vanha teksti ja laitetaan sinne omat tilalle:\
  https://prnt.sc/tUAhVjl3w2MV 

  Nyt jos käydään katsomassa nettisivua osoitteessa http://localhost näemme, että se on muuttunut:\
  https://prnt.sc/xgBLgIYPU-Xj

# b) Moottorix
  Ladataan Nginx `sudo install nginx` ja laitetaan se päälle `sudo systemctl start nginx`. Taas tarkistus `sudo systemctl status nginx` avulla.\
  https://prnt.sc/m0iW2D9-_7T8 

  Nginx html tiedosto on samassa osoitteessa kuin Apachen, ja koska edellisessä kohdassa annoin käyttäjille muokkausoikeudet kaikkiin /var/www/html/ tiedostoihin ja hakemistoihin, ei niitä tarvitse nyt vaihtaa. Kävin hieman muokaamassa nettisivun tekstiä:\
  https://prnt.sc/p8tmI7gKJLaY 
  
# c) Automoottorix
**Poistetaan nginx ennen tämän tehtävän tekoa**
  Luodaan uusi hakemisto Nginxää varten `mkdir nginx`. Luodaan sinne inventory tiedosto `micro inventory.ini` ja sinne sisälle tekstit `[web] localhost ansible_connection=local`.\
  Tehdään tämän jälkeen .yml tiedosto `micro nginx.yml` johon kirjoitetaan seuraavat tiedot:\ 
  https://prnt.sc/yJdtmPRZ1BHt 
  Sitten ajetaan kometo `ansible-playbook -i inventory.ini nginx.ynl --ask-become-pass` annetaan salasana ja sen jälkeen tulostuu seuraava:\
  https://prnt.sc/98uzszPKsXWq 

  Html tiedostoa voi muokata jälleen kerran micron avulla `/var/www/html/index.html`. Nyt voidaan katsoa toimiko koko homma selaimen kautta osoitteessa http://localhost  
  https://prnt.sc/gXGG31e4daUy


  ## References
  - ansible-doc service sivut
  - [news.ycombinator.com](https://news.ycombinator.com/)
  - Karvinen, T. 2026. Apache installed with Ansible - quick notes. Luettavissa: https://terokarvinen.com/apache-ansible/. Luettu: 17/11/2026
  - Ansible Community Documentation. Nov 17, 2026. Handlers: running operations on change. Luettavissa: https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html. Luettu: 17/11/2026
