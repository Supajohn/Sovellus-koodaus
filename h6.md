# h6 - Onkohan tämä turvallinen käyttää?
- Tehtävän ideana on tutkia Tapo C200 kameran ohjelmiston turvallisuutta jo opituilla menetelmillä. Tässä yritän selvittämään ohjelmiston eri rakennetta ja sen tiedostoja. Myös muuta liittyvää kuten ohjelmiston salasanaa ja salaukseen liittyviä toimintoja.
  Tehtävä on suoritettu käyttämällä file, binwalk, strings, grep, DD:tä ja unsquashfs.
  Tarkoituksena on selvittää erilaiset ohjelmiston haavoittuvuudet ja turvallisuuteen liittymät ongelmat. Samalla saada salasana selviksi.


# Tehtävä

- Latasin Tapo C200 dump tiedoston ja aloitin katsomalla millainen tiedosto on kyseessä file -komennolla. Sitten ajettiin binwalk -komento paikannettiin tiedoston eri osia eli kuten bootloader, kernel ja rootfs.
<img width="1132" height="231" alt="image" src="https://github.com/user-attachments/assets/e7fbda11-037b-4cb7-ac8c-bc80103f2091" />

- Strings komennolla tarkistin, että onko koko firmware-tiedosto salattu. Mutta kävi ilmi ettei se ollukaan. Löydetyt neljä riviä oli vaan satunnaisia tavusarjoja.
<img width="1107" height="147" alt="image" src="https://github.com/user-attachments/assets/76375721-0593-4e6e-a554-5c23eeeff050" />

- Tehtävän tiedoston purkamiseen ei tarvittu tp-link-decryptiä sillä kyseessä ei ollut TP-linkin ladattava / salattu päivityspaketti.

<img width="1407" height="136" alt="image" src="https://github.com/user-attachments/assets/7e8e8e9f-e5d8-424b-b070-e49068492c19" />
- Sitten katsottiin -strings komennolla vielä tarkemmin hyödyntäen grep:iä huomiona "bootargs=" jolla suodatettiin mitä tiedosto käyttää käynnistyessä. Kun binwalk siis kertoo missä eri osat on tiedostossa niin bootargs antoi tiedon suoraan, koska se on selkokielisenä suoraan flashissä.
- Kaksi riviä tuli ilmille, jossa rootfstype=squashfs tuli ilmille.


-
<img width="1417" height="451" alt="image" src="https://github.com/user-attachments/assets/f9a00386-9520-48fe-815a-24dbeb42b0f5" />
- Leikkasin -dd komennolla tässä kohdassa dumpista kohdan jonka aikasemmin binwalk -komento oli antanut. Eli leikattu kohta on juuri tuo aikasemmin mainittu rootfs.squashfs osio tiedostossa.
Jonka jälkeen Unsquashfs -komennolla saadaan perettua tuo leikattu tiedosto hakemistopuuksi levylle. Tällöin sitä pystytään tutkimaan normaalilla työkaluilla eli ls, file ja strings.

<img width="607" height="77" alt="image" src="https://github.com/user-attachments/assets/2ea2c5fe-c6e5-4f16-8693-f36790482aad" />
- Puretun rootfs tiedoston jälkeen kartoitettiin find- komennolla rajoitetulle alueelle yleiskuva hakemistonrakenteesta.
<img width="1412" height="102" alt="image" src="https://github.com/user-attachments/assets/8cb7e98c-433e-4707-9e4e-b368737981b7" />
- Tiedosto on ELF-binääriin muodostuva.

- Lopulta katsottiin, että löytyykö salasanaa tiedostosta.
<img width="900" height="650" alt="image" src="https://github.com/user-attachments/assets/703cae15-6bbc-4c74-adb1-295854b685ff" />

- Salasanaa ei ole tiedostossa ja tiedostossa saatiin selville, että root-salasanaa ei ole tallennettuna staattisena arvona flashiin. Mutta se generoidaan ohjelmallisesti ajamisen aikana. Eli sen takia ei löydetty salasanaa koska sitä ei ole olemassa staattisena siellä. 


# mahdolliset haavoittuvuudet ja niiden hyödyntäminen
- En osaa täysin sanoa, että voisiko niitä haavoittuvuuksia käyttään hyödyksi. Todennäköisesti varmasti kyllä. Omaan mieleen osui eniten tuo salasanan generointi, kun kameralaite tällä tiedostolla liitettäisiin toiseen laitteeseen. Niin ei tietoa miten annettu salasana lasketaan ja käsitellään. Tosin monia riskihavaintoja kyllä tuli ilmi, mutta niiden hyödyksi käyttäminen vaatii varmaan lisää jatkotyötä jotta siitä saisi jotain irti.

- Lähteet

Claude tekoälyä hyödynnetty komentojen ja vinkkien antamisessa. 

https://github.com/robbins/tp-link-decrypt Luettu 12.9.2026

https://tldp.org/HOWTO/html_single/SquashFS-HOWTO/ Luettu 15.9.2026

https://www.geeksforgeeks.org/linux-unix/readelf-command-in-linux-with-examples/ Luettu 15.9.2026

https://www.hacefresko.com/posts/tp-link-tapo-c200-unauthenticated-rce Luettu 15.9.2026
