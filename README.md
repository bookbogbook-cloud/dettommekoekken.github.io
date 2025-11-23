<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title> Dit lokale tomme køkken! </title> (Det her er en prototype, ikke den rigtige app)
<style>
  body { font-family: Cursive, sans-serif; background:#a9c7ee; margin:0; padding:20; }
  h1 { text-align:center; color:#29353C; }
  .container { max-width:700; margin:0 auto; background:#FFE8E1; padding:20; border-radius:12; box-shadow:0 5 15 (0,0,0,0.1);}
  label { font-weight:bold; display:block; margin-bottom:8; }
  input, button, select { padding:8; font-size:14; border-radius:6; border:1 solid #ccc; margin-bottom:12; width:100; }
  button { background:#768A96; color:white; border:none; cursor:pointer; }
  button:hover { background:#daf0ff; }
  ul { list-style:none; padding:0; }
  li { padding:10; border-bottom:1 solid #eee; }
  .missing { color:#F3E4F5; }
  .ready { color:#A7ABDE0; }
</style>
</head>
<body>
<div class="container">
  <h1>Dit lokale tomme køkken! </h1>
  <label for="ingredients">Indtast de ingredienser du vil have med i din kreative ret!:):</label>
  <input type="text" id="ingredients" placeholder="fx. æg, mælk, tomater, ost">
  <button id="findBtn">Vis passende lækre opskrifter:></button>
  <h2>Opskrifter der matcher med dine ingredienser:D:</h2>
  <ul id="recipeList"></ul>
</div>

<script>
// Sample recipe database
 recipes  [
  { navn"Tomatpasta", ingredienser["tomat","pasta","olivenolie","hvidløg","salt"] },
  { navn"Ostomelet", ingredienser["æg","mælk","ost","salt","peber"] },
  { navn"Grillet ostesandwich", ingredienser["brød","ost","smør"] },
  { navn"Surdejsbrød", ingredienser["mel","vand","salt"] },
  { navn"Simpel salat", ingredienser["salat","tomat","agurk","olivenolie","salt"] },
  { navn"Kartoffelmos", ingredienser["kartofler","smør","mælk","salt","peber"] },
  { navn"Kyllingesalat", ingredienser["kylling","salat","majs","agurk","dressing"] },
  { navn"Røræg", ingredienser["æg","mælk","smør","salt","peber"] },
  { navn"Havreboller", ingredienser["havregryn","vand","mel","salt"] },
  { navn"Frugtsalat", ingredienser["æble","banan","appelsin","pære","melon"] },
  { navn"Tunsalat", ingredienser["tun","majs","løg","mayonnaise","citron"] },
  { navn"Karrysuppe", ingredienser["kylling","løg","gulerod","karry","fløde"] },
  { navn"Lasagne", ingredienser["lasagneplader","spuash","tomatsovs","ost","løg"] },
  { navn"Boller", ingredienser["mel","gær","vand","salt","smør"] },
  { navn"Pizzadej", ingredienser["mel","gær","vand","olivenolie","salt"] },
  { navn"Pizza Margherita", ingredienser["pizzadej","tomatsovs","ost","basilikum"] },
  { navn"Smoothie", ingredienser["banan","jordbær","blåbær","isterninger"] },
  { navn"Græsk salat", ingredienser["feta","tomat","agurk","oliven","løg"] },
  { navn"Stegt ris", ingredienser["ris","æg","majs","ærter","salt"] },
  { navn"Pastasalat", ingredienser["pasta","majs","skinke","ærter","æg"] },
  { navn"Kylling i karry", ingredienser["kylling","karry","løg","fløde","ris"] },
  { navn"Stegt æbletærte", ingredienser["æble","kanel","smør","mel","salt"] },
  { navn"Kakao Dadelboller", ingredienser["mel","dadler","æg","kakao","smør"] },
  { navn"Kanelbidder", ingredienser["smør","kanel","æg","mel"] },
  { navn"Kakao og bananbrød", ingredienser["banan","mel","æg","kakao","smør"] },
  { navn"Tomatsuppe", ingredienser["tomat","løg","hvidløg","bouillon","fløde"] },
  { navn"Ratatouille", ingredienser["aubergine","squash","tomat","peberfrugt","løg"] },
  { navn"Kyllingesandwich", ingredienser["brød","kylling","salat","tomat","mayonnaise"] },
  { navn"Linsesuppe", ingredienser["linser","løg","gulerod","hvidløg","bouillon"] },
  { navn"Spaghetti sorte bønner", ingredienser["sorte bønner","tomat","spaghetti","løg","hvidløg"] },
  { navn"Fiskefrikadeller", ingredienser["fisk","æg","mel","salt","peber"] },
  { navn"Kartoffelsalat", ingredienser["kartofler","mayonnaise","sennep","purløg"] },
  { navn"Æggemadder", ingredienser["æg","rugbrød","smør","salt"] },
  { navn"Hotdog", ingredienser["pølse","brød","sennep","ketchup","ristet løg"] },
  { navn"Burger", ingredienser["kidneybønner","bolle","ost","salat","tomat"] },
  { navn"Falafel", ingredienser["kikærter","løg","hvidløg","persille","mel"] },
  { navn"Hummus", ingredienser["kikærter","tahini","citron","hvidløg","olivenolie"] },
  { navn"Tzatziki", ingredienser["yoghurt","agurk","hvidløg","citron","salt"] },
  { navn"Rugbrødsmad med laks", ingredienser["rugbrød","røget laks","flødeost","dild","citron"] },
  { navn"Taco", ingredienser["tacobrød","kikærter","tomat","salat","ost"] },
  { navn"Wraps", ingredienser["tortilla","kylling","salat","agurk","dressing"] },
  { navn"Blåbær muffins", ingredienser["mel","blåbær","æg","smør","bagepulver"] },
  { navn"Bananpandekager", ingredienser["banan","æg","mel","mælk"] },
  { navn"Smoothie bowl", ingredienser["banan","jordbær","yoghurt","blåbær"] },
  { navn"Toast", ingredienser["brød","ost","skinke","smør"] },
  { navn"Pasta med pesto", ingredienser["pasta","pesto","parmesan","olivenolie"] },
  { navn"Koldskål", ingredienser["kærnemælk","æg","citronsaft eller skræller","vanilje","kammerjunkere"] },
  { navn"Rugbrødsmad med leverpostej", ingredienser["rugbrød","leverpostej","agurk eller rødbede"] },
  { navn"Bagte kartofler", ingredienser["kartofler","smør","salt","peber"] },
  { navn"Æggekage", ingredienser["æg","mælk","bacon","purløg","salt"] },
  { navn"Omelet med rester", ingredienser["æg","mælk","ost","grøntsager","salt"] },
  { navn"Pastarester med ketchup", ingredienser["pasta","ketchup","salt"] },
  { navn"Toast med ost og skinke", ingredienser["brød","ost","skinke","smør"] },
  { navn"Rugbrødspizza", ingredienser["rugbrød","tomatsovs","ost","peberfrugt"] },
  { navn"Kartoffelomelet", ingredienser["æg","kartofler","løg","salt","olie"] },
  { navn"Yoghurt med granola", ingredienser["yoghurt","banan","granola"] },
  { navn"Grøntsagssuppe af rester", ingredienser["gulerod","kartoffel","løg","bouillon","vand"] },
  { navn"Kold pasta med mayo", ingredienser["pasta","mayonnaise","majs","ærter"] },
  { navn"Tortilla med ost", ingredienser["tortilla","ost","smør","krydderier"] },
  { navn"Æblegrød", ingredienser["æble","sukker","vand","kanel"] },
  { navn"Bagte gulerødder", ingredienser["gulerod","olie","salt","peber"[Du kan også tage rosmarin, basilikum eller oregano på hvis du vil have mere smag] },
  { navn"Røræg", ingredienser["æg","mælk","smør","salt"] },
  { navn"Mælk og havregryn", ingredienser["havregryn","mælk"] },
  { navn"Toast med peanutbutter og banan", ingredienser["brød","peanutbutter","banan"] },
  { navn"Grillet ostesandwich", ingredienser["brød","ost","smør"] },
  { navn"Mini-pizza af toastbrød", ingredienser["brød","tomatsovs","ost","skinke"] },
  { navn"Kold kartoffelsalat", ingredienser["kartofler","mayonnaise","salt","purløg"] },
  { navn"Bagte æbler", ingredienser["æble","smør","kanel"] },
  { navn"Rester-wrap", ingredienser["tortilla","kylling","salat","dressing"] },
  { navn"Pasta med smeltet ost", ingredienser["pasta","ost","salt"] },
  { navn"Tunmayo sandwich", ingredienser["tun","mayonnaise","brød","citron"] },
  { navn"Spejlæg på rugbrød", ingredienser["æg","smør","rugbrød","salt"] },
  { navn"Resterisotto", ingredienser["ris","ost","smør","bouillon"] },
  { navn"Koldskål med frugt", ingredienser["kærnemælk","æg","vanilje","frugt"] },
  { navn"Æggekage i ovn", ingredienser["æg","mælk","ost","bacon"] },
  { navn"Grøntsagsfringre", ingredienser["gulerod eller andre rodfrugter","kartoffel","olie","salt"] },
  { navn"Nudler med grøntsager", ingredienser["nudler","sojasovs","grøntsager","æg"] },
  { navn"Bagt toast med tomat", ingredienser["brød","tomat","ost","oregano"] },
  { navn"Pølsehorn med rester", ingredienser["brød","pølse","ost","ketchup"] },
  { navn"Wrap med æg og salat", ingredienser["æg","tortilla","salat","dressing"] },
  { navn"Bagte rester i fad", ingredienser["grøntsager","ost","æg","fløde"] },
  { navn"Yoghurtis", ingredienser["yoghurt","honning","frosne bær"] },
  { navn"Brændende kærlighed", ingredienser["kartofler","bacon","løg","smør"] },
  { navn"Tunsalat", ingredienser["tun","majs","løg","mayonnaise"] },
  { navn"Pasta med ketchup og ost", ingredienser["pasta","ketchup","ost"] },
  { navn"French toast", ingredienser["æg","mælk","brød","sukker"] },
  { navn"Bagte æg i muffinsform", ingredienser["æg","ost","bacon","peber"] },
  { navn"Restergryde", ingredienser["grøntsager","ris","tomatsovs","krydderier"] },
  { navn"Suppe med nudler", ingredienser["nudler","bouillon","grøntsager","sojasovs"] },
  { navn"Bagt havregrød", ingredienser["havregryn","mælk","banan","honning"] },
  { navn"Mælkeshake", ingredienser["mælk","banan","bær","isterninger"] },
  { navn"Pølseomelet", ingredienser["æg","pølse","løg","smør"] },
  { navn"Kold pastasalat", ingredienser["pasta","majs","ærter","skinke","dressing"] },
  { navn"Grøntsagspandekager", ingredienser["mel","æg","mælk","gulerod eller andre grøntsager"] },
  { navn"Pizzasnegle af rester", ingredienser["dej","tomatsovs","ost","skinke"] },
  { navn"Rugbrødsmad deluxe", ingredienser["rugbrød","ost","tomat","pesto"] },
  { navn"Ost på brød", ingredienser["brød","ost"] },
  { navn"Smør på brød", ingredienser["brød","smør"] },
  { navn"Æble med kanel", ingredientser["æble","kanel"] },
  { navn"Bananis", ingredienser["banan","yoghurt"] },
  { navn"Æg med salt", ingredienser["æg","salt"] },
  { navn"Æg med peber", ingredienser["æg","peber"] },
  { navn"Yoghurt med honning", ingredienser["yoghurt","honning"] },
  { navn"Grillet tomat med ost", ingredienser["tomat","ost"] },
  { navn"Bagt kartoffel med smør", ingredienser["kartofler","smør"] },
  { navn"Pølse med brød", ingredienser["pølse","brød"] },
  { navn"Yoghurt med banan", ingredienser["banan","yoghurt"] },
  { navn"Banan med kanel", ingredienser["banan","kanel"] },
  { navn"Pølse med ost", ingredienser["pølse","ost"] },
  { navn"Skinke med ost", ingredienser["skinke","ost"] },
  { navn"Kartoffel med ost", ingredienser["kartofler","ost"] },
  { navn"Brød med ost og smør", ingredienser["brød","ost"] },
  { navn"Rugbrød med ost og smør", ingredienser["rugbrød","ost"] },
  { navn"Brød med kanel og smør", ingredienser["brød","kanel"] },
  { navn"Æg med sesamfrø og salt", ingredienser["æg","sesamfrø"] },
  { navn"Yoghurt med kakao og kanel", ingredienser["yoghurt","kanel og kakao"] },
  { navn"Banan med peanutbutter", ingredienser["banan","peanutbutter"] },
  { navn"Ristet rugbrød med avocado", ingredienser["rugbrød","avocado","salt"] },
  { navn"Yoghurt med honning", ingredienser["yoghurt","honning"] },
  { navn"Æggetoast", ingredienser["æg","brød","smør"] },
  { navn"Tomatskiver med mozzarella", ingredienser["tomat","mozzarella","olivenolie"] },
  { navn"Kold pasta med pesto", ingredienser["pasta","pesto"] },
  { navn"Grøntsagsomelet", ingredienser["æg","grøntsager","ost"] },
  { navn"Bagte kartoffelbåde", ingredienser["kartofler","olie","salt"] },
  { navn"Agurkesalat", ingredienser["agurk","eddike"] },
  { navn"Rugbrød med leverpostej", ingredienser["rugbrød","leverpostej"] },
  { navn"Tun med majs", ingredienser["tun","majs","mayonnaise"] },
  { navn"Wrap med ost og skinke", ingredienser["tortilla","ost","skinke"] },
  { navn"Græsk yoghurt med bær", ingredienser["græsk yoghurt","bær","honning"] },
  { navn"Kyllingesalat", ingredienser["kylling","mayonnaise","salat"] },
  { navn"Stegt æg på rugbrød", ingredienser["æg","rugbrød","smør"] },
  { navn"Tomatsuppe", ingredienser["tomat","fløde","løg"] },
  { navn"Græskarsuppe", ingredienser["græskar","fløde","løg"] },
  { navn"Skyr med müsli", ingredienser["skyr","müsli"] }
  { navn"Bagte æg i peberfrugt", ingredienser["æg","peberfrugt"] },
  { navn"Kikærtesalat", ingredienser["kikærter","tomat","citron"] },
  { navn"Æblesnacks", ingredienser["æble","peanutbutter og kakao"] },
  { navn"Gulerodssnack", ingredienser["gulerod","dip"] },
  { navn"Kartoffelmos", ingredienser["kartofler","smør og salt","mælk"] },
  { navn"Tunpasta", ingredienser["tun","pasta","mayonnaise og majs"] },
  { navn"Toast med banan", ingredienser["brød","banan"] },
  { navn"Kold kakao", ingredienser["mælk","kakaopulver"] },
  { navn"Æg med bacon", ingredienser["æg","bacon"] },
  { navn"Smoothie med banan og mælk", ingredienser["banan","mælk"] },
  { navn"Avocadomad", ingredienser["avocado","rugbrød"] },
  { navn"Mini æggepizza", ingredienser["æg","ost","tomat"] },
  { navn"Bagt tomat med ost", ingredienser["tomat","ost"] },
  { navn"Agurkestænger med dip", ingredienser["agurk","yoghurt"] },
  { navn"Bagte champignoner", ingredienser["champignoner","smør","salt og andre valgfrie krydderier"] },
  { navn"Frugtsalat", ingredienser["æble","banan","yoghurt"] },
  { navn"Wrap med æg og salat", ingredienser["tortilla","æg","salat"] },
  { navn"Røræg med ost", ingredienser["æg","ost","smør"] },
  { navn"Tomatsandwich", ingredienser["brød","tomat","krydderier"] },
  { navn"Kylling på rugbrød", ingredienser["kylling","rugbrød","mayonnaise"] },
  { navn"Pære med yoghurt", ingredienser["pære","yoghurt"] },
  { navn"Rugbrød med avocado og æg", ingredienser["rugbrød","avocado","æg"] },
  { navn"Skinkeomelet", ingredienser["æg","skinke"] },
  { navn"Bagt kartoffel med ost", ingredienser["kartoffel","ost"] },
  { navn"Toast med æg", ingredienser["brød","æg"] },
  { navn"Tunmad", ingredienser["rugbrød","tun","mayonnaise"] },
  { navn"Pasta med majs", ingredienser["pasta","majs"] },
  { navn"Karryris", ingredienser["ris","karry","salt og peber"] },
  { navn"Kylling i karry", ingredienser["kylling","karry","fløde"] },
  { navn"Kold pastasalat", ingredienser["pasta","ærter","mayonnaise"] },
  { navn"Grøntsagswrap", ingredienser["tortilla","salat","tomat"] },
  { navn"Mælkesmoothie", ingredienser["mælk","frugt"] },
  { navn"Bagte ærter", ingredienser["ærter","olie","salt og rosmarin"] },
  { navn"Tomat med krydderier", ingredienser["tomat","salt og valgfrie krydderier"] },
  { navn"Kold pasta med æg", ingredienser["pasta","æg","mayonnaise og majs"] },
  { navn"Rugbrød med æg", ingredienser["rugbrød","æg","smør"] },
  { navn"Gulerod og hummus", ingredienser["gulerod","hummus"] },
  { navn"Bagte løg", ingredienser["løg","olie","salt"] },
  { navn"Bagt kartoffelsnack", ingredienser["kartofler","salt"] },
  { navn"Toast med syltetøj", ingredienser["brød","syltetøj"] },
  { navn"Rugbrød med ost", ingredienser["rugbrød","ost"] },
  { navn"Kold risengrød med mælk og kanel", ingredienser["risengrød med kanel","mælk"] },
  { navn"Omelet med løg", ingredienser["æg","løg"] },
  { navn"Bagt pølse", ingredienser["pølse","smør"] },
  { navn"Frisk tomatsalat", ingredienser["tomat","løg","olie og oregano"] },
  { navn"Rugbrød med makrel", ingredienser["rugbrød","makrel"] },
  { navn"Skyr med peanutbutter og blåbær", ingredienser["skyr","peanutbutter og blåbær"] },
  { navn"Kyllingesandwich", ingredienser["brød","kylling","salat"] },
  { navn"Æg og spinat", ingredienser["æg","spinat og peber"] },
  { navn"Ris med grønt", ingredienser["ris","valgfrie grøntsager"] },
];

// Get elements
 ingredientsInput = document.getElementById('ingredients');
 findBtn = document.getElementById('findBtn');
 recipeList = document.getElementById('recipeList');
 findRecipes() {
  userIngredients = ingredientsInput.value
    .toLowerCase()
    .split(',')
    .map(i  i.trim())
    .filter(i => i);

  recipeList.innerHTML = '';

   (userIngredients.length  0) {
    recipeList.innerHTML  '<li>Skriv mindst én ingrediens.</li>';
    ;
  }

  // 🔍 Filtrér kun opskrifter hvor man har mindst en af ingredienserne
  const possibleRecipes  recipes.filter(recipe 
    recipe.ingredients.every(i  userIngredients.includes(i))
  );

  // 📭 Hvis ingen opskrifter matcher
  if (possibleRecipes.length === 0) {
    recipeList.innerHTML = '<li>Desværre får du ingen mad i dag med de ingredienser:( </li>';
    return;
  }

  // 🍳 Vis alle opskrifter du kan lave
  possibleRecipes.forEach(recipe => {
    const li  document.createElement('li');
    li.className  'ready';
    li.innerHTML  `<strong>${recipe.name}</strong><br>
      <em>Ingredienser:</em> ${recipe.ingredients.join(', ')}<br>
      <em>Er denne til din smag?:) </em>`;
    recipeList.appendChild(li);
  });
}


// Event listener
findBtn.addEventListener('click', findRecipes);
</script>
</body>
</html>
