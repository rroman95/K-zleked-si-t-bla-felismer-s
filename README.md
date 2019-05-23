# Közlekedési táblák felismerése és osztályozása valós időben

### Feladat ###
Közlekedési táblák felismerése valós idöben

### Környezet ###
Python futtatására alkalmas környezet (Windows 10 - 64bit), Python fordító program - Spyder (Python 3.6.5 64bit) , OpenCV 4.1.0 verzió.

### 1. Leírás ###
Ez a program közlekedési táblák felismerésére szolgál az OpenCV könyvtárat és a Python nyelvet használva.
Felismerhetö táblák:

![Screenshot](images/allsigns.jpg)

### 2. Felismerés ###
  
  Kép előkészítése az OpenCV funkciók használatára
  - cv2.medianBlur filter alkalmazása zaj eltüntetése és élek elsimítása érdekében (így kissebb esélyel észlel a program hamis kör alakzatot)
  - cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY) funkció alkalmazása - a kép átkonvertálása 8-bit single channel képpé ugyanis a cv2.HoughCircles
    funkció csak ilyenekkel tud dolgozni.

  Kör és kör köré írt hatszögek felismerése.
      A kör és hasonló alakzatok felismerését az OpenCV cv2.HoughCircles funkciója segítségével végezzük, mely kizárólag szürkeárnyalatos 
      képekkel tud dolgozni.
      
   Táblák felismerése
      A körök felismerése után a get_dominant_color funkció segítségével meghatározzuk, hogy a kör által lefedett területen mi a
      domináns szín.
      Ez által le tudjuk szükiteni a lehetséges táblákat ELSÖBBSÉGET SZABÁLYOZÓ táblákra (PIROS -> if dominant_color[2] > 100:) vagy
      UTASÍTÁST ADÓ jelzötáblákra (KÉK -> elif dominant_color[0] > 80:). 
      
      Mivel a program az elsöbbséget adó táblák közül csupán a STOP táblát tudja felismerni, így ha a domináns szín 100 felett van,
      automatikan STOP táblát
      ismer fel a program.
      Ha a "elif dominant_color[0] > 80:" érvényesül, a program 3 zónára szegmentálja a kört. 
      Ezen zónákon belül külön leelemzi a domináns színt,
      így meghatározvaa program által felismerhetö UTASÍTÁST ADÓ jelzötáblák fajtáját.
      
### 3. Lehetséges problémák ###

A felismerési módszer primitívsége miatt a program hajlamos a perspektívából és fényviszonyokból adódóan félreismerni bizonyos táblákat.
A tábla szegmentálási módszer általi felismerése tévesen ismerhet fel alakzatokat, hiányos, "defektív" táblákat bizonyos táblaként.
Példa: 




![Screenshot](images/forwardandright_false.jpg)
![Screenshot](images/stopsign_false.jpg)


