<!DOCTYPE html>
<html lang="sr">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Kviz: Priroda i društvo – 3. razred (Eduka)</title>
<style>
  :root{
    --bg1:#b3e5fc;
    --bg2:#e1f5fe;
    --card:#ffffff;
    --accent:#0288d1;
    --btn:#4fc3f7;
    --next:#ffb74d;
    --muted:#6b7280;
  }
  body{
    margin:0;
    font-family: "Comic Sans MS", "Baloo 2", system-ui, sans-serif;
    background: linear-gradient(180deg,var(--bg1),var(--bg2));
    color:#222;
    -webkit-font-smoothing:antialiased;
    -moz-osx-font-smoothing:grayscale;
  }
  .wrap{ max-width:980px; margin:22px auto; padding:16px; }
  .card{
    background:var(--card);
    border-radius:16px;
    padding:16px;
    box-shadow:0 8px 30px rgba(0,0,0,0.12);
  }
  header{ text-align:center; margin-bottom:12px; }
  h1{ margin:6px 0; color:var(--accent); font-size:26px; }
  p.lead{ margin:0; color:#555; }

  /* Home area */
  #home-grid{ display:grid; grid-template-columns:repeat(2,1fr); gap:10px; margin-top:12px; }
  @media (max-width:700px){ #home-grid{ grid-template-columns:1fr; } }
  .area-btn{
    background:#e6f7ff;
    border:2px solid rgba(2,136,209,0.08);
    color:var(--accent);
    padding:12px;
    border-radius:12px;
    cursor:pointer;
    font-weight:700;
    text-align:left;
    box-shadow:0 6px 12px rgba(3,169,244,0.08);
  }
  .area-btn:hover{ transform:translateY(-2px); transition:transform .12s ease; }

  /* Quiz area */
  .oblast-title{ font-weight:700; font-size:18px; color:#055e7b; margin-bottom:6px; }
  .question{ font-size:20px; margin:10px 0; }
  .options{ display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-top:10px; }
  @media (max-width:600px){ .options{ grid-template-columns:1fr; } .question{ font-size:18px; } }
  button.opt{
    padding:12px; font-size:16px; border-radius:12px; border:none; cursor:pointer;
    background:var(--btn); color:white; transition:transform .12s ease, box-shadow .12s ease;
    box-shadow:0 6px 12px rgba(79,195,247,0.18);
  }
  button.opt:active{ transform:translateY(1px) }
  button.opt[disabled]{ opacity:0.8; cursor:default; }
  .correct{ background:#81c784 !important; box-shadow:0 6px 12px rgba(129,199,132,0.18) !important;}
  .wrong{ background:#e57373 !important; box-shadow:0 6px 12px rgba(229,115,115,0.18) !important;}

  #controls{ display:flex; gap:12px; justify-content:center; margin-top:18px; align-items:center; flex-wrap:wrap; }
  #next-btn{ background:var(--next); border:none; padding:10px 18px; border-radius:12px; cursor:pointer; font-weight:700; }
  #back-home{ background:#fff; border:2px solid #cfeefb; padding:10px 14px; border-radius:12px; cursor:pointer; color:var(--accent); font-weight:700; }

  .meta-row{ display:flex; gap:10px; justify-content:center; margin:12px 0; flex-wrap:wrap; }
  .pill{ background:#f0f8ff; border:1px solid rgba(2,136,209,0.06); padding:6px 10px; border-radius:12px; font-weight:600; color:var(--accent); }

  #explanation{ margin-top:12px; font-size:16px; color:#222; min-height:46px; }
  footer{ text-align:center; margin-top:18px; color:var(--muted); font-size:13px; }

</style>
</head>
<body>
  <div class="wrap">
    <div class="card">
      <header>
        <h1>🌍 Kviz: Priroda i društvo — 3. razred (Eduka)</h1>
        <p class="lead">Izaberi oblast i uči kroz zabavu • 36 oblasti • 5 pitanja po oblasti • Offline</p>
      </header>

      <!-- POČETNA STRANICA -->
      <div id="home" class="card" style="padding:14px;">
        <div style="display:flex; justify-content:space-between; align-items:center; gap:10px;">
          <div style="font-weight:700; color:#055e7b;">Izaberi oblast:</div>
          <div style="font-size:13px; color:var(--muted);">Klikni na oblast da počneš kviz za tu oblast</div>
        </div>

        <div id="home-grid"></div>
      </div>

      <!-- KVIZ STRANICA -->
      <div id="quiz" class="card" style="padding:18px; display:none;">
        <div class="meta-row">
          <div class="pill" id="oblast-pill">Oblast: 1 / 36</div>
          <div class="pill" id="pitanje-pill">Pitanje: 1 / 5</div>
          <div class="pill" id="tacno-pill">Tačno: 0</div>
        </div>

        <div class="oblast-title" id="oblast-title"></div>
        <div class="question" id="question-text"></div>
        <div class="options" id="options-container"></div>
        <div id="explanation"></div>

        <div id="controls">
          <button id="back-home">🏠 Vrati se na početnu</button>
          <button id="next-btn" style="display:none;">Sledeće pitanje ➡️</button>
          <div id="score" style="font-weight:700; color:var(--accent);"></div>
        </div>
      </div>

      <footer>
        <small>Napomena: pitanja i objašnjenja su prilagođeni Eduka udžbeniku (jezik i sažetak) kako bi bili jednostavni i razumljivi za učenike.</small>
      </footer>
    </div>
  </div>

<script>
/* PODACI: 36 oblasti x 5 pitanja (iste kao u ranijoj verziji) */
const quizData = [
  { oblast: "Reljef mog kraja (Strana 4-5)", pitanja:[
      { q:"Šta je reljef?", options:["Životinje u prirodi","Nepravilnosti Zemljine površine","Vode na površini","Biljke u mom kraju"], answer:"Nepravilnosti Zemljine površine", explanation:"Reljef su nepravilnosti Zemljine površine (str. 4)." },
      { q:"Koja je najniža vrsta uzvišenja?", options:["Planine preko 500 m","Bregovska do 200 m","Brda od 200 do 500 m","Ravnice na 0 m"], answer:"Bregovska do 200 m", explanation:"Bregovska su najniža, do 200 m (str. 4)." },
      { q:"Šta je ravnica?", options:["Visoka uzvišenja","Niska površina sa poljima","Duboka dolina","Planinski vrh"], answer:"Niska površina sa poljima", explanation:"Ravnica je niska površina sa poljima (str. 5)." },
      { q:"Kako se zove visoka ravna površina na planini?", options:["Klisura","Visoravan","Kotlina","Brdo"], answer:"Visoravan", explanation:"Visoravan je visoka ravna površina (str. 5)." },
      { q:"Koja je razlika između brda i planina?", options:["Brda su viša","Planine su niže od brda","Brda su do 500 m, planine preko 500 m","Nema razlike"], answer:"Brda su do 500 m, planine preko 500 m", explanation:"Brda su do 500 m, planine preko 500 m (str. 4)." }
    ]
  },
  { oblast: "Uzvišenja i udubljenja (Strana 5-6)", pitanja:[
      { q:"Šta je brdo?", options:["Visina od 200 do 500 m","Ravnica","Planina preko 1000 m","Duboka dolina"], answer:"Visina od 200 do 500 m", explanation:"Brdo je visina od 200 do 500 m (str. 5)." },
      { q:"Kako se zove duboka uska dolina?", options:["Ravnica","Klisura","Brdo","Visoravan"], answer:"Klisura", explanation:"Klisura je duboka uska dolina (str. 5)." },
      { q:"Šta je kotlina?", options:["Visoko uzvišenje","Niže udubljenje okruženo brdima","Rečna obala","Planinski vrh"], answer:"Niže udubljenje okruženo brdima", explanation:"Kotlina je niže udubljenje okruženo brdima (str. 5)." },
      { q:"Gde se nalazi visoravan?", options:["U ravnici","Na planini","U moru","U dolini"], answer:"Na planini", explanation:"Visoravan se nalazi na planini (str. 5)." },
      { q:"Koja je najviša vrsta reljefa?", options:["Breg","Planina","Brdo","Ravnica"], answer:"Planina", explanation:"Planina je najviša vrsta reljefa (str. 4)." }
    ]
  },
  { oblast: "Površinske vode (Strana 6-7)", pitanja:[
      { q:"Šta su površinske vode?", options:["Vode ispod zemlje","Vode na površini Zemlje","Oblaci","Sneg"], answer:"Vode na površini Zemlje", explanation:"Površinske vode su vode na površini (str. 6)." },
      { q:"Gde počinje reka?", options:["Na ušću","Na izvoru","U moru","U jezeru"], answer:"Na izvoru", explanation:"Reka počinje na izvoru (str. 6)." },
      { q:"Šta je tok reke?", options:["Mesto ulivanja","Put od izvora do ušća","Obala reke","Dubina vode"], answer:"Put od izvora do ušća", explanation:"Tok je put od izvora do ušća (str. 6)." },
      { q:"Kako se zove mesto gde se reka uliva?", options:["Izvor","Ušće","Bara","Potok"], answer:"Ušće", explanation:"Ušće je mesto ulivanja reke (str. 6)." },
      { q:"Šta je jezero?", options:["Mali tok vode","Veća stajaća voda","Brza reka","Duboka klisura"], answer:"Veća stajaća voda", explanation:"Jezero je veća stajaća voda (str. 7)." }
    ]
  },
  { oblast: "Bara i potok (Strana 7-8)", pitanja:[
      { q:"Šta je bara?", options:["Veliko jezero","Mala plitka voda","Brza reka","Duboki kanal"], answer:"Mala plitka voda", explanation:"Bara je mala plitka voda (str. 7)." },
      { q:"Kako se zove mali tok vode?", options:["Potok","Jezero","Ušće","Klisura"], answer:"Potok", explanation:"Potok je mali tok vode (str. 7)." },
      { q:"Gde se obično nalazi bara?", options:["Na planini","U ravnici","U moru","U klisuri"], answer:"U ravnici", explanation:"Bara se obično nalazi u ravnici (str. 7)." },
      { q:"Koja je razlika između bare i jezera?", options:["Bara je veća","Jezero je plitko","Bara je manja i plitka","Nema razlike"], answer:"Bara je manja i plitka", explanation:"Bara je manja i plitka (str. 7)." },
      { q:"Šta se uliva u potok?", options:["Drugi potok","Jezero","More","Planina"], answer:"Drugi potok", explanation:"Potok prima druge potoke (str. 7)." }
    ]
  },
  { oblast: "Život u ravnici (Strana 8-9)", pitanja:[
      { q:"Šta ljudi rade u ravnici?", options:["Stočarstvo","Poljoprivreda","Rudarstvo","Ribolov"], answer:"Poljoprivreda", explanation:"U ravnici ljudi obično rade poljoprivredu (str. 8)." },
      { q:"Koji putevi su u ravnici?", options:["Vijugavi","Ravni i široki","Strmi","Uski"], answer:"Ravni i široki", explanation:"U ravnici putevi su ravni i široki (str. 8)." },
      { q:"Šta je karakteristično za ravnicu?", options:["Planine","Polja i njive","Klisure","Sneg"], answer:"Polja i njive", explanation:"Karakteristična su polja i njive (str. 8)." },
      { q:"Koje kulture se gaje u ravnici?", options:["Vinogradi","Usevi i voće","Šume","Trave"], answer:"Usevi i voće", explanation:"U ravnici se gaje usevi i voće (str. 8)." },
      { q:"Gde su plastenici u ravnici?", options:["Na brdima","Za povrće","U moru","Na planinama"], answer:"Za povrće", explanation:"Plastenici služe za gajenje povrća (str. 8)." }
    ]
  },
  { oblast: "Život u brdovitom kraju (Strana 9)", pitanja:[
      { q:"Šta ljudi rade u brdovitom kraju?", options:["Poljoprivreda","Voćarstvo","Rudarstvo","Ribolov"], answer:"Voćarstvo", explanation:"U brdovitom kraju često se bave voćarstvom (str. 9)." },
      { q:"Koji putevi su u brdovitom kraju?", options:["Ravni","Vijugavi","Široki","Bez puteva"], answer:"Vijugavi", explanation:"Putevi su vijugavi (str. 9)." },
      { q:"Koje kulture se gaje u brdovitom kraju?", options:["Usevi","Vinogradi","Njive","Šume"], answer:"Vinogradi", explanation:"Često se gaje vinogradi (str. 9)." },
      { q:"Šta je stočarstvo?", options:["Gajenje biljaka","Gajenje stoke","Ribolov","Gradnja"], answer:"Gajenje stoke", explanation:"Stočarstvo je gajenje stoke (str. 9)." },
      { q:"Gde se često nalazi voćnjak?", options:["U ravnici","U brdovitom kraju","U moru","Na planini"], answer:"U brdovitom kraju", explanation:"Voćnjaci često rastu u brdovitom kraju (str. 9)." }
    ]
  },
  { oblast: "Život u planinskom kraju (Strana 9-10)", pitanja:[
      { q:"Šta ljudi rade u planinama?", options:["Poljoprivreda","Stočarstvo","Ribolov","Gradnja"], answer:"Stočarstvo", explanation:"U planinskom kraju se često bavi stočarstvom (str. 9)." },
      { q:"Koji putevi su u planinama?", options:["Ravni","Uski i vijugavi","Široki","Bez puteva"], answer:"Uski i vijugavi", explanation:"Putevi u planinama su uski i vijugavi (str. 9)." },
      { q:"Šta je turizam u planinama?", options:["Gajenje useva","Poseta prirodi","Ribolov","Poljoprivreda"], answer:"Poseta prirodi", explanation:"Turizam je poseta prirodi (str. 9)." },
      { q:"Koja životinja se gaji u planinama?", options:["Krave","Ribe","Ptice","Zečevi"], answer:"Krave", explanation:"U planinama se često gaje krave (str. 9)." },
      { q:"Gde se nalazi rudarstvo?", options:["U ravnici","U planinama","U jezeru","U moru"], answer:"U planinama", explanation:"Rudarstvo se često nalazi u planinama (str. 10)." }
    ]
  },
  { oblast: "Orijentacija pomoću Sunca (Strana 12)", pitanja:[
      { q:"Gde izlazi Sunce?", options:["Na zapadu","Na istoku","Na severu","Na jugu"], answer:"Na istoku", explanation:"Sunce izlazi na istoku (str. 12)." },
      { q:"Gde zalazi Sunce?", options:["Na istoku","Na zapadu","Na severu","Na jugu"], answer:"Na zapadu", explanation:"Sunce zalazi na zapadu (str. 12)." },
      { q:"Gde je sever u podne?", options:["Ispred tebe","Iza tebe","Levo","Desno"], answer:"Iza tebe", explanation:"U podne je sever iza tebe (str. 12)." },
      { q:"Šta pomaže Sunce u orijentaciji?", options:["Vreme","Strane sveta","Visinu","Dubinu"], answer:"Strane sveta", explanation:"Sunce pomaže da odredimo strane sveta (str. 12)." },
      { q:"Kada je Sunce na vrhuncu?", options:["Ujutru","U podne","Uveče","Noću"], answer:"U podne", explanation:"Sunce je na vrhuncu u podne (str. 12)." }
    ]
  },
  { oblast: "Kompas i strane sveta (Strana 13)", pitanja:[
      { q:"Šta pokazuje kompas?", options:["Vreme","Sever","Visinu","Daljinu"], answer:"Sever", explanation:"Kompas pokazuje sever (str. 13)." },
      { q:"Koje su glavne strane sveta?", options:["Sever, jug, istok, zapad","Gore, dole","Levo, desno","Napred, nazad"], answer:"Sever, jug, istok, zapad", explanation:"Glavne strane sveta su sever, jug, istok i zapad (str. 13)." },
      { q:"Gde je zapad ako je istok desno?", options:["Levo","Ispred","Iza","Gore"], answer:"Levo", explanation:"Ako je istok desno, zapad je levo (str. 13)." },
      { q:"Kako se zove igla na kompasu?", options:["Strelica","Ručica","Točak","Dugme"], answer:"Strelica", explanation:"Igla na kompasu se zove strelica (str. 13)." },
      { q:"Šta je jug?", options:["Strana gde Sunce izlazi","Strana gde je Sunce u podne","Strana gde zalazi","Strana gde je noć"], answer:"Strana gde je Sunce u podne", explanation:"Jug je strana gde je Sunce u podne (str. 13)." }
    ]
  },
  { oblast: "Vreme i godišnja doba (Strana 14-15)", pitanja:[
      { q:"Koliko sati ima dan?", options:["12","24","48","10"], answer:"24", explanation:"Dan ima 24 sata (str. 14)." },
      { q:"Šta je godina?", options:["12 meseci","365 dana","30 dana","7 dana"], answer:"365 dana", explanation:"Godina ima 365 dana (str. 14)." },
      { q:"Koje je prvo godišnje doba?", options:["Proleće","Leto","Jesen","Zima"], answer:"Proleće", explanation:"Prvo godišnje doba je proleće (str. 15)." },
      { q:"Kada pada sneg?", options:["Leto","Zima","Proleće","Jesen"], answer:"Zima", explanation:"Sneg obično pada zimi (str. 15)." },
      { q:"Koliko meseci ima godina?", options:["10","12","8","6"], answer:"12", explanation:"Godina ima 12 meseci (str. 14)." }
    ]
  },
  { oblast: "Plan grada (Strana 16)", pitanja:[
      { q:"Šta je plan grada?", options:["Slika prirode","Prikaz grada odozgo","Karta mora","Plan planine"], answer:"Prikaz grada odozgo", explanation:"Plan grada je prikaz grada odozgo (str. 16)." },
      { q:"Koje zgrade su na planu?", options:["Kuće i ulice","Planine","Jezera","Šume"], answer:"Kuće i ulice", explanation:"Na planu se vide kuće i ulice (str. 16)." },
      { q:"Kako se zova crtanje plana?", options:["Slikanje","Mapiranje","Gradnja","Crtanje"], answer:"Mapiranje", explanation:"Crtanje plana zovemo mapiranje (str. 16)." },
      { q:"Gde se vidi put na planu?", options:["Linijama","Bojom","Oblacima","Drvećem"], answer:"Linijama", explanation:"Put se na planu prikazuje linijama (str. 16)." },
      { q:"Šta pomaže plan grada?", options:["Pronalaženje puta","Plivanje","Letenje","Spavanje"], answer:"Pronalaženje puta", explanation:"Plan pomaže pri pronalaženju puta (str. 16)." }
    ]
  },
  { oblast: "Biljke i šume (Strana 54-55)", pitanja:[
      { q:"Šta je šuma?", options:["Polje sa travom","Površina sa drvećem","Jezero","Planina"], answer:"Površina sa drvećem", explanation:"Šuma je površina na kojoj raste mnogo drveća (str. 54)." },
      { q:"Koji deo biljke je за disanje?", options:["Koreni","Listovi","Cvetovi","Stabljika"], answer:"Listovi", explanation:"Listovi služe za disanje biljaka (str. 55)." },
      { q:"Šta raste u šumi?", options:["Njive","Drveće","Pustinje","Bare"], answer:"Drveće", explanation:"U šumi raste drveće (str. 54)." },
      { q:"Koja biljka ima koren?", options:["Drvo","Oblak","Rečica","Ptica"], answer:"Drvo", explanation:"Drvo ima koren (str. 55)." },
      { q:"Gde se nalazi šuma?", options:["U ravnici","U planinama","U moru","U jezeru"], answer:"U planinama", explanation:"Mnoge šume nalaze se i u planinskim područjima (str. 54)." }
    ]
  },
  { oblast: "Životinje u prirodi (Strana 61-62)", pitanja:[
      { q:"Koja životinja živi u vodi?", options:["Lav","Riba","Ptica","Žirafa"], answer:"Riba", explanation:"Ribe žive u vodi (str. 61)." },
      { q:"Šta je ptica?", options:["Životinja sa krilima","Biljka","Riba","Drvo"], answer:"Životinja sa krilima", explanation:"Ptica je životinja koja ima krila (str. 61)." },
      { q:"Gde živi medved?", options:["U šumi","U jezeru","U moru","Na polju"], answer:"U šumi", explanation:"Medved živi u šumi (str. 62)." },
      { q:"Koja životinja skače?", options:["Zec","Riba","Ptica","Drvo"], answer:"Zec", explanation:"Zec skače (str. 62)." },
      { q:"Šta jede lisica?", options:["Travu","Meso","Cveće","Vodu"], answer:"Meso", explanation:"Lisica je mesožder i jede meso (str. 62)." }
    ]
  },
  { oblast: "Parkovi i livade (Strana 68-69)", pitanja:[
      { q:"Šta je park?", options:["Zeleni prostor za odmor","Fabrika","Rečica","Planina"], answer:"Zeleni prostor za odmor", explanation:"Park je zeleni prostor namenjen odmoru i igranju (str. 68)." },
      { q:"Šta raste na livadi?", options:["Drveće","Trava i cveće","Njive","Šume"], answer:"Trava i cveće", explanation:"Na livadi raste trava i cveće (str. 68)." },
      { q:"Gde se ljudi šetaju?", options:["U parku","U moru","U klisuri","U jezeru"], answer:"U parku", explanation:"Ljudi se često šetaju u parku (str. 68)." },
      { q:"Koja biljka je na livadi?", options:["Maslačak","Vinova loza","Drvo","Usev"], answer:"Maslačak", explanation:"Maslačak je česta biljka na livadama (str. 69)." },
      { q:"Šta je cilj parka?", options:["Odmor i igra","Rad","Plivanje","Spavanje"], answer:"Odmor i igra", explanation:"Cilj parka je odmor i igra (str. 68)." }
    ]
  },
  { oblast: "Zaštita prirode (Strana 74-75)", pitanja:[
      { q:"Šta je zaštita prirode?", options:["Čuvanje biljaka","Gradnja fabrika","Lov","Sakupljanje otpada"], answer:"Čuvanje biljaka", explanation:"Zaštita prirode znači čuvati biljke i životinje (str. 74)." },
      { q:"Zašto čuvamo šume?", options:["Za vazduh","Za vodu","Za igru","Za hranu"], answer:"Za vazduh", explanation:"Šume su važne zato što pomažu da imamo čist vazduh (str. 74)." },
      { q:"Koji je problem za prirodu?", options:["Zagađenje","Čišćenje","Sadnja","Šetnja"], answer:"Zagađenje", explanation:"Zagađenje je veliki problem za prirodu (str. 75)." },
      { q:"Šta treba bacati u kantu?", options:["Otpad","Hranu","Vodu","Drveće"], answer:"Otpad", explanation:"Otpad treba bacati u kantu za smeće (str. 75)." },
      { q:"Kako pomažemo prirodi?", options:["Sadnjom drveća","Lovom","Zagađenjem","Gradnjom"], answer:"Sadnjom drveća", explanation:"Sadnjom drveća pomažemo prirodi (str. 74)." }
    ]
  },
  { oblast: "Površina i obim (Strana 22-23)", pitanja:[
      { q:"Šta je površina?", options:["Visina","Prostor koji zauzima","Dubina","Dužina"], answer:"Prostor koji zauzima", explanation:"Površina je prostor koji predmet zauzima (str. 22)." },
      { q:"Kako se meri površina?", options:["Metrima","Kvadratnim metrima","Kilogramima","Litrom"], answer:"Kvadratnim metrima", explanation:"Površina se meri u kvadratnim metrima (m²) (str. 22)." },
      { q:"Šta je obim?", options:["Dužina oko predmeta","Visina","Širina","Dubina"], answer:"Dužina oko predmeta", explanation:"Obim je dužina koja ide oko predmeta (str. 23)." },
      { q:"Kako se meri obim?", options:["Kvadratnim metrima","Metrima","Litrom","Kilogramima"], answer:"Metrima", explanation:"Obim se meri metrima (str. 23)." },
      { q:"Koja je jedinica za površinu?", options:["m²","m","kg","l"], answer:"m²", explanation:"Jedinica za površinu je kvadratni metar (m²) (str. 22)." }
    ]
  },
  { oblast: "Mape i karte (Strana 22)", pitanja:[
      { q:"Šta je geografska karta?", options:["Prikaz grada","Prikaz zemlje","Slika kuće","Plan reke"], answer:"Prikaz zemlje", explanation:"Geografska karta prikazuje deo zemlje ili celu državu (str. 22)." },
      { q:"Koje boje su na karti?", options:["Zeleno, plavo","Crno, crveno","Žuto, narandžasto","Roze, ljubičasto"], answer:"Zeleno, plavo", explanation:"Na karti često koristimo zelenu i plavu boju (str. 22)." },
      { q:"Šta označava plava boja?", options:["Planine","Vode","Šume","Putevi"], answer:"Vode", explanation:"Plava boja obično označava vode (reke, jezera, more) (str. 22)." },
      { q:"Kako se zove karta grada?", options:["Plan","Geografska karta","Slika","Karta mora"], answer:"Plan", explanation:"Karta grada se zove plan (str. 22)." },
      { q:"Šta pomaže karta?", options:["Pronalaženje mesta","Igra","Spavanje","Jelo"], answer:"Pronalaženje mesta", explanation:"Karta pomaže pri pronalaženju mesta (str. 22)." }
    ]
  },
  { oblast: "Vreme i klima (Strana 50-51)", pitanja:[
      { q:"Šta je vreme?", options:["Dan i noć","Planina","Rečica","Drvo"], answer:"Dan i noć", explanation:"Vreme je pojava dan i noć, kao i vremenski uslovi (str. 50)." },
      { q:"Koja je sezona kiše?", options:["Leto","Jesen","Zima","Proleće"], answer:"Jesen", explanation:"Jesen je često povezana sa kišom (str. 51)." },
      { q:"Šta donosi vetar?", options:["Hladnoću","Toplinu","Sneg","Cveće"], answer:"Hladnoću", explanation:"Vetar može doneti hladnoću (str. 50)." },
      { q:"Kada je najtoplije?", options:["Zima","Leto","Jesen","Proleće"], answer:"Leto", explanation:"Najtoplije je tokom leta (str. 51)." },
      { q:"Šta meri termometar?", options:["Vreme","Temperaturu","Visinu","Daljinu"], answer:"Temperaturu", explanation:"Termometar meri temperaturu (str. 50)." }
    ]
  },
  { oblast: "Promene u prirodi (Strana 52-53)", pitanja:[
      { q:"Šta menja sezone?", options:["Sunce","Mesec","Zvezde","Oblaci"], answer:"Sunce", explanation:"Promenu godišnjih doba uzrokuje položaj Zemlje i Sunca (str. 52)." },
      { q:"Kada cveta cveće?", options:["Zima","Proleće","Jesen","Leto"], answer:"Proleće", explanation:"Cveće obično cveta u proleće (str. 53)." },
      { q:"Šta pada zimi?", options:["Kiša","Sneg","Grane","Listovi"], answer:"Sneg", explanation:"Zimi često pada sneg (str. 53)." },
      { q:"Kada žute listovi?", options:["Proleće","Leto","Jesen","Zima"], answer:"Jesen", explanation:"Listovi obično žute u jesen (str. 53)." },
      { q:"Koja sezona donosi voće?", options:["Zima","Leto","Jesen","Proleće"], answer:"Leto", explanation:"Mnoge vrste voća sazrevaju leti (str. 53)." }
    ]
  },
  { oblast: "Biljke i voda (Strana 56-57)", pitanja:[
      { q:"Šta biljkama treba za rast?", options:["Voda","Kamen","Pesak","Vazduh"], answer:"Voda", explanation:"Biljkama je potrebna voda za rast (str. 56)." },
      { q:"Koji deo biljke upija vodu?", options:["Listovi","Koreni","Cvetovi","Stabljika"], answer:"Koreni", explanation:"Koreni upijaju vodu iz zemlje (str. 56)." },
      { q:"Šta je stabljika?", options:["Deo za vodu","Deo za podršku","Deo za cveće","Deo za listove"], answer:"Deo za podršku", explanation:"Stabljika drži biljku i prenosi vodu (str. 56)." },
      { q:"Gde rastu biljke?", options:["U moru","U zemlji","U vazduhu","U vodi"], answer:"U zemlji", explanation:"Većina biljaka raste u zemlji (str. 57)." },
      { q:"Koja biljka ima cvetove?", options:["Ruža","Drvo","Trava","Korov"], answer:"Ruža", explanation:"Ruža je biljka koja ima cveće (str. 57)." }
    ]
  },
  { oblast: "Životinjski svet (Strana 63-64)", pitanja:[
      { q:"Koja životinja pliva?", options:["Pas","Riba","Mačka","Zec"], answer:"Riba", explanation:"Ribe plivaju (str. 63)." },
      { q:"Gde živi ptica?", options:["U vodi","U gnezdu","U šumi","U pećini"], answer:"U gnezdu", explanation:"Ptice često žive ili prave gnezda (str. 63)." },
      { q:"Koja životinja jede travu?", options:["Lav","Krava","Zmija","Lisica"], answer:"Krava", explanation:"Krava je životinja koja jede travu (str. 64)." },
      { q:"Šta je gušter?", options:["Ptica","Gmaz","Sisavac","Riba"], answer:"Gmaz", explanation:"Gušter je predstavnik gmizavaca (gmaz) (str. 64)." },
      { q:"Gde živi riba?", options:["U šumi","U vodi","Na drvetu","U zemlji"], answer:"U vodi", explanation:"Ribe žive u vodi (str. 63)." }
    ]
  },
  { oblast: "Prirodne zajednice (Strana 70-71)", pitanja:[
      { q:"Šta je šuma?", options:["Zajednica drveća","Zajednica vode","Zajednica ptica","Zajednica kamenja"], answer:"Zajednica drveća", explanation:"Šuma je zajednica drveća i drugih organizama (str. 70)." },
      { q:"Gde žive životinje u šumi?", options:["U vodi","U gnezdima","U pećinama","U zemlji"], answer:"U gnezdima", explanation:"Mnoge ptice i životinje imaju gnezda ili skrovišta u šumi (str. 70)." },
      { q:"Koja je zajednica u vodi?", options:["Šuma","Jezero","Livada","Planina"], answer:"Jezero", explanation:"Jezero je zajednica u vodi (str. 71)." },
      { q:"Šta raste na livadi?", options:["Drveće","Trava","Njive","Šume"], answer:"Trava", explanation:"Na livadi najčešće raste trava (str. 71)." },
      { q:"Koja zajednica ima ptice?", options:["Jezero","Šuma","Planina","Pustinja"], answer:"Šuma", explanation:"Šume su bogate pticama (str. 70)." }
    ]
  },
  { oblast: "Promene u biljkama (Strana 72-73)", pitanja:[
      { q:"Kada biljke cvetaju?", options:["Zima","Proleće","Jesen","Leto"], answer:"Proleće", explanation:"Biljke obično cvetaju u proleće (str. 72)." },
      { q:"Šta pada sa drveća jeseni?", options:["Cveće","Listovi","Voće","Grane"], answer:"Listovi", explanation:"Listovi opadaju u jesen (str. 73)." },
      { q:"Kada biljke daju voće?", options:["Zima","Leto","Jesen","Proleće"], answer:"Leto", explanation:"Mnoge biljke daju voće leti (str. 73)." },
      { q:"Šta se menja na biljkama zimi?", options:["Listovi opadaju","Cveće cveta","Voće raste","Grane rastu"], answer:"Listovi opadaju", explanation:"Zimi listovi opadaju (str. 73)." },
      { q:"Kada biljke počinju da rastu?", options:["Zima","Proleće","Jesen","Leto"], answer:"Proleće", explanation:"Rast biljaka obično počinje u proleće (str. 72)." }
    ]
  },
  { oblast: "Životinjski život (Strana 65-66)", pitanja:[
      { q:"Kada ptice sele?", options:["Leto","Jesen","Zima","Proleće"], answer:"Jesen", explanation:"Ptice sele uglavnom u jesen (str. 65)." },
      { q:"Šta rade životinje zimi?", options:["Spavaju","Lete","Plivaju","Trče"], answer:"Spavaju", explanation:"Neke životinje spavaju ili hiberniraju zimi (str. 66)." },
      { q:"Koja životinja gradi gnezdo?", options:["Ptica","Riba","Zec","Lav"], answer:"Ptica", explanation:"Ptičje vrste prave gnezda (str. 65)." },
      { q:"Kada se životinje pare?", options:["Zima","Proleće","Jesen","Leto"], answer:"Proleće", explanation:"Mnoge životinje se pare u proleće (str. 66)." },
      { q:"Šta jede medved pre zimskog sna?", options:["Travu","Med","Vodu","Pesak"], answer:"Med", explanation:"Medvedi jedu mnogo hrane, uključujući med, pre zimskog sna (str. 66)." }
    ]
  },
  { oblast: "Priroda i čovek (Strana 76)", pitanja:[
      { q:"Šta čovek uzima iz prirode?", options:["Hranu","Kamene","Pesak","Oblake"], answer:"Hranu", explanation:"Ljudi iz prirode uzimaju hranu i druge resurse (str. 76)." },
      { q:"Kako čovek šteti prirodi?", options:["Čišćenjem","Zagađenjem","Sadnjom","Šetanjem"], answer:"Zagađenjem", explanation:"Zagađenje je način na koji čovek šteti prirodi (str. 76)." },
      { q:"Šta čovek gradi u prirodi?", options:["Kuće","Jezera","Planine","Šume"], answer:"Kuće", explanation:"Ljudi grade kuće i druge objekte u prirodi (str. 76)." },
      { q:"Kako čovek pomaže prirodi?", options:["Bacanjem otpada","Sadnjom drveća","Sečenjem šuma","Zagađenjem vode"], answer:"Sadnjom drveća", explanation:"Sadnjom drveća čovek pomaže prirodi (str. 76)." },
      { q:"Šta je poljoprivreda?", options:["Uzgoj biljaka","Lov","Ribolov","Gradnja"], answer:"Uzgoj biljaka", explanation:"Poljoprivreda je uzgoj biljaka i stoke (str. 76)." }
    ]
  },
  { oblast: "Reljef i voda (Strana 4-6)", pitanja:[
      { q:"Koja voda oblikuje reljef?", options:["Kiša","Sneg","Vetar","Sunce"], answer:"Kiša", explanation:"Voda (kiša) oblikuje reljef erozijom i tokovima (str. 4)." },
      { q:"Šta je klisura?", options:["Ravnica","Duboka dolina","Brdo","Jezero"], answer:"Duboka dolina", explanation:"Klisura je duboka i uska dolina (str. 5)." },
      { q:"Gde teče reka?", options:["U ravnici","Na planini","U klisuri","U moru"], answer:"U klisuri", explanation:"Reke često teku kroz klisure (str. 6)." },
      { q:"Koja je najniža tačka reljefa?", options:["Planina","Ravnica","Brdo","Visoravan"], answer:"Ravnica", explanation:"Ravnica je najniža vrsta reljefa (str. 4)." },
      { q:"Šta oblikuje visoravan?", options:["Voda","Vetar","Sunce","Sneg"], answer:"Voda", explanation:"Voda i erozija doprinose oblikovanju visoravni (str. 5)." }
    ]
  },
  { oblast: "Tok reke (Strana 6)", pitanja:[
      { q:"Šta je izvor?", options:["Mesto ulivanja","Početak reke","Kraj reke","Dubina"], answer:"Početak reke", explanation:"Izvor je mesto gde reka počinje (str. 6)." },
      { q:"Gde je ušće?", options:["Na početku","Na kraju","U sredini","U jezeru"], answer:"Na kraju", explanation:"Ušće je kraj toka reke gde se uliva u drugo more ili reku (str. 6)." },
      { q:"Kako teče reka?", options:["Nizvodno","Uzvodno","U krug","Stajaća"], answer:"Nizvodno", explanation:"Reka teče nizvodno od izvora prema ušću (str. 6)." },
      { q:"Šta je obala reke?", options:["Strana reke","Dubina","Tok","Izvor"], answer:"Strana reke", explanation:"Obala je strana reke (str. 6)." },
      { q:"Koja reka ima tok?", options:["Jezero","Potok","Bara","More"], answer:"Potok", explanation:"Potok je manji tok vode i ima svoj tok (str. 6)." }
    ]
  },
  { oblast: "Voda u prirodi (Strana 7)", pitanja:[
      { q:"Šta je stajaća voda?", options:["Reka","Jezero","Potok","Klisura"], answer:"Jezero", explanation:"Jezero je primer stajaće vode (str. 7)." },
      { q:"Gde se skuplja kiša?", options:["U jezeru","U moru","U barama","U planinama"], answer:"U barama", explanation:"Kiša može da se skuplja u barama (str. 7)." },
      { q:"Koja voda je slatka?", options:["More","Jezero","Okean","Bara"], answer:"Jezero", explanation:"Većina jezera ima slatku vodu (str. 7)." },
      { q:"Šta je plima?", options:["Povećanje vode","Smanjenje vode","Tok reke","Kiša"], answer:"Povećanje vode", explanation:"Plima je periodično povećanje nivoa mora (str. 7)." },
      { q:"Gde teče potok?", options:["U ravnici","U moru","U planini","U jezeru"], answer:"U ravnici", explanation:"Potok može teći i kroz ravnicu (str. 7)." }
    ]
  },
  { oblast: "Poljoprivreda (Strana 8)", pitanja:[
      { q:"Šta je poljoprivreda?", options:["Uzgoj biljaka","Lov","Ribolov","Gradnja"], answer:"Uzgoj biljaka", explanation:"Poljoprivreda je uzgoj biljaka i stoke (str. 8)." },
      { q:"Koje kulture se gaje?", options:["Usevi","Šume","Planine","Jezera"], answer:"Usevi", explanation:"U poljoprivredi često se gaje usevi (str. 8)." },
      { q:"Gde se radi poljoprivreda?", options:["U ravnici","U moru","U planinama","U klisuri"], answer:"U ravnici", explanation:"Mnoge farme i njive su u ravnicama (str. 8)." },
      { q:"Šta koriste poljoprivrednici?", options:["Traktore","Avione","Brodove","Bicikle"], answer:"Traktore", explanation:"Traktori su česta mašina na farmama (str. 8)." },
      { q:"Koji usev raste u ravnici?", options:["Pšenica","Vinova loza","Drveće","Trava"], answer:"Pšenica", explanation:"Pšenica je tipičan usev ravnica (str. 8)." }
    ]
  },
  { oblast: "Stočarstvo (Strana 9)", pitanja:[
      { q:"Šta je stočarstvo?", options:["Gajenje stoke","Ribolov","Poljoprivreda","Gradnja"], answer:"Gajenje stoke", explanation:"Stočarstvo je uzgoj stoke (str. 9)." },
      { q:"Koja životinja se gaji?", options:["Krave","Ribe","Ptice","Zmije"], answer:"Krave", explanation:"Krave su česta stoka (str. 9)." },
      { q:"Gde se radi stočarstvo?", options:["U ravnici","U planinama","U moru","U jezeru"], answer:"U planinama", explanation:"Stočarstvo se često obavlja i u planinskim predelima (str. 9)." },
      { q:"Šta daje krava?", options:["Mleko","Jaja","Perje","Vunu"], answer:"Mleko", explanation:"Krava daje mleko (str. 9)." },
      { q:"Koja životinja daje vunu?", options:["Ovca","Krava","Konj","Pas"], answer:"Ovca", explanation:"Ovca daje vunu (str. 9)." }
    ]
  },
  { oblast: "Putevi i saobraćaj (Strana 92-93)", pitanja:[
      { q:"Šta je put?", options:["Kretanje","Vozilo","Staza","Kuća"], answer:"Staza", explanation:"Put je staza kojom se kreću ljudi i vozila (str. 92)." },
      { q:"Koji put je u ravnici?", options:["Ravni","Vijugavi","Strmi","Uski"], answer:"Ravni", explanation:"U ravnici većinom su ravni putevi (str. 92)." },
      { q:"Šta je semafor?", options:["Svetlo za saobraćaj","Karta","Kompas","Sat"], answer:"Svetlo za saobraćaj", explanation:"Semafor reguliše saobraćaj svetlima (str. 93)." },
      { q:"Koje vozilo vozi po šinama?", options:["Voz","Automobil","Bicikl","Avion"], answer:"Voz", explanation:"Vozovi voze po šinama (str. 93)." },
      { q:"Šta pomaže bezbednost?", options:["Pravila","Igra","Spavanje","Jelo"], answer:"Pravila", explanation:"Saobraćajna pravila pomažu bezbednosti (str. 93)." }
    ]
  },
  { oblast: "Vozila (Strana 93)", pitanja:[
      { q:"Šta je automobil?", options:["Vozilo na točkovima","Vozilo na vodi","Vozilo u vazduhu","Vozilo na šinama"], answer:"Vozilo na točkovima", explanation:"Automobil je vozilo sa točkovima (str. 93)." },
      { q:"Koje vozilo leti?", options:["Avion","Voz","Automobil","Bicikl"], answer:"Avion", explanation:"Avion je vozilo koje leti (str. 93)." },
      { q:"Šta je bicikl?", options:["Vozilo sa pedalama","Vozilo sa krilima","Vozilo sa točkovima","Vozilo na vodi"], answer:"Vozilo sa pedalama", explanation:"Bicikl se pokreće pedalama (str. 93)." },
      { q:"Gde vozi brod?", options:["U vazduhu","Na vodi","Na suvom","Na šinama"], answer:"Na vodi", explanation:"Brod plovi po vodi (str. 93)." },
      { q:"Koje vozilo ide brzo?", options:["Avion","Konj","Bicikl","Traktor"], answer:"Avion", explanation:"Avion je brzo vozilo (str. 93)." }
    ]
  },
  { oblast: "Bezbednost u saobraćaju (Strana 93)", pitanja:[
      { q:"Šta je prelaz?", options:["Mesto za prelazak","Vozilo","Put","Kuća"], answer:"Mesto za prelazak", explanation:"Pešački prelaz je mesto gde ljudi bezbedno prelaze ulicu (str. 93)." },
      { q:"Kako preći ulicu?", options:["Na zeleno","Na crveno","Bez gledanja","Trčeći"], answer:"Na zeleno", explanation:"Ulicu treba preći kada semafor pokazuje zeleno (str. 93)." },
      { q:"Šta nose deca u saobraćaju?", options:["Prsluk","Kapa","Rukavice","Čarape"], answer:"Prsluk", explanation:"Reflektujući prsluk pomaže vidljivost dece (str. 93)." },
      { q:"Koje boje ima semafor?", options:["Crvena, žuta, zelena","Plava, crvena","Žuta, plava","Zeleno, narandžasto"], answer:"Crvena, žuta, zelena", explanation:"Semafor koristi crvenu, žutu i zelenu boju (str. 93)." },
      { q:"Šta pomaže vidljivost?", options:["Prsluk","Kapa","Patike","Ruksak"], answer:"Prsluk", explanation:"Prsluk sa reflektujućim trakama povećava vidljivost (str. 93)." }
    ]
  },
  { oblast: "Kretanje (Strana 91)", pitanja:[
      { q:"Šta je kretanje?", options:["Pomeranje","Sedeti","Spavati","Jesti"], answer:"Pomeranje", explanation:"Kretanje znači pomerati se (str. 91)." },
      { q:"Kako se krećemo pešice?", options:["Hodanje","Vožnja","Letenje","Plivanje"], answer:"Hodanje", explanation:"Pešice se krećemo hodanjem (str. 91)." },
      { q:"Koja životinja se kreće brzo?", options:["Zečevi","Krokodili","Žabe","Ribe"], answer:"Zečevi", explanation:"Zečevi su brze životinje (str. 91)." },
      { q:"Šta pomaže kretanje?", options:["Noge","Krila","Rep","Oči"], answer:"Noge", explanation:"Noge pomažu u kretanju (str. 91)." },
      { q:"Gde se krećemo?", options:["Na putu","U vodi","U vazduhu","U šumi"], answer:"Na putu", explanation:"Ljudi se obično kreću po putu (str. 91)." }
    ]
  },
  { oblast: "Materijali (Strana 105-106)", pitanja:[
      { q:"Šta je materijal?", options:["Što čini stvari","Hrana","Voda","Vazduh"], answer:"Što čini stvari", explanation:"Materijal je supstanca od koje su stvari napravljene (str. 105)." },
      { q:"Koji materijal je tvrd?", options:["Kamen","Papir","Pamuk","Vuna"], answer:"Kamen", explanation:"Kamen je primer tvrdog materijala (str. 106)." },
      { q:"Koji materijal je mekan?", options:["Drvo","Pamuk","Gvožđe","Staklo"], answer:"Pamuk", explanation:"Pamuk je mekan materijal (str. 106)." },
      { q:"Šta je od drveta?", options:["Stol","Čaša","Nož","Kamen"], answer:"Stol", explanation:"Sto je često napravljen od drveta (str. 105)." },
      { q:"Koji materijal je tečan?", options:["Voda","Kamen","Drvo","Gvožđe"], answer:"Voda", explanation:"Voda je tečan materijal (str. 106)." }
    ]
  },
  { oblast: "Promene materijala (Strana 107-108)", pitanja:[
      { q:"Šta je topljenje?", options:["Čvrsto u tečnost","Tečnost u gas","Gas u čvrsto","Čvrsto u gas"], answer:"Čvrsto u tečnost", explanation:"Topljenje je prelazak iz čvrstog u tečno stanje (str. 107)." },
      { q:"Kada se led topi?", options:["U toplom","U hladnom","U vodi","U vazduhu"], answer:"U toplom", explanation:"Led se topi na toplom (str. 107)." },
      { q:"Šta je zamrzavanje?", options:["Tečnost u čvrsto","Čvrsto u tečnost","Gas u tečnost","Tečnost u gas"], answer:"Tečnost u čvrsto", explanation:"Zamrzavanje je prelazak iz tečnog u čvrsto stanje (str. 108)." },
      { q:"Kada voda postaje led?", options:["U hladnoći","U toploti","U kiši","U vetru"], answer:"U hladnoći", explanation:"Voda postaje led kada je veoma hladno (str. 108)." },
      { q:"Šta je isparavanje?", options:["Tečnost u gas","Gas u tečnost","Čvrsto u tečnost","Tečnost u čvrsto"], answer:"Tečnost u gas", explanation:"Isparavanje je prelazak tečnosti u gas (str. 108)." }
    ]
  }
]; // kraj quizData

// ELEMENTI
const homeEl = document.getElementById("home");
const homeGrid = document.getElementById("home-grid");
const quizEl = document.getElementById("quiz");
const oblastPill = document.getElementById("oblast-pill");
const pitanjePill = document.getElementById("pitanje-pill");
const tacnoPill = document.getElementById("tacno-pill");
const oblastTitle = document.getElementById("oblast-title");
const questionText = document.getElementById("question-text");
const optionsContainer = document.getElementById("options-container");
const explanationDiv = document.getElementById("explanation");
const nextBtn = document.getElementById("next-btn");
const backHomeBtn = document.getElementById("back-home");
const scoreDiv = document.getElementById("score");

let oblastIndex = 0;
let pitanjeIndex = 0;
let tacno = 0;

// Napravi dugmad za sve oblasti na početnoj strani
function renderHome(){
  homeGrid.innerHTML = "";
  quizData.forEach((ob, idx) => {
    const btn = document.createElement("button");
    btn.className = "area-btn";
    btn.textContent = `${idx+1}. ${ob.oblast}`;
    btn.onclick = () => startOblast(idx);
    homeGrid.appendChild(btn);
  });
  showHome();
}

function showHome(){
  homeEl.style.display = "";
  quizEl.style.display = "none";
  // scroll top
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function startOblast(idx){
  oblastIndex = idx;
  pitanjeIndex = 0;
  tacno = 0;
  showQuiz();
  prikaziPitanje();
}

function showQuiz(){
  homeEl.style.display = "none";
  quizEl.style.display = "";
  window.scrollTo({ top: 0, behavior: 'smooth' });
  scoreDiv.textContent = "";
  explanationDiv.textContent = "";
  nextBtn.style.display = "none";
}

function azurirajMeta(){
  oblastPill.textContent = `Oblast: ${oblastIndex+1} / ${quizData.length}`;
  pitanjePill.textContent = `Pitanje: ${pitanjeIndex+1} / ${quizData[oblastIndex].pitanja.length}`;
  tacnoPill.textContent = `Tačno: ${tacno}`;
}

// prikaži trenutno pitanje
function prikaziPitanje(){
  azurirajMeta();
  explanationDiv.textContent = "";
  nextBtn.style.display = "none";
  scoreDiv.textContent = "";
  const oblast = quizData[oblastIndex];
  const pitanje = oblast.pitanja[pitanjeIndex];
  oblastTitle.textContent = oblast.oblast;
  questionText.textContent = pitanje.q;

  // nasumično pomešaj opcije
  const shuffled = pitanje.options.slice().sort(()=>Math.random()-0.5);
  optionsContainer.innerHTML = "";

  shuffled.forEach(opt=>{
    const btn = document.createElement("button");
    btn.className = "opt";
    btn.textContent = opt;
    btn.onclick = () => odgovorClick(btn, pitanje);
    optionsContainer.appendChild(btn);
  });
}

function disableAllOptions(){
  const svi = document.querySelectorAll(".opt");
  svi.forEach(b=> b.disabled = true);
}

function odgovorClick(btn, pitanje){
  disableAllOptions();
  const svi = document.querySelectorAll(".opt");
  const odg = btn.textContent;
  if(odg === pitanje.answer){
    btn.classList.add("correct");
    tacno++;
    scoreDiv.textContent = "✅ Tačno!";
  } else {
    btn.classList.add("wrong");
    scoreDiv.textContent = "❌ Netačno!";
    // istakni pravi odgovor
    for(const b of svi) if(b.textContent === pitanje.answer) b.classList.add("correct");
  }
  explanationDiv.textContent = "💡 " + pitanje.explanation;
  nextBtn.style.display = "inline-block";
  azurirajMeta();
}

// sledeće pitanje ili kraj oblasti
nextBtn.addEventListener("click", ()=>{
  pitanjeIndex++;
  const oblast = quizData[oblastIndex];
  if(pitanjeIndex >= oblast.pitanja.length){
    // kraj oblasti
    prikaziKrajOblasti();
  } else {
    prikaziPitanje();
  }
});

// prikaži kraj oblasti + opcije za povratak/ponavljanje
function prikaziKrajOblasti(){
  oblastTitle.textContent = `✅ Završena oblast: ${quizData[oblastIndex].oblast}`;
  questionText.textContent = `Tačnih odgovora: ${tacno} / ${quizData[oblastIndex].pitanja.length}`;
  optionsContainer.innerHTML = `
    <div style="grid-column:1/-1; text-align:center; font-weight:700; padding:6px;">
      Možeš da ponoviš oblast ili se vratiš na početnu stranicu.
    </div>
  `;
  explanationDiv.innerHTML = `
    <div style="margin-top:6px;">
      <button id="ponovi" style="padding:10px 12px; margin-right:8px; border-radius:10px; border:none; cursor:pointer; background:#4fc3f7; color:white; font-weight:700;">🔁 Ponovi oblast</button>
      <button id="vrati" style="padding:10px 12px; border-radius:10px; border:2px solid #cfeefb; background:white; cursor:pointer; color:var(--accent); font-weight:700;">🏠 Vrati se na početnu</button>
    </div>
  `;
  nextBtn.style.display = "none";
  document.getElementById("ponovi").onclick = ()=> startOblast(oblastIndex);
  document.getElementById("vrati").onclick = ()=> renderHome();
  azurirajMeta();
}

// dugme vrati na home tokom kviza
backHomeBtn.addEventListener("click", ()=>{
  // potvrda? za decu izostavljeno, odmah se vraća
  renderHome();
});

// inicijalizacija home grid
renderHome();

</script>
</body>
</html>
