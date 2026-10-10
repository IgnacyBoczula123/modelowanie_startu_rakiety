# 🚀 Symulacja Dynamiki Lotu Rakiety: Starship IFT-5

Projekt numerycznej rekonstrukcji fazy wznoszenia rakiety w układzie o zmiennej masie na przykładzie piątego lotu testowego (**Integrated Flight Test 5 – IFT-5**) systemu Starship / Super Heavy (SpaceX) z 13 października 2024 roku.

---

## 👥 Autorzy
* **Ignacy Boczula**
* **Jacek Hodakowski**

---

## 📌 Opis Projektu

Głównym celem projektu jest symulacja i analiza numeryczna dynamiki lotu rakiety wielostopniowej w gęstych warstwach atmosfery oraz w zmiennym polu grawitacyjnym Ziemi. Model bada bilans energetyczny oraz straty prędkości ($\Delta v$), które w warunkach rzeczywistych degradują idealne osiągi wynikające z klasycznego równania Ciołkowskiego.

W projekcie udało się z powodzeniem:
1. **Zrekonstruować rzeczywistą trajektorię wznoszenia misji Starship IFT-5** do momentu wyłączenia silników pierwszego stopnia (MECO).
2. **Zbadać kluczowe metryki inżynierskie** (Max-Q, profil przeciążeń, TWR).
3. **Przeprowadzić serię pouczających symulacji alternatywnych**, demonstrujących fizyczne konsekwencje zjawisk takich jak brak dławienia ciągu czy brak ubytku masy.

---

## 🛠️ Wykorzystane Narzędzia i Technologie

Projekt został w całości zaimplementowany w języku **Python** w środowisku **Jupyter Notebook**:

* **Python 3.x** – główny język obliczeń i modelowania,
* **NumPy** – wektoryzacja obliczeń numerycznych i implementacja równań różniczkowych,
* **SciPy** (`scipy.interpolate`) – interpolacja programu sterowania kątem pochylenia (*pitch schedule*),
* **Matplotlib** – wizualizacja profili wysokości, prędkości, kątów oraz metryk inżynierskich,
* **Pandas** – strukturyzacja i przetwarzanie danych telemetrycznych.

---

## 🔬 Zastosowane Modele Fizyczne i Numeryczne

Projekt wykorzystuje układ równań różniczkowych sterowanego lotu płaskiego (2D) oparty na równaniu Mieszczerskiego:

* **Stan układu:** Wektor zmiennych stanu:
  $$\mathbf{y}(t) = [x, h, v, \gamma, m]$$
  gdzie:
  - $x$ – dystans poziomy (*downrange*),
  - $h$ – wysokość nad poziomem morza,
  - $v$ – prędkość całkowita,
  - $\gamma$ – kąt toru lotu (*flight path angle*),
  - $m$ – masa układu (konstrukcja + paliwo).

* **Model grawitacyjny:** Prawo powszechnego ciążenia Newtona zrelatywizowane względem promienia Ziemi $R_Z$:
  $$g(h) = g_0 \cdot \left(\frac{R_Z}{R_Z + h}\right)^2$$

* **Model atmosfery:** Wykładniczy model gęstości ze skalą wysokości $H \approx 8500\text{ m}$:
  $$\rho(h) = \rho_0 \cdot \exp\left(-\frac{h}{H}\right)$$

* **Model prędkości dźwięku:** Liniowy gradient termiczny w troposferze ($0 - 11\text{ km}$):
  $$v_{sound}(h) = v_{SL} - k \cdot h$$

* **Aerodynamika i opór:** Skokowa zmiana współczynnika oporu falowego $C_d$ w reżimie transonicznym wokół $\text{Mach } 1$:
  $$D = \frac{1}{2} \rho(h) v^2 C_d A$$

* **Algorytm całkujący:** Własna wektorowa implementacja metody **Rungego-Kutty 4. rzędu (RK4)**, zapewniająca wysoką stabilność i dokładność całkowania przy małym kroku czasowym ($dt = 0{,}05\text{ s}$).

---

## 📊 Kluczowe Metryki Inżynierskie

Podczas symulacji w sposób ciągły monitorowane są parametry krytyczne dla integralności konstrukcji:
* **Maksymalne ciśnienie dynamiczne ($Max-Q$):**
  $$q = \frac{1}{2} \rho v^2$$
* **Profil przeciążeń ($G\text{-Force}$):**
  $$a_G = \frac{T - D}{m \cdot g_0}$$
* **Stosunek ciągu do ciężaru ($TWR$):**
  $$TWR = \frac{T}{m \cdot g(h)}$$

---

## 🧪 Przeprowadzone Symulacje i Wnioski

Poza wierną rekonstrukcją lotu IFT-5, w projekcie zaimplementowano i porównano scenariusze badawcze:

1. **Scenariusz bazowy (IFT-5 – Pełna rekonstrukcja):**
   * Płynne przejście przez strefę transsoniczną przy zredukowanym ciągu silników (*engine throttling*).
   * Prawidłowe zlokalizowanie punktu $Max-Q$ oraz bezpieczny poziom przeciążeń konstrukcji przed separacją stopni.
2. **Brak dławienia silników (100% Throttle):**
   * Wykazano gwałtowny wzrost ciśnienia dynamicznego w strefie $Max-Q$ znacznie przekraczający limity wytrzymałościowe kadłuba, co w rzeczywistych warunkach doprowadziłoby do katastrofy aerodynamicznej (*RUD*).
3. **Symulacja układu o stałej masie:**
   * Obrazuje „tyranię równania rakietowego” – bez dynamicznej redukcji masy wypalanego paliwa rakieta nie jest w stanie pokonać oporów grawitacyjnych i osiągnąć prędkości niezbędnej do wejścia na orbitę.

---

## 📁 Struktura Repozytorium

```text
├── modelowanie_startu_rakiety.ipynb   # Główny notatnik z wyprowadzeniami, kodem i wykresami
├── README.md                          # Dokumentacja projektu
└── requirements.txt                   # Zależności i biblioteki
