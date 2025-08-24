<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Memory Game</title>
  <style>
    body { display: flex; flex-direction: column; align-items: center; background: #fafafa; font-family: Arial; }
    h1 { margin: 20px; }
    .grid { display: grid; grid-template-columns: repeat(4, 100px); gap: 10px; }
    .card { width: 100px; height: 100px; background: #007bff; display: flex; justify-content: center; align-items: center; font-size: 2rem; color: #fff; cursor: pointer; border-radius: 8px; }
    .flipped { background: #fff; color: #000; }
  </style>
</head>
<body>
  <h1>Memory Game</h1>
  <div class="grid"></div>
  <p id="result"></p>

  <script>
    const grid = document.querySelector(".grid");
    const result = document.getElementById("result");
    let chosenCards = [];
    let chosenIds = [];
    let matches = 0;

    const cardArray = ["🍎", "🍌", "🍇", "🍉", "🍎", "🍌", "🍇", "🍉"];
    cardArray.sort(() => 0.5 - Math.random());

    function createBoard() {
      for (let i = 0; i < cardArray.length; i++) {
        const card = document.createElement("div");
        card.classList.add("card");
        card.setAttribute("data-id", i);
        card.innerText = "?";
        card.addEventListener("click", flipCard);
        grid.appendChild(card);
      }
    }

    function flipCard() {
      let id = this.getAttribute("data-id");
      if (chosenIds.includes(id)) return; // prevent double click
      this.classList.add("flipped");
      this.innerText = cardArray[id];
      chosenCards.push(cardArray[id]);
      chosenIds.push(id);

      if (chosenCards.length === 2) {
        setTimeout(checkMatch, 500);
      }
    }

    function checkMatch() {
      const cards = document.querySelectorAll(".card");
      const [id1, id2] = chosenIds;
      if (chosenCards[0] === chosenCards[1]) {
        matches++;
      } else {
        cards[id1].innerText = "?";
        cards[id1].classList.remove("flipped");
        cards[id2].innerText = "?";
        cards[id2].classList.remove("flipped");
      }
      chosenCards = [];
      chosenIds = [];
      result.innerText = `Matches: ${matches}`;
      if (matches === cardArray.length / 2) {
        result.innerText = "🎉 You won!";
      }
    }

    createBoard();
  </script>
</body>
</html>
