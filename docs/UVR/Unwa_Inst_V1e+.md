# Unwa Inst V1e+ -mallin asennus Ultimate Vocal Removeriin (macOS)

Inst V1e+ on unwan Mel-Band Roformer -instrumentaalimalli. Se on viritetty säilyttämään instrumentaali mahdollisimman täyteläisenä (*fullness*), ja hintana on hieman taustakohinaa. Siksi se sopii hyvin elektroniseen musiikkiin, jossa syntikkaliidit ja padit menevät samalle taajuusalueelle kuin laulu.

---

## 1. Asenna UVR:n Roformer-beta

Vakaa UVR 5.6 ei osaa ajaa Roformer-malleja, joten tarvitset beta-version.

1. Lataa [UVR:n GitHub-julkaisusivulta](https://github.com/Anjok07/ultimatevocalremovergui/releases) oikea versio:
   - **Apple Silicon (M1/M2/M3/M4):** `Ultimate_Vocal_Remover_v5_6_Roformer_beta_0115_MacOS_arm64_hf.dmg`
   - **Intel-Mac:** `Ultimate_Vocal_Remover_v5_6_Roformer_beta_0115_MacOS_x86_64.dmg`
2. Vedä sovellus Ohjelmat-kansioon (Applications). Beta käyttää samaa nimeä kuin vanha versio, joten macOS kysyy, korvataanko vanha. Beta tekee kaiken saman kuin vakaa versio, joten korvaaminen on ok.
   - **Ennen korvaamista:** klikkaa vanhaa sovellusta oikealla → *Show Package Contents* (Näytä paketin sisältö). Jos sieltä löytyy `models`-kansio, jossa on lataamiasi malleja, kopioi se talteen. Korvaaminen poistaa kaiken sovelluksen sisältä.
3. Jos macOS ei suostu avaamaan sovellusta, aja Terminalissa:

```bash
sudo xattr -rd com.apple.quarantine /Applications/Ultimate\ Vocal\ Remover.app
```

---

## 2. Lataa mallitiedostot

Mallin tiedostot ovat unwan Hugging Face -repossa: [pcunwa/Mel-Band-Roformer-Inst](https://huggingface.co/pcunwa/Mel-Band-Roformer-Inst/tree/main)

| Tiedosto | Koko | Suora lataus |
|---|---|---|
| `inst_v1e_plus.ckpt` (itse malli) | 913 Mt | [Lataa](https://huggingface.co/pcunwa/Mel-Band-Roformer-Inst/resolve/main/inst_v1e_plus.ckpt?download=true) |
| `config_melbandroformer_inst.yaml` (asetustiedosto) | 906 tavua | [Lataa](https://huggingface.co/pcunwa/Mel-Band-Roformer-Inst/resolve/main/config_melbandroformer_inst.yaml?download=true) |

> ⚠️ Käytä tiedostoa `config_melbandroformer_inst.yaml`. **Älä** käytä samassa repossa olevaa `config_melbandroformer_inst_v2.yaml`-tiedostoa, koska se kuuluu Inst V2 -mallille.

---

## 3. Asenna malli UVR:ssä

1. Valitse **Choose Process Method** → **MDX-Net**.
2. Avaa **Choose MDX-Net Model** -pudotusvalikko ja valitse listan alareunasta **Install Model**.
3. Valitse ladattu `inst_v1e_plus.ckpt`.
   - 💡 Vaihda asennusikkunassa tilaksi **Move Model…** (siirto) kopioinnin sijaan. Siirto samalla levyllä on välitön. Kopiointi taas tapahtuu käyttöliittymän säikeessä, joten ikkuna jumittuu kopioinnin ajaksi ja näytölle tulee rantapallo. Siirron jälkeen tiedosto ei enää ole Lataukset-kansiossa.
4. **Choose Model Param** -ikkunassa valitse **Install New yaml** ja valitse `config_melbandroformer_inst.yaml`.
5. Valitse **Set Model Type** -kohtaan **MelBand-Roformer**. Ota nimenomaan se ilman v2-päätettä, koska v2 on eri mallityypeille tarkoitettu koodipolku.
6. Vahvista. Malli ilmestyy MDX-Net-mallilistaan.

> UVR lisää asennetun yaml-tiedoston nimeen numerosarjan, esim. `config_melbandroformer_inst_1792337619`. Tämä on normaalia.

---

## 4. Suositellut asetukset

| Asetus | Arvo | Selitys |
|---|---|---|
| **Other Only** | ✅ päällä | Mallin asetustiedostossa kohdestemmi on `other`, joten "Other" = **instrumentaali** |
| **No Other Only** | ❌ pois | "No Other" = kaikki muu eli **laulu** |
| **Segment Size** | Default | Käyttää yaml-tiedoston palakokoa (485 100 näytettä ≈ 11 s), jota malli odottaa |
| **Overlap** | 2 (lopulliseen ajoon 4) | 2 on mallin oletus (50 % päällekkäisyys). Suurempi arvo on hitaampi ja parannus on pieni |
| **GPU Conversion** | ✅ päällä | Käyttää Macin GPU:ta (MPS) |
| **Sample Mode (30s)** | ✅ ensimmäisellä kerralla | Testaa 30 sekunnin pätkällä ennen koko kappaletta |
| **Tiedostomuoto** | WAV | Ei MP3:a, jos raitaa käytetään DAW:ssa tai DJ-editeissä |

Roformer-mallit ovat paljon raskaampia kuin MDX-Net Inst HQ 3, joten käsittely kestää selvästi pidempään.

**Vinkki:** testaa Sample Modella kohta, jossa laulu ja syntikkaliidi soivat päällekkäin. Jos hiljaisissa breakdowneissa kuuluu laulun jäämiä, kokeile vertailuksi puhtaampaa mallia, esim. becruilyn Deuxia.

---

## 5. Vianmääritys: ikkuna jäätyy asennuksessa (rantapallo)

Alle 1 Gt:n tiedoston kopiointi kestää sekunteja. Jos jumi jatkuu yli pari minuuttia, UVR on jumissa tai odottaa jotain:

1. **Etsi piilossa oleva ikkuna:** avaa Mission Control (F3 tai kolmen sormen pyyhkäisy ylös). Jos päävalikon takana on UVR-ikkuna (esim. *Choose Model Param* tai virheilmoitus), hoida se.
2. **Tarkista Activity Monitorista (Toimintojen valvonta):** jos Ultimate Vocal Removerin CPU on lähellä 0 % eikä Levy-välilehdellä näy kirjoitusta, sovellus on jumissa.
3. **Pakota lopetus:** ⌥⌘⎋ (Option+Cmd+Esc) → Ultimate Vocal Remover.
4. **Asenna malli käsin** (ohittaa Install Model -dialogin):

```bash
# Etsi UVR:n mallikansio sovelluspaketin sisältä
find "/Applications/Ultimate Vocal Remover.app" -type d -name MDX_Net_Models

# Liitä tulostettu polku muuttujaan ja katso kansion sisältö
M="<liitä polku tähän>"
ls -lh "$M"

# Jos kansiossa on keskeneräinen inst_v1e_plus.ckpt (alle 913 Mt), poista se
rm "$M/inst_v1e_plus.ckpt"

# Kopioi malli ja asetustiedosto oikeisiin paikkoihin
cp ~/Downloads/inst_v1e_plus.ckpt "$M/"
cp ~/Downloads/config_melbandroformer_inst.yaml "$M/model_data/mdx_c_configs/"
```

5. Käynnistä UVR, valitse malli MDX-Net-listasta. **Choose Model Param** -ikkuna aukeaa, koska UVR ei vielä tunne mallia. Valitse `config_melbandroformer_inst` ja mallityypiksi **MelBand-Roformer**.

Jos UVR jäätyy myös käsin asennuksen jälkeen heti mallia valittaessa, ongelma ei ole kopioinnissa vaan mallin lataamisessa. Ota silloin talteen Activity Monitorin tiedot tai virheilmoituksen teksti.

---

## Linkit

- [UVR – GitHub-julkaisut (Roformer-beta)](https://github.com/Anjok07/ultimatevocalremovergui/releases)
- [UVR v5.6 -julkaisutiedot (asennus ja macOS-ohjeet)](https://github.com/Anjok07/ultimatevocalremovergui/releases/tag/v5.6)
- [pcunwa/Mel-Band-Roformer-Inst – mallin tiedostot](https://huggingface.co/pcunwa/Mel-Band-Roformer-Inst/tree/main)
- [V1e+:n asetustiedosto](https://huggingface.co/pcunwa/Mel-Band-Roformer-Inst/blob/main/config_melbandroformer_inst.yaml)
- [MVSEP – MelBand Roformer -mallien vertailu (fullness/bleedless)](https://mvsep.com/algorithms/49)
- [BS-RoFormer-keskustelu #45 – mallien asennus UVR:ään](https://github.com/lucidrains/BS-RoFormer/discussions/45)
