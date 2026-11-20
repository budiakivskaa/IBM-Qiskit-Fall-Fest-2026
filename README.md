# Kwantowa Kostka 

Interaktywna demonstracja mechaniki kwantowej w przeglądarce — działa w pełni offline, bez żadnych zależności. Stworzona na **Qiskit Fall Fest**.

## Demo

Otwórz plik `quantum_dice_v2.html` w przeglądarce. Nie jest wymagany serwer ani instalacja.

---

## Idea

Kostka do gry zbudowana jest z **3 kubitów**, co daje 8 możliwych wyników (000–111). Wynik odczytuje się jako liczbę binarną i dodaje 1 — otrzymując ściankę 1–6. Wyniki `110` i `111` oznaczają, że **kostka spadła ze stołu** i rzut jest powtarzany.

Gracz ustawia kubity przeciągając punkt na **sferze Blocha** i obserwuje, jak zmienia się rozkład prawdopodobieństwa ścianek w czasie rzeczywistym.

---

## Ekrany

### 🎲 Ekran 1 — Ustaw kostkę

Trzy sfery Blocha dla kubitów **A** (MSB), **B** i **C** (LSB).

- **Przeciąganie lewym przyciskiem** → przesuwa punkt kwantowy na sferze (zmienia θ i φ)
- **Przeciąganie prawym przyciskiem lub Ctrl+drag** → obraca kamerę, by obejrzeć kulę z każdej strony
- **Przycisk H** przy każdej sferze → nakłada bramkę Hadamarda z animacją przejścia punktu
- **Ściąga** (tabelka wynik → kubity A B C) z podświetleniem najbardziej prawdopodobnej ścianki
- **Histogram teoretyczny** — aktualizuje się na żywo przy każdym ruchu
- **Histogram rzeczywisty** — wypełnia się po kliknięciu „Rzuć 120 razy"; licznik pokazuje ile razy kostka „spadła ze stołu"
- **Animowana kostka 3D** do pojedynczego rzutu

**Przykłady do demonstracji:**
| Ustawienie | Efekt |
|---|---|
| Kubity w pozycji południe-północ-południe | Szóstka w 100% |
| H na wszystkich trzech od stanu \|000⟩ | Uczciwa kostka (równy rozkład) |
| H → obrót fazy 180° → H | Pewne 0 zamienia się w pewne 1, choć po drodze histogram wyglądał losowo |

---

### 🔗 Ekran 2 — Splącz z kostką kolegi

Dwie kostki: **X** (twoja, edytowalna) i **Y** (kolegi, startuje z \|000⟩).

- **Trzy sfery Blocha dla każdej kostki** — drag i przycisk H tak samo jak na ekranie 1
- **Kable CNOT** — kliknięcie kabla splata odpowiadające sobie kubity kostek X i Y przez bramkę CNOT
  - Gdy kubit zostaje splątany, jego punkt **zapada się do wnętrza kuli**
  - Przy pełnym splątaniu punkt ląduje w środku — kubit nie ma już własnego stanu
  - Odłączenie kabla powoduje powrót punktu na powierzchnię
- **Tryb zaawansowany** — nakłada H na wszystkie kubity tuż przed pomiarem (pomiar „pod innym kątem"), co pozwala odróżnić prawdziwe splątanie od klasycznej korelacji
- **Histogramy X i Y** osobno + **siatka 6×6** wspólnych wyników
- **Rzuć tylko X** → histogram Y natychmiast zapada się do jednego słupka (kolaps splątanego stanu)

**Co można odkryć:**
- 1 kabel → kostki zawsze mają tę samą parzystość
- 3 kable → zawsze ta sama liczba na obu kostkach (przekątna siatki)
- Tryb zaawansowany → korelacja splątana utrzymuje się, korelacja klasyczna ginie

---

## Silnik kwantowy

Cały kod zawarty jest w jednym pliku HTML (~1400 linii), bez frameworków.

| Klasa / funkcja | Opis |
|---|---|
| `QState(n)` | Wektor amplitud zespolonych dla n kubitów (2ⁿ liczb). Operacje: Ry, Rz, H, CNOT, sample, collapse |
| `QState6` | 6-kubitowy stan (64 amplitudy) dla ekranu 2; metody marginalnych prawdopodobieństw, próbkowania par |
| `blochVector(k)` | Oblicza wektor Blocha kubitu k z macierzy gęstości (Tr_others). Dla splątanego zwraca punkt wewnątrz kuli (r < 1) |
| `drawBloch()` | Renderer sfery 3D na `<canvas>` — siatka równoleżników/południków, rzut ortograficzny, obrót kamery |
| `diceProbs(p8)` | Przekształca 8 prawdopodobieństw bazowych na 7 wartości (ścianki 1–6 + „spadła ze stołu") |

### Bramki

- **Ry(θ)** — obrót wokół osi Y sfery Blocha; wyznacza stosunek P(|0⟩) do P(|1⟩)
- **Rz(φ)** — obrót wokół osi Z; ustawia fazę przy |1⟩
- **H** — `(a, b) → ((a+b)/√2, (a−b)/√2)` dla każdej pary amplitud różniących się danym bitem
- **CNOT** — zamienia amplitudy kubitu docelowego tam, gdzie kubit sterujący ma 1

### Sfera Blocha a splątanie

Dla kubitu niesplątanego macierz gęstości zredukowana `ρ = Tr_B(|ψ⟩⟨ψ|)` odpowiada punktowi na **powierzchni** kuli (czystość = 1). Dla kubitu splątanego `ρ = I/2` (stan maksymalnie mieszany), co odpowiada **środkowi** kuli — kubit nie ma własnego kierunku.

---

## Sterowanie sferą Blocha

| Akcja | Efekt |
|---|---|
| Lewy przycisk myszy + drag | Przesuwa punkt kwantowy (zmienia stan kubitu) |
| Prawy przycisk myszy + drag | Obraca kamerę |
| Ctrl + drag | Obraca kamerę |
| Przycisk H | Nakłada bramkę Hadamarda z animacją |

---

## Wymagania

- Nowoczesna przeglądarka (Chrome, Firefox, Safari, Edge)
- Brak połączenia z internetem — plik działa w pełni lokalnie
- Responsywny układ działa na tabletach i telefonach

---

*Projekt stworzony na Qiskit Fall Fest.*
