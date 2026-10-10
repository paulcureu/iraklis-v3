# Iraklis — site portofoliu (varianta C, bizantin contemporan)

Site static, făcut doar cu HTML, CSS și JavaScript. Nu are nevoie de build, de Node sau de vreun framework. Merge direct pe **GitHub Pages**, **Vercel** sau **Netlify** și e responsive pentru telefon, tabletă și desktop.

Stilul: fundal de pergament, roșu bizantin, auriu, poze în rame de arc (ca niște nișe de iconostas), triptice și ornamente de mozaic.

## Ce conține

```
index.html          Acasă: triptic cu 3 lucrări, citat, lucrări alese, banda roșie cu Bunavestirea, practica, despre, comandă
works.html          Toate cele 14 serii în rame de arc, cu filtru după loc
series.html         Pagina unei serii: series.html?s=iconostasis-moscow
contact.html        Contact + formular
404.html            Pagina „nu există” (GitHub Pages o folosește automat)
assets/
  css/style.css     Tot designul (culori, fonturi și spații sunt în :root, sus)
  js/data.js        LISTA DE LUCRĂRI. Aici editezi seriile.
  js/main.js        Header, meniul de pe telefon, galeria, viewer-ul de poze și formularul
  fonts/            Fonturile (Cinzel, EB Garamond), găzduite local
  img/works/<serie>/01.jpg, 01-sm.jpg …   Pozele, câte 2 mărimi (1600px și 800px)
  img/site/         Portretul, timpanul „Bunavestire”, imaginea de share (og.jpg)
  img/favicon.svg   Iconița din tab-ul browserului
```

## Cum îl vezi pe calculator

Cel mai simplu e cu extensia **Live Server** din VS Code: click dreapta pe `index.html` și alegi „Open with Live Server”.

Sau din terminal, în folderul proiectului:

```bash
npx serve .
# sau
python3 -m http.server 8000
```

Dacă deschizi `index.html` direct (dublu-click), merge și așa, dar unele browsere blochează anumite lucruri pe `file://`, așa că e mai sigur cu un server local.

## Cum îl pui pe GitHub Pages

1. Creează un repository nou pe GitHub, de exemplu `iraklis`.
2. Urcă fișierele, **cu tot cu folderul `assets`** și fișierul `.nojekyll`. Ai două variante:
   - din browser: „Add file” → „Upload files” și tragi tot conținutul folderului;
   - din terminal:
     ```bash
     git init
     git add .
     git commit -m "Iraklis portfolio"
     git branch -M main
     git remote add origin https://github.com/UTILIZATORUL-TAU/iraklis.git
     git push -u origin main
     ```
3. În repository mergi la **Settings → Pages**. La „Source” alegi **Deploy from a branch**, apoi branch-ul `main` și folderul `/ (root)`, și salvezi.
4. După 1–2 minute site-ul e live la `https://UTILIZATORUL-TAU.github.io/iraklis/`.

Toate link-urile sunt relative, deci site-ul merge și într-un subfolder, cum e cel de mai sus.

**Domeniu propriu** (de ex. `iraklis.art`): în Settings → Pages → „Custom domain” scrii domeniul, iar la firma de unde ai cumpărat domeniul pui înregistrările DNS indicate de GitHub.

**Vercel:** „Add New → Project”, alegi repository-ul, la Framework Preset alegi **Other**, lași Build Command gol și dai Deploy.

## Cum modifici lucrările

Totul e în `assets/js/data.js`. Fiecare serie arată așa:

```js
{
  slug: "iconostasis-moscow",      // numele folderului cu poze + adresa paginii
  title: "Iconostasis",
  place: "Moscow, Russia",
  years: "",                        // ex. "2018" sau "2008–2009"
  region: "russia",                 // filtrul din galerie: athos, russia, georgia, ny, studio
  category: "Sacred art",
  materials: "",                    // ex. "Limestone, red marble"
  description: "",                  // 2–3 propoziții despre lucrare
  images: [
    { f: '01', w: 1080, h: 1418, sw: 609 },   // 01 = coperta seriei
    ...
  ]
}
```

- **An, materiale, descriere:** completează câmpurile `years`, `materials` și `description`. Ce rămâne gol nu apare pe site, deci nu se văd goluri sau texte de umplutură. Anul și materialele apar în „fișa” de sub titlul seriei, iar descrierea apare cu italic sub ea. Merită cerute de la Iraklis măcar pentru seriile importante.
- **Tripticul din pagina seriei:** primele 3 poze formează tripticul de sus. Poza `01` stă în mijloc (arcul mare), `02` în stânga și `03` în dreapta. Pe telefon, arcul din mijloc trece pe toată lățimea, iar celelalte două stau dedesubt. Pozele de la `04` încolo apar în coloanele de mai jos („Plates”).
- **Ordinea seriilor:** e ordinea din fișier. Numerotarea Nº 01, Nº 02 … se face automat. Pagina Works se actualizează singură din acest fișier.

### Ce lucrări apar pe prima pagină

Secțiunea „An iconostasis of works” de pe prima pagină arată 8 serii alese de mână. Le schimbi în `index.html`, în atributul `data-picks`:

```html
<div class="arch-grid" id="home-works" data-picks="capitals-four-evangelists,mount-athos-2023-2025,iconostasis-moscow,…"></div>
```

Scrii `slug`-urile din `data.js`, separate prin virgulă, în ordinea dorită. Tot în `index.html` sunt scrise direct cele 3 poze din tripticul de sus și textele secțiunilor.

### Cum adaugi o serie nouă

1. Creează folderul `assets/img/works/numele-seriei/` și pune pozele cu numele `01.jpg`, `02.jpg` și așa mai departe.
   Redimensionează-le înainte la maximum 1600px pe latura lungă, de exemplu cu [squoosh.app](https://squoosh.app).
2. Adaugă în `data.js` un obiect nou, cu `slug: "numele-seriei"` și pozele scrise simplu: `{ f: '01' }, { f: '02' }`.
   `w`, `h`, `sw` și fișierele `-sm.jpg` sunt opționale. Ajută doar la viteză pe telefon.
3. Coperta seriei (în rama de arc) e poza `01`. Ramele taie poza în format 3:4, deci pentru copertă alege o poză cu subiectul în centru.

## Formularul de contact

Când vizitatorul apasă „Send enquiry”, i se deschide aplicația de email cu mesajul deja completat către `narikaleli1@gmail.com`. Nu e nevoie de server sau de vreun serviciu extern.
Ca să schimbi adresa, modifici `CONTACT_EMAIL` în `assets/js/main.js` și link-urile `mailto:` din fișierele HTML.

Dacă mai târziu vrei ca mesajele să vină direct, fără să se deschidă aplicația de email, poți folosi Formspree sau Web3Forms. Pui `action` pe formular și scoți handler-ul din `initContact()`.

## După ce ai domeniul

În fiecare fișier HTML înlocuiește `content="assets/img/site/og.jpg"` cu adresa completă, de exemplu `https://iraklis.art/assets/img/site/og.jpg`. Așa apare poza când cineva trimite link-ul pe WhatsApp sau Facebook.

## Note

- Textele sunt în engleză și sunt adaptate după site-urile vechi. Merită verificate cu Iraklis.
- Pozele primesc din CSS o ușoară tentă caldă (clasa `.warm`), ca să se potrivească cu fundalul de pergament. Dacă vrei pozele exact ca în original, șterge regula `.warm` din `style.css`.
- Animațiile se opresc automat pentru cei care au „reduce motion” activat în sistem.
- Fonturile sunt sub licența SIL Open Font License (vezi `assets/fonts/LICENSE-*.txt`).
