- Aluksi teen simppelin C ohjelman. Sen sisälle laitettiin mitä se tulee printtaamaan osoitetulla tavalla.

Luodaan se tekemällä nano Hello.cpp

<img width="739" height="247" alt="{6AAA796F-E7B8-482C-A35C-D58476055274}" src="https://github.com/user-attachments/assets/47181762-afc5-449f-a21e-275babfc8e06" />


- Kuvan osoitetulla tavalla sen pitäisi printata "Hello World".

- Sitten kun kootaan G++ avulla niin saadaan printtaus.
- Koodi ajetaan komennolla ./hello

<img width="834" height="232" alt="{F1CE5CE5-7B23-4ACB-9609-35CD3AB97805}" src="https://github.com/user-attachments/assets/0ca98bbc-3eec-4878-9365-a82b08fb0f28" />

- Kuten kuva osoittaa niin saatiin onnistuneesti Hello World printattua yksinkertaisella ohjelmalla.


- Nyt analysoidaan binääriä ja katsotaan minkä tyypin binääriä se on yksinkertaisella komennolla.

<img width="1657" height="89" alt="{B1B8DEC6-ECC2-4E45-A154-D6FD9762FF87}" src="https://github.com/user-attachments/assets/27990229-37a3-4abd-a66b-74002dccd60f" />

- Huomataan että se on ELF formaatilla.

- Sitten vielä katsotaan ohjelmakoodia komennolla objdump -d hello (Rajasin 15 riviin)

<img width="1570" height="594" alt="{132143CB-28E3-4591-9BED-9AF42970BA39}" src="https://github.com/user-attachments/assets/5a36d027-c827-4511-a4aa-e16635d480fb" />




- LÄHTEET

- https://opensource.com/article/20/4/linux-binary-analysis
- https://www.coddykit.com/pages/blog-detail?id=512582&slug=unlocking-the-black-box-a-beginner-s-guide-to-reverse-engineering-binary-analysi
