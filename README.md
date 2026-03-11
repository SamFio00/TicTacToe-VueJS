<p align="center">
  <a href="https://tictactoe-samfio00.netlify.app/" target="_blank">
    <img src="https://image.thum.io/get/width/900/https://tictactoe-samfio00.netlify.app" alt="TicTacToe Prewiew" />
  </a>
</p>

# Tic Tac Toe


Una reinterpretazione moderna del classico **Tic Tac Toe**, sviluppata con **Vue.js**.

---

## Demo

Gioca qui:

https://tictactoe-samfio00.netlify.app/

---

## Funzionalità

- Partita locale a due giocatori
- Rilevamento automatico del vincitore
- Rilevamento del pareggio
- Linea animata sulla combinazione vincente
- Celle vincenti evidenziate
- Animazioni per simboli e stato di gioco
- Pulsante di reset della partita
- Layout completamente responsive

---

## Tecnologie utilizzate

Il progetto è sviluppato utilizzando:

- **Vue 3** (Composition API)
- **Vite**
- **SCSS**
- **JavaScript**
- **HTML5 / CSS3**

---

## Componenti principali

### App.vue

È il componente principale dell'applicazione e gestisce lo **stato del gioco**.

Contiene:

- stato della board
- giocatore corrente
- rilevamento del vincitore
- rilevamento del pareggio
- reset della partita

---

### GameBoard.vue

Responsabile della **visualizzazione della griglia di gioco**.

Gestisce:

- rendering delle celle
- click sulle celle
- evidenziazione della combinazione vincente
- visualizzazione della linea vincente

---

### GameStatus.vue

Mostra lo **stato corrente della partita**:

- giocatore attuale
- vincitore
- pareggio

---

## Logica di gioco

La logica controlla tutte le possibili combinazioni vincenti:

- righe
- colonne
- diagonali

Quando una combinazione viene soddisfatta:

1. viene identificato il giocatore vincitore  
2. le celle coinvolte vengono evidenziate  
3. viene disegnata una **linea animata sulla combinazione vincente**

Se la griglia si riempie senza vincitori, la partita termina in **pareggio**.

---

## Stile e animazioni

Il design dell'interfaccia si ispira a un'estetica **neon/cyber**.

Gli stili sono organizzati tramite SCSS in:

- variabili condivise
- animazioni riutilizzabili
- stili scoped nei componenti

Tra gli effetti visivi principali:

- simboli con effetto glow
- animazioni di comparsa dei simboli
- titolo con effetto flicker
- linea vincente animata

---

## Autore

@2026 - Samuele Fiorini   
- GitHub: https://github.com/SamFio00  
- LinkedIn: https://www.linkedin.com/in/samuele-fiorini-38bba9325  
- Instagram: https://www.instagram.com/fiorini_sam_00
