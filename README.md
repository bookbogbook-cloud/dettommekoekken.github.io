<!DOCTYPE html>
<html lang="da">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Det tomme køkken</title>
  <!-- Det her er en prototype, ikke den rigtige app -->
  <style>
    body { font-family: Cursive, sans-serif; background:#a9c7ee; margin:0; padding:20px; }
    h1 { text-align:center; color:#29353C; }
    .container { max-width:700px; margin:0 auto; background:#FFE8E1; padding:20px; border-radius:12px; box-shadow:0 5px 15px rgba(0,0,0,0.1); }
    label { font-weight:bold; display:block; margin-bottom:8px; }
    input, button, select { padding:8px; font-size:14px; border-radius:6px; border:1px solid #ccc; margin-bottom:12px; width:100%; box-sizing:border-box; }
    button { background:#768A96; color:white; border:none; cursor:pointer; padding:8px 12px; display:inline-block; width:auto; }
    button:hover { background:#6a7b86; }
    ul { list-style:none; padding:0; }
    li { padding:10px; border-bottom:1px solid #eee; }
    .missing { color:#F3E4F5; }
    .ready { color:#A7ABDE; }
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
    // small test dataset so you can verify button works
    const recipes = [
      { name: "Tomatpasta", ingredients: ["tomat","pasta","olivenolie","hvidløg","salt"] },
      { name: "Ostomelet", ingredients: ["æg","mælk","ost","salt","peber"] },
      { name: "Pandekager", ingredients: ["mel","mælk","æg","sukker","smør"] }
    ];

    // Hent elementer
    const ingredientsInput = document.getElementById('ingredients');
    const findBtn = document.getElementById('findBtn');
    const recipeList = document.getElementById('recipeList');

    function findRecipes() {
      const userIngredients = ingredientsInput.value
        .toLowerCase()
        .split(',')
        .map(i => i.trim())
        .filter(Boolean);

      recipeList.innerHTML = '';

      if (userIngredients.length === 0) {
        recipeList.innerHTML = '<li>Skriv mindst én ingrediens.</li>';
        return;
      }

      const possibleRecipes = recipes.filter(recipe =>
        recipe.ingredients.every(i => userIngredients.includes(i))
      );

      if (possibleRecipes.length === 0) {
        recipeList.innerHTML = '<li>Desværre får du ingen mad i dag med de ingredienser.</li>';
        return;
      }

      possibleRecipes.forEach(recipe => {
        const li = document.createElement('li');
        li.className = 'ready';
        li.innerHTML = `<strong>${recipe.name}</strong><br>
          <em>Ingredienser:</em> ${recipe.ingredients.join(', ')}<br>
          <em>Du kan lave denne!</em>`;
        recipeList.appendChild(li);
      });
    }

    findBtn.addEventListener('click', findRecipes);
  </script>
</body>
</html>
