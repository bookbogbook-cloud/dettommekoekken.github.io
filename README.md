<!DOCTYPE html>
<html lang="da">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Det tomme køkken</title>

  <style>
    body { 
      font-family: Cursive, sans-serif; 
      background:#a9c7ee; 
      margin:0; 
      padding:20px; 
    }
    h1 { 
      text-align:center; 
      color:#29353C; 
    }
    .container { 
      max-width:700px; 
      margin:0 auto; 
      background:#FFE8E1; 
      padding:20px; 
      border-radius:12px; 
      box-shadow:0 5px 15px rgba(0,0,0,0.1); 
    }
    label { 
      font-weight:bold; 
      display:block; 
      margin-bottom:8px; 
    }
    input, button { 
      padding:8px; 
      font-size:14px; 
      border-radius:6px; 
      border:1px solid #ccc; 
      margin-bottom:12px; 
      width:100%; 
      box-sizing:border-box; 
    }
    button { 
      background:#768A96; 
      color:white; 
      border:none; 
      cursor:pointer; 
      width:auto; 
      display:inline-block; 
      padding:8px 12px;
    }
    button:hover { 
      background:#6a7b86; 
    }
    ul { 
      list-style:none; 
      padding:0; 
    }
    li { 
      padding:10px; 
      border-bottom:1px solid #eee; 
    }
    .ready { 
      color:#29353C; 
    }
  </style>
</head>

<body>
  <div class="container">
    <h1>Det tomme køkken</h1>

    <label for="ingredients">Enter ingredients you have (comma separated):</label>
    <input type="text" id="ingredients" placeholder="fx. æg, mælk, tomater, ost">

    <button id="findBtn">Find Recipes</button>

    <h2>Matching Recipes:</h2>
    <ul id="recipeList"></ul>
  </div>

  <script>
    const recipes = [
      { name: "Bananpandekage", ingredients: ["banan","æg"] },
      { name: "Nutella-toast", ingredients: ["nutella","brød"] },
      { name: "Avocado-mad", ingredients: ["avocado","brød"] },
      { name: "Yoghurt med honning", ingredients: ["yoghurt","honning"] },
      { name: "Ostemad", ingredients: ["ost","brød"] },
      { name: "Peanutbutter-toast", ingredients: ["peanutbutter","brød"] },
      { name: "Tomatmad", ingredients: ["tomat","brød"] },
      { name: "Agurkebidder", ingredients: ["agurk","salt"] },
      { name: "Kiks med ost", ingredients: ["kiks","ost"] },
      { name: "Kiks med peanutbutter", ingredients: ["kiks","peanutbutter"] },
      { name: "Kold kakao", ingredients: ["mælk","kakaopulver"] },
      { name: "Koldskål light", ingredients: ["yoghurt","sukker"] },
      { name: "Lille omelet", ingredients: ["æg","salt"] },
      { name: "Røræg basic", ingredients: ["æg","smør"] },
      { name: "Mini-pizza", ingredients: ["brød","tomatsauce"] },
      { name: "Råkost", ingredients: ["gulerod","citron"] },
      { name: "Agurkesalat hurtig", ingredients: ["agurk","eddike"] },
      { name: "Tomatsalat mini", ingredients: ["tomat","olivenolie"] },
      { name: "Melon snack", ingredients: ["melon","salt"] },
      { name: "Æble snack", ingredients: ["æble","kanel"] },
      { name: "Kakao-banan", ingredients: ["banan","kakao"] },
      { name: "Hvidløgsbrød", ingredients: ["brød","hvidløg"] },
      { name: "Smørtoast", ingredients: ["brød","smør"] },
      { name: "Majs snack", ingredients: ["majs","smør"] },
      { name: "Tunpure", ingredients: ["tun","mayonnaise"] },
      { name: "Chips & dip", ingredients: ["chips","cremefraiche"] },
      { name: "Popcorn", ingredients: ["majs","olie"] },
      { name: "Milkshake basic", ingredients: ["mælk","is"] },
      { name: "Is-sandwich", ingredients: ["kiks","is"] },
      { name: "Frugtsalat simpel", ingredients: ["æble","banan"] },
      { name: "Pita med ost", ingredients: ["pita","ost"] },
      { name: "Spegepølse-mad", ingredients: ["brød","spegepølse"] },
      { name: "Tun-toast", ingredients: ["brød","tun"] },
      { name: "Honningristet brød", ingredients: ["brød","honning"] },
      { name: "Kartoffel-snack", ingredients: ["kartoffel","salt"] },
      { name: "Risgrød basic", ingredients: ["ris","mælk"] },
      { name: "Havregrød basic", ingredients: ["havregryn","vand"] },
      { name: "Gnocchi pannestegt", ingredients: ["gnocchi","smør"] },
      { name: "Pestomad", ingredients: ["brød","pesto"] },
      { name: "Tomatpesto", ingredients: ["tomat","pesto"] },
      { name: "Peberfrugtsnack", ingredients: ["peberfrugt","olie"] },
      { name: "Æggesandwich mini", ingredients: ["æg","brød"] },
      { name: "Choko-bolle", ingredients: ["bolle","nutella"] },
      { name: "Rå løg med salt", ingredients: ["løg","salt"] },
      { name: "Smoothie simpel", ingredients: ["banan","mælk"] },
      { name: "Bagt banan", ingredients: ["banan","honning"] },
      { name: "Æblechips", ingredients: ["æble","sukker"] },
      { name: "Kiksekage hurtig", ingredients: ["kiks","chokolade"] },
      { name: "Is-kaffe light", ingredients: ["kaffe","is"] }
    ];

    const ingredientsInput = document.getElementById("ingredients");
    const recipeList = document.getElementById("recipeList");
    const findBtn = document.getElementById("findBtn");

    function findRecipes() {
      const userIngredients = ingredientsInput.value
        .toLowerCase()
        .split(",")
        .map(i => i.trim())
        .filter(Boolean);

      recipeList.innerHTML = "";

      if (userIngredients.length === 0) {
        recipeList.innerHTML = "<li>Skriv mindst én ingrediens.</li>";
        return;
      }

      const possible = recipes.filter(r =>
        r.ingredients.every(ing => userIngredients.includes(ing))
      );

      if (possible.length === 0) {
        recipeList.innerHTML = "<li>Du kan ikke lave noget med de ingredienser 😭</li>";
        return;
      }

      possible.forEach(r => {
        const li = document.createElement("li");
        li.className = "ready";
        li.innerHTML = `<strong>${r.name}</strong><br>
          <em>Ingredienser:</em> ${r.ingredients.join(", ")}`;
        recipeList.appendChild(li);
      });
    }

    findBtn.addEventListener("click", findRecipes);
  </script>

</body>
</html>
