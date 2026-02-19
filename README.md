# Gestione Energetica Casa Fotovoltaica

Un'applicazione Java a riga di comando per monitorare e ottimizzare i consumi domestici in base alla produzione di un impianto fotovoltaico.

---

# Descrizione

Il programma permette di simulare lo stato energetico della propria abitazione in tempo reale, confrontando la produzione solare con il consumo degli elettrodomestici attivi. L'utente può aggiungere o rimuovere elettrodomestici, inserire le condizioni ambientali (ora, stagione, meteo) e ricevere un feedback su surplus o deficit energetico.

---

# Struttura del Progetto

```
progetto
─ Main.java                      # Entry point e gestione del menu
─ Elettrodomestici.java          # Classe astratta base
─ CaricoCiclico.java             # Elettrodomestici a cicli (es. lavatrice, forno)
─ CaricoContinuo.java            # Elettrodomestici sempre accesi (es. frigorifero)
─ CaricoManuale.java             # Elettrodomestici ad uso manuale (es. phon, condizionatore)
─ DatabaseElettrodomestici.java  # Gestione catalogo e lista attivi
─ Fotovoltaico.java              # Calcolo produzione solare
```

# File CSV richiesti (nella directory di esecuzione)

```
carico_Ciclico.csv     # nome, potenza_min, potenza_media, potenza_max
carico_Manuale.csv     # nome, potenza_nominale
carico_Continuo.csv    # nome, consumo_annuo_kWh
```

---

# Architettura delle Classi

```
Elettrodomestici  (abstract)
─ CaricoCiclico    >   potenza selezionabile tra 3 livelli (min/medio/max)
─ CaricoContinuo   >   consumo distribuito sulle 24h (kWh/anno → kW/h)
─ CaricoManuale    >   potenza fissa × tempo d'uso

Fotovoltaico       >    produzione in kW in base a ora, stagione e meteo
DatabaseElettrodomestici > catalogo + lista elettrodomestici attivi
```

---

# Come Avviare

1. **Compilare** tutti i file `.java` nella cartella `progetto/`:
   ```bash
   javac progetto/*.java
   ```

2. **Posizionare i file CSV** nella stessa directory da cui si esegue il programma.

3. **Eseguire**:
   ```bash
   java progetto.Main
   ```

---

# Funzionalità del Menu

| Opzione | Descrizione |
|--------|-------------|
| `0` | Esci dal programma |
| `1` | Aggiungi un elettrodomestico al calcolo |
| `2` | Rimuovi un elettrodomestico dal calcolo |
| `3` | Calcola lo stato energetico (produzione vs consumo) |
| `4` | Visualizza gli elettrodomestici attualmente in funzione |

---

# Elettrodomestici Supportati

| Nome | Tipo |
|------|------|
| Lavatrice | Ciclico |
| Lavastoviglie | Ciclico |
| Forno | Ciclico |
| Phon | Manuale |
| Friggitrice ad aria | Manuale |
| Condizionatore | Manuale |
| Aspirapolvere | Manuale |
| Frigorifero | Continuo |

---

# Modello di Produzione Fotovoltaica

La produzione dipende da tre variabili:

- **Ora del giorno** (0–23): curve di irraggiamento precaricate per ogni stagione
- **Stagione**: primavera, estate, autunno, inverno
- **Meteo**:
  - `1` = Sole > 100% della potenza massima
  - `2` = Nuvoloso > 40%
  - `3` = Pioggia > 15%

---

# Note e Limitazioni Attuali

- I percorsi dei file CSV sono relativi alla directory di esecuzione: assicurarsi di avviare il programma dalla cartella corretta.
- Il catalogo degli elettrodomestici è fisso e va ampliato manualmente nei file CSV e in `DatabaseElettrodomestici.java`.
  
---

# Autore

Progetto sviluppato nell'ambito del corso di Programmazione ad Oggetti in Java.
