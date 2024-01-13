# Analiza "game cheating" mehanizama u kompetitivnim video igrama

## Uvod
Od nastanka kompetitivnih video igara, maliciozni igrači su nastojali da pronadju bilo kakvu osetljivost sistema kako bi ostvarili prednost u odnosu na ostale igrače. Ovaj trend je posebno postao popularan kod novijih igara koje su doživljavale nagli porast igrača.
U ovom članku pokušaćemo da analiziramo najpoznatije metode cheat-ovanja, kao i mitigacije kojima se autori igara služe.

Cheat-ovanje poprima različite forme i to u vidu skripti, botova ili nepodržanog načina igre koji se eksploatiše zavisno od funkcionalnosti koje poseduje igra. Zajedničko za svaku od ovih formi jeste da se na neki način ostvari prednost i to je veliki problem kako za ostale igrače, tako i za autore igara. Osim što su ostali igrači nadjačani, ne njihovom voljom, igrači gube lojalnost prema igri te samim tim direktno štete autorima i njihovim prihodima. Sve ovo je uzrokovalo da se kompanije koje razvijaju video igre ozbiljno posvete odbrani od svih poznatih vidova cheat-ovanja te tako kreiraju svoje anti-cheat sisteme. Naravno anti-cheat sistemi se razlikuju od kompanije/video igre i ne postoji univerzalno rešenje za svaku vrstu igre. Najpoznatiji anti-cheat sistemi, koje ćemo detaljno opisati u daljem tekstu, jesu **VAC (Valve)**, **Easy Anti-Cheat (Epic Games)** i **Vanguard (Riot Games)**.

## Arhitektura cheat-ovanja
Kako bismo bolje razumeli na koji način funkcioniše cheat-ovi i na kojem arhitekturalno nivou sistema se odvijaju, analiziraćemo sistem privilegija koji se odvija u kernelu(procesoru).

<p align="center">
    <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Analiza%20pretnji%20kroz%20analizu%20biblioteka%20za%20razvoj/Slike/ModelPretnji.png"/>
</p>
<p align="center">
   CPU slojevi privilegija
</p>
