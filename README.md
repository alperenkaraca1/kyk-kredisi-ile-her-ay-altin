# kykkredisiileherayaltin
//Kyk kredisi ile her ay altın alındığı senaryoda 4 sene sonra neler olacağını gösteren program

#include <stdio.h>
int main() {

int baslangicYili, baslangicAyi;
int ay;
int i;

/* KYK */
float baslangicKYK, aylikKYK;
float kykYillikArtis, aylikKYKArtis;
float toplamKredi = 0.0f;

/* ASGARI UCRET */
float asgari4 = 0.0f;
float asgari6 = 0.0f;
const float ASGARI_ARTIS = 0.25f;   /* %25 SABİT */

/* Altın - iki senaryo */
float altinBaslangic;
float altinKotu, altinIyi;
float aylikArtisKotu, aylikArtisIyi;
float yillikAltinKotu, yillikAltinIyi;
float gramKotu = 0.0f, gramIyi = 0.0f;
float gramKotu4 = 0.0f, gramIyi4 = 0.0f;
float gramKotu6 = 0.0f, gramIyi6 = 0.0f;
float fiyatKotu4 = 0.0f, fiyatIyi4 = 0.0f;
float fiyatKotu6 = 0.0f, fiyatIyi6 = 0.0f;

/* Araba / Ev */
int arabaVar, evVar;
float arabaFiyati = 0.0f, evFiyati = 0.0f;
float araba4 = 0.0f, araba6 = 0.0f;
float ev4 = 0.0f, ev6 = 0.0f;

/* ---------- GİRİŞ ---------- */

printf("Baslangic yili: ");
scanf("%d", &baslangicYili);

printf("Baslangic ayi (1-12): ");
scanf("%d", &baslangicAyi);

printf("Baslangic aylik KYK (TL): ");
scanf("%f", &baslangicKYK);

printf("KYK yillik artis orani (Tahmini : %%25-%%35): ");
scanf("%f", &kykYillikArtis);

printf("Gram altin baslangic fiyati (TL): ");
scanf("%f", &altinBaslangic);

printf("\nAltin yillik artis oranlarini giriniz:\n");
printf("Kotu senaryo (Tahmini: %%60- %%70): ");
scanf("%f", &yillikAltinKotu);

printf("Iyi senaryo (Tahmini: %%45-%%55): ");
scanf("%f", &yillikAltinIyi);

printf("\nAsgari ucret yillik artis orani SABIT olarak %%25 alinmistir.\n");

printf("\nAraba almak istiyor musunuz? (1=Evet 0=Hayir): ");
scanf("%d", &arabaVar);
if (arabaVar) {
    printf("Su anki araba fiyati (TL): ");
    scanf("%f", &arabaFiyati);
}

printf("Ev almak istiyor musunuz? (1=Evet 0=Hayir): ");
scanf("%d", &evVar);
if (evVar) {
    printf("Su anki ev fiyati (TL): ");
    scanf("%f", &evFiyati);
}

/* ---------- ORANLAR ---------- */

aylikKYK = baslangicKYK;
aylikKYKArtis = kykYillikArtis / 12.0f / 100.0f;

altinKotu = altinBaslangic;
altinIyi = altinBaslangic;

aylikArtisKotu = yillikAltinKotu / 12.0f / 100.0f;
aylikArtisIyi  = yillikAltinIyi  / 12.0f / 100.0f;

/* ---------- 72 AYLIK DÖNGÜ ---------- */

for (ay = 1; ay <= 72; ay++) {

    if (ay <= 48) {
        toplamKredi += aylikKYK;
        gramKotu += aylikKYK / altinKotu;
        gramIyi  += aylikKYK / altinIyi;
        aylikKYK *= (1.0f + aylikKYKArtis);
    }

    altinKotu *= (1.0f + aylikArtisKotu);
    altinIyi  *= (1.0f + aylikArtisIyi);

    if (ay == 48) {
        gramKotu4 = gramKotu;
        gramIyi4  = gramIyi;
        fiyatKotu4 = altinKotu;
        fiyatIyi4  = altinIyi;
    }

    if (ay == 72) {
        gramKotu6 = gramKotu;
        gramIyi6  = gramIyi;
        fiyatKotu6 = altinKotu;
        fiyatIyi6  = altinIyi;
    }
}

/* ---------- ASGARI UCRET HESABI ---------- */

int yil4 = baslangicYili + 4;
int yil6 = baslangicYili + 6;

/* 4. YIL */
if (yil4 == 2024) asgari4 = 17002;
else if (yil4 == 2025) asgari4 = 22104;
else if (yil4 == 2026) asgari4 = 28075;
else {
    asgari4 = 28075;
    for (i = 0; i < yil4 - 2026; i++)
        asgari4 *= (1.0f + ASGARI_ARTIS);
}

/* 6. YIL */
if (yil6 == 2024) asgari6 = 17002;
else if (yil6 == 2025) asgari6 = 22104;
else if (yil6 == 2026) asgari6 = 28075;
else {
    asgari6 = 28075;
    for (i = 0; i < yil6 - 2026; i++)
        asgari6 *= (1.0f + ASGARI_ARTIS);
}

/* ---------- DEĞERLER ---------- */

float degerKotu4 = gramKotu4 * fiyatKotu4;
float degerIyi4  = gramIyi4  * fiyatIyi4;
float degerKotu6 = gramKotu6 * fiyatKotu6;
float degerIyi6  = gramIyi6  * fiyatIyi6;

/* ---------- ARABA / EV ---------- */

if (arabaVar) {
    araba4 = arabaFiyati;
    araba6 = arabaFiyati;
    for (i = 0; i < 4; i++) araba4 *= 1.25f;
    for (i = 0; i < 6; i++) araba6 *= 1.25f;
}

if (evVar) {
    ev4 = evFiyati;
    ev6 = evFiyati;
    for (i = 0; i < 4; i++) ev4 *= 1.30f;
    for (i = 0; i < 6; i++) ev6 *= 1.30f;
}

/* ---------- ORTAK NET KAR HESAPLARI ---------- */

float net4 = degerIyi4 - toplamKredi;
float net6 = degerIyi6 - toplamKredi;

float arabaOran4 = 0.0f, arabaOran6 = 0.0f;
float evOran4 = 0.0f, evOran6 = 0.0f;

if (arabaVar) {
    arabaOran4 = (net4 / araba4) * 100.0f;
    arabaOran6 = (net6 / araba6) * 100.0f;
}

if (evVar) {
    evOran4 = (net4 / ev4) * 100.0f;
    evOran6 = (net6 / ev6) * 100.0f;
}


/* ---------- SONUÇ ---------- */

printf("\n=========== 4. YIL (%d) ===========\n", yil4);
printf("KYK Borcu: %.2f TL\n", toplamKredi);
printf("Asgari Ucret: %.2f TL\n", asgari4);

printf("\n[KOTU SENARYO]\nAltin Degeri: %.2f TL\nNet Kar: %.2f TL\n",
       degerKotu4, degerKotu4 - toplamKredi);

printf("\n[IYI SENARYO]\nAltin Degeri: %.2f TL\nNet Kar: %.2f TL\n\n\n",
       degerIyi4, degerIyi4 - toplamKredi);

if (arabaVar) printf("Araba Fiyati: %.2f TL\n", araba4);
if (evVar)    printf("Ev Fiyati: %.2f TL\n", ev4);

printf("\n--- 4. YIL MANTIK DEGERLENDIRMESI ---\n");

if (net4 > 0)
    printf("KYK borcu odenebiliyor.\n");
else
    printf("KYK borcu tam odenemiyor.\n");

if (arabaVar)
    printf("Arabanin %%%.2f'si alinabiliyor.\n", arabaOran4);

if (evVar)
    printf("Evin %%%.2f'si alinabiliyor.\n", evOran4);

if (net4 > 0)
    printf("4. yilda altini bozmak MANTIKLI bir secenek.\n");
else
    printf("4. yilda bozmak RISKLI.\n");

printf("\n\n\n=========== 6. YIL (%d) ===========\n\n\n", yil6);
printf("KYK Borcu: %.2f TL\n", toplamKredi);
printf("Asgari Ucret: %.2f TL\n", asgari6);

printf("\n[KOTU SENARYO]\nAltin Degeri: %.2f TL\nNet Kar: %.2f TL\n",
       degerKotu6, degerKotu6 - toplamKredi);

printf("\n[IYI SENARYO]\nAltin Degeri: %.2f TL\nNet Kar: %.2f TL\n\n\n",
       degerIyi6, degerIyi6 - toplamKredi);

if (arabaVar) printf("Araba Fiyati: %.2f TL\n", araba6);
if (evVar)    printf("Ev Fiyati: %.2f TL\n", ev6);

printf("\n--- 6. YIL MANTIK DEGERLENDIRMESI ---\n");

if (net6 > 0)
    printf("KYK borcu odenebiliyor.\n");
else
    printf("KYK borcu tam odenemiyor.\n");

if (arabaVar)
    printf("Arabanin %%%.2f'si alinabiliyor.\n", arabaOran6);

if (evVar)
    printf("Evin %%%.2f'si alinabiliyor.\n", evOran6);

if (net6 > net4)
    printf("6. yilda beklemek daha fazla alim gucu sagliyor.\n");
else
    printf("Beklemenin ek faydasi sinirli.\n");

	printf("\n\n========== GENEL KARAR ==========\n\n");

if (net6 > net4) {
    printf("6. YILDA bozmak DAHA MANTIKLI.\n");
    printf("Sebep: Net kar ve alim gucu daha yuksek.\n");
}
else if (net4 > net6) {
    printf("4. YILDA bozmak DAHA MANTIKLI.\n");
    printf("Sebep: Erken bozmak daha avantajli.\n");
}
else {
    printf("4. ve 6. yil arasinda anlamli fark yok.\n");
}

if (arabaVar && arabaOran6 > arabaOran4)
    printf("Araba hedefi icin 6. yil daha uygun.\n");

if (evVar && evOran6 > evOran4)
    printf("Ev hedefi icin 6. yil daha uygun.\n");
    
system("pause");
return 0;
}

