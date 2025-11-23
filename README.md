<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title> Dit lokale tomme køkken! </title> (Det her er en prototype, ikke den rigtige app)
<style>
  body { font-family: Cursive, sans-serif; background:#a9c7ee; margin:0; padding:20px; }
  h1 { text-align:center; color:#29353C; }
  .container { max-width:700px; margin:0 auto; background:#FFE8E1; padding:20px; border-radius:12px; box-shadow:0 5px 15px rgba(0,0,0,0.1);}
  label { font-weight:bold; display:block; margin-bottom:8px; }
  input, button, select { padding:8px; font-size:14px; border-radius:6px; border:1px solid #ccc; margin-bottom:12px; width:100%; }
  button { background:#768A96; color:white; border:none; cursor:pointer; }
  button:hover { background:#daf0ff; }
  ul { list-style:none; padding:0; }
  li { padding:10px; border-bottom:1px solid #eee; }
  .missing { color:#F3E4F5; }
  .ready { color:#A7ABDE0; }
</style>
</head>
<body>
<div class="container">
  <h1>Dit lokale tomme køkken! </h1>
  <label for="ingredients">Indtast de ingredienser du vil have med i din kreative ret!:</label>
  <input type="text" id="ingredients" placeholder="fx. æg, mælk, tomater, ost">
  <button id="findBtn">Find Opskrifter</button>
  <h2>Matchende Opskrifter:</h2>
  <ul id="recipeList"></ul>
</div>

<script>
// Sample recipe database
const recipes = [
  { navn:"Tomatpasta", ingredienser:["tomat","pasta","olivenolie","hvidløg","salt"] },
  { navn:"Ostomelet", ingredients:["æg","mælk","ost","salt","peber"] },
  { navn:"Grillet ostesandwich", ingredients:["brød","ost","smør"] },
  { navn:"Surdejsbrød", ingredients:["mel","vand","salt"] },
  { navn:"Simpel salat", ingredients:["salat","tomat","agurk","olivenolie","salt"] },
  { navn:"Kartoffelmos", ingredients:["kartofler","smør","mælk","salt","peber"] },
  { navn:"Kyllingesalat", ingredients:["kylling","salat","majs","agurk","dressing"] },
  { navn:"Røræg", ingredients:["æg","mælk","smør","salt","peber"] },
  { navn:"Havreboller", ingredients:["havregryn","vand","mel","salt"] },
  { navn:"Frugtsalat", ingredients:["æble","banan","appelsin","pære","melon"] },
  { navn:"Tunsalat", ingredients:["tun","majs","løg","mayonnaise","citron"] },
  { navn:"Karrysuppe", ingredients:["kylling","løg","gulerod","karry","fløde"] },
  { navn:"Lasagne", ingredients:["lasagneplader","spuash","tomatsovs","ost","løg"] },
  { navn:"Boller", ingredients:["mel","gær","vand","salt","smør"] },
  { navn:"Pizzadej", ingredients:["mel","gær","vand","olivenolie","salt"] },
  { navn:"Pizza Margherita", ingredients:["pizzadej","tomatsovs","ost","basilikum"] },
  { navn:"Smoothie", ingredients:["banan","jordbær","blåbær","isterninger"] },
  { navn:"Græsk salat", ingredients:["feta","tomat","agurk","oliven","løg"] },
  { navn:"Stegt ris", ingredients:["ris","æg","majs","ærter","salt"] },
  { navn:"Pastasalat", ingredients:["pasta","majs","skinke","ærter","æg"] },
  { navn:"Kylling i karry", ingredients:["kylling","karry","løg","fløde","ris"] },
  { navn:"Stegt æbletærte", ingredients:["æble","kanel","smør","mel","salt"] },
  { navn:"Kakao Dadelboller", ingredients:["mel","dadler","æg","kakao","smør"] },
  { navn:"Kanelbidder", ingredients:["smør","kanel","æg","mel"] },
  { navn:"Kakao og bananbrød", ingredients:["banan","mel","æg","kakao","smør"] },
  { navn:"Tomatsuppe", ingredients:["tomat","løg","hvidløg","bouillon","fløde"] },
  { navn:"Ratatouille", ingredients:["aubergine","squash","tomat","peberfrugt","løg"] },
  { navn:"Kyllingesandwich", ingredients:["brød","kylling","salat","tomat","mayonnaise"] },
  { navn:"Linsesuppe", ingredients:["linser","løg","gulerod","hvidløg","bouillon"] },
  { navn:"Spaghetti sorte bønner", ingredients:["sorte bønner","tomat","spaghetti","løg","hvidløg"] },
  { navn:"Fiskefrikadeller", ingredients:["fisk","æg","mel","salt","peber"] },
  { navn:"Kartoffelsalat", ingredients:["kartofler","mayonnaise","sennep","purløg"] },
  { navn:"Æggemadder", ingredients:["æg","rugbrød","smør","salt"] },
  { navn:"Hotdog", ingredients:["pølse","brød","sennep","ketchup","ristet løg"] },
  { navn:"Burger", ingredients:["kidneybønner","bolle","ost","salat","tomat"] },
  { navn:"Falafel", ingredients:["kikærter","løg","hvidløg","persille","mel"] },
  { navn:"Hummus", ingredients:["kikærter","tahini","citron","hvidløg","olivenolie"] },
  { navn:"Tzatziki", ingredients:["yoghurt","agurk","hvidløg","citron","salt"] },
  { navn:"Rugbrødsmad med laks", ingredients:["rugbrød","røget laks","flødeost","dild","citron"] },
  { navn:"Taco", ingredients:["tacobrød","kikærter","tomat","salat","ost"] },
  { navn:"Wraps", ingredients:["tortilla","kylling","salat","agurk","dressing"] },
  { navn:"Blåbær muffins", ingredients:["mel","blåbær","æg","smør","bagepulver"] },
  { navn:"Bananpandekager", ingredients:["banan","æg","mel","mælk"] },
  { navn:"Smoothie bowl", ingredients:["banan","jordbær","yoghurt","blåbær"] },
  { navn:"Toast", ingredients:["brød","ost","skinke","smør"] },
  { navn:"Pasta med pesto", ingredients:["pasta","pesto","parmesan","olivenolie"] },
  { navn:"Koldskål", ingredients:["kærnemælk","æg","citronsaft eller skræller","vanilje","kammerjunkere"] },
  { navn:"Rugbrødsmad med leverpostej", ingredients:["rugbrød","leverpostej","agurk eller rødbede"] },
  { navn:"Bagte kartofler", ingredients:["kartofler","smør","salt","peber"] },
  { navn:"Æggekage", ingredients:["æg","mælk","bacon","purløg","salt"] },
  { navn:"Omelet med rester", ingredients:["æg","mælk","ost","grøntsager","salt"] },
  { navn:"Pastarester med ketchup", ingredients:["pasta","ketchup","salt"] },
  { navn:"Toast med ost og skinke", ingredients:["brød","ost","skinke","smør"] },
  { navn:"Rugbrødspizza", ingredients:["rugbrød","tomatsovs","ost","peberfrugt"] },
  { navn:"Kartoffelomelet", ingredients:["æg","kartofler","løg","salt","olie"] },
  { navn:"Yoghurt med granola", ingredients:["yoghurt","banan","granola"] },
  { navn:"Grøntsagssuppe af rester", ingredients:["gulerod","kartoffel","løg","bouillon","vand"] },
  { navn:"Kold pasta med mayo", ingredients:["pasta","mayonnaise","majs","ærter"] },
  { navn:"Tortilla med ost", ingredients:["tortilla","ost","smør","krydderier"] },
  { navn:"Æblegrød", ingredients:["æble","sukker","vand","kanel"] },
  { navn:"Bagte gulerødder", ingredients:["gulerod","olie","salt","peber"[Du kan også tage rosmarin, basilikum eller oregano på hvis du vil have mere smag]] },
  { navn:"Røræg", ingredients:["æg","mælk","smør","salt"] },
  { navn:"Mælk og havregryn", ingredients:["havregryn","mælk"] },
  { navn:"Toast med peanutbutter og banan", ingredients:["brød","peanutbutter","banan"] },
  { navn:"Grillet ostesandwich", ingredients:["brød","ost","smør"] },
  { navn:"Mini-pizza af toastbrød", ingredients:["brød","tomatsovs","ost","skinke"] },
  { navn:"Kold kartoffelsalat", ingredients:["kartofler","mayonnaise","salt","purløg"] },
  { navn:"Bagte æbler", ingredients:["æble","smør","kanel"] },
  { navn:"Rester-wrap", ingredients:["tortilla","kylling","salat","dressing"] },
  { navn:"Pasta med smeltet ost", ingredients:["pasta","ost","salt"] },
  { navn:"Tunmayo sandwich", ingredients:["tun","mayonnaise","brød","citron"] },
  { navn:"Spejlæg på rugbrød", ingredients:["æg","smør","rugbrød","salt"] },
  { navn:"Resterisotto", ingredients:["ris","ost","smør","bouillon"] },
  { navn:"Koldskål med frugt", ingredients:["kærnemælk","æg","vanilje","frugt"] },
  { navn:"Æggekage i ovn", ingredients:["æg","mælk","ost","bacon"] },
  { navn:"Grøntsagsfringre", ingredients:["gulerod eller andre rodfrugter","kartoffel","olie","salt"] },
  { navn:"Nudler med grøntsager", ingredients:["nudler","sojasovs","grøntsager","æg"] },
  { navn:"Bagt toast med tomat", ingredients:["brød","tomat","ost","oregano"] },
  { navn:"Pølsehorn med rester", ingredients:["brød","pølse","ost","ketchup"] },
  { navn:"Wrap med æg og salat", ingredients:["æg","tortilla","salat","dressing"] },
  { navn:"Bagte rester i fad", ingredients:["grøntsager","ost","æg","fløde"] },
  { navn:"Yoghurtis", ingredients:["yoghurt","honning","frosne bær"] },
  { navn:"Brændende kærlighed", ingredients:["kartofler","bacon","løg","smør"] },
  { navn:"Tunsalat", ingredients:["tun","majs","løg","mayonnaise"] },
  { navn:"Pasta med ketchup og ost", ingredients:["pasta","ketchup","ost"] },
  { navn:"French toast", ingredients:["æg","mælk","brød","sukker"] },
  { navn:"Bagte æg i muffinsform", ingredients:["æg","ost","bacon","peber"] },
  { navn:"Restergryde", ingredients:["grøntsager","ris","tomatsovs","krydderier"] },
  { navn:"Suppe med nudler", ingredients:["nudler","bouillon","grøntsager","sojasovs"] },
  { navn:"Bagt havregrød", ingredients:["havregryn","mælk","banan","honning"] },
  { navn:"Mælkeshake", ingredients:["mælk","banan","bær","isterninger"] },
  { navn:"Pølseomelet", ingredients:["æg","pølse","løg","smør"] },
  { navn:"Kold pastasalat", ingredients:["pasta","majs","ærter","skinke","dressing"] },
  { navn:"Grøntsagspandekager", ingredients:["mel","æg","mælk","gulerod eller andre grøntsager"] },
  { navn:"Pizzasnegle af rester", ingredients:["dej","tomatsovs","ost","skinke"] },
  { navn:"Rugbrødsmad deluxe", ingredients:["rugbrød","ost","tomat","pesto"] },
  { navn:"Ost på brød", ingredients:["brød","ost"] },
  { navn:"Smør på brød", ingredients:["brød","smør"] },
  { navn:"Æble med kanel", ingredients:["æble","kanel"] },
  { navn:"Bananis", ingredients:["banan","yoghurt"] },
  { navn:"Æg med salt", ingredients:["æg","salt"] },
  { navn:"Æg med peber", ingredients:["æg","peber"] },
  { navn:"Yoghurt med honning", ingredients:["yoghurt","honning"] },
  { navn:"Grillet tomat med ost", ingredients:["tomat","ost"] },
  { navn:"Bagt kartoffel med smør", ingredients:["kartofler","smør"] },
  { navn:"Pølse med brød", ingredients:["pølse","brød"] },
  { navn:"Yoghurt med banan", ingredients:["banan","yoghurt"] },
  { navn:"Banan med kanel", ingredients:["banan","kanel"] },
  { navn:"Pølse med ost", ingredients:["pølse","ost"] },
  { navn:"Skinke med ost", ingredients:["skinke","ost"] },
  { navn:"Kartoffel med ost", ingredients:["kartofler","ost"] },
  { navn:"Brød med ost og smør", ingredients:["brød","ost"] },
  { navn:"Rugbrød med ost og smør", ingredients:["rugbrød","ost"] },
  { navn:"Brød med kanel og smør", ingredients:["brød","kanel"] },
  { navn:"Æg med sesamfrø og salt", ingredients:["æg","sesamfrø"] },
  { navn:"Yoghurt med kakao og kanel", ingredients:["yoghurt","kanel og kakao"] },
  { navn:"Banan med peanutbutter", ingredients:["banan","peanutbutter"] },
  { navn:"Ristet rugbrød med avocado", ingredients:["rugbrød","avocado","salt"] },
  { navn:"Yoghurt med honning", ingredients:["yoghurt","honning"] },
  { navn:"Æggetoast", ingredients:["æg","brød","smør"] },
  { navn:"Tomatskiver med mozzarella", ingredients:["tomat","mozzarella","olivenolie"] },
  { navn:"Kold pasta med pesto", ingredients:["pasta","pesto"] },
  { navn:"Grøntsagsomelet", ingredients:["æg","grøntsager","ost"] },
  { navn:"Bagte kartoffelbåde", ingredients:["kartofler","olie","salt"] },
  { navn:"Agurkesalat", ingredients:["agurk","eddike"] },
  { navn:"Rugbrød med leverpostej", ingredients:["rugbrød","leverpostej"] },
  { navn:"Tun med majs", ingredients:["tun","majs","mayonnaise"] },
  { navn:"Wrap med ost og skinke", ingredients:["tortilla","ost","skinke"] },
  { navn:"Græsk yoghurt med bær", ingredients:["græsk yoghurt","bær","honning"] },
  { navn:"Kyllingesalat", ingredients:["kylling","mayonnaise","salat"] },
  { navn:"Stegt æg på rugbrød", ingredients:["æg","rugbrød","smør"] },
  { navn:"Tomatsuppe", ingredients:["tomat","fløde","løg"] },
  { navn:"Græskarsuppe", ingredients:["græskar","fløde","løg"] },
  { navn:"Skyr med müsli", ingredients:["skyr","müsli"] }
  { navn:"Bagte æg i peberfrugt", ingredients:["æg","peberfrugt"] },
  { navn:"Kikærtesalat", ingredients:["kikærter","tomat","citron"] },
  { navn:"Æblesnacks", ingredients:["æble","peanutbutter og kakao"] },
  { navn:"Gulerodssnack", ingredients:["gulerod","dip"] },
  { navn:"Kartoffelmos", ingredients:["kartofler","smør og salt","mælk"] },
  { navn:"Tunpasta", ingredients:["tun","pasta","mayonnaise og majs"] },
  { navn:"Toast med banan", ingredients:["brød","banan"] },
  { navn:"Kold kakao", ingredients:["mælk","kakaopulver"] },
  { navn:"Æg med bacon", ingredients:["æg","bacon"] },
  { navn:"Smoothie med banan og mælk", ingredients:["banan","mælk"] },
  { navn:"Avocadomad", ingredients:["avocado","rugbrød"] },
  { navn:"Mini æggepizza", ingredients:["æg","ost","tomat"] },
  { navn:"Bagt tomat med ost", ingredients:["tomat","ost"] },
  { navn:"Agurkestænger med dip", ingredients:["agurk","yoghurt"] },
  { navn:"Bagte champignoner", ingredients:["champignoner","smør","salt og andre valgfrie krydderier"] },
  { navn:"Frugtsalat", ingredients:["æble","banan","yoghurt"] },
  { navn:"Wrap med æg og salat", ingredients:["tortilla","æg","salat"] },
  { navn:"Røræg med ost", ingredients:["æg","ost","smør"] },
  { navn:"Tomatsandwich", ingredients:["brød","tomat","krydderier"] },
  { navn:"Kylling på rugbrød", ingredients:["kylling","rugbrød","mayonnaise"] },
  { navn:"Pære med yoghurt", ingredients:["pære","yoghurt"] },
  { navn:"Rugbrød med avocado og æg", ingredients:["rugbrød","avocado","æg"] },
  { navn:"Skinkeomelet", ingredients:["æg","skinke"] },
  { navn:"Bagt kartoffel med ost", ingredients:["kartoffel","ost"] },
  { navn:"Toast med æg", ingredients:["brød","æg"] },
  { navn:"Tunmad", ingredients:["rugbrød","tun","mayonnaise"] },
  { navn:"Pasta med majs", ingredients:["pasta","majs"] },
  { navn:"Karryris", ingredients:["ris","karry","salt og peber"] },
  { navn:"Kylling i karry", ingredients:["kylling","karry","fløde"] },
  { navn:"Kold pastasalat", ingredients:["pasta","ærter","mayonnaise"] },
  { navn:"Grøntsagswrap", ingredients:["tortilla","salat","tomat"] },
  { navn:"Mælkesmoothie", ingredients:["mælk","frugt"] },
  { navn:"Bagte ærter", ingredients:["ærter","olie","salt og rosmarin"] },
  { navn:"Tomat med krydderier", ingredients:["tomat","salt og valgfrie krydderier"] },
  { navn:"Kold pasta med æg", ingredients:["pasta","æg","mayonnaise og majs"] },
  { navn:"Rugbrød med æg", ingredients:["rugbrød","æg","smør"] },
  { navn:"Gulerod og hummus", ingredients:["gulerod","hummus"] },
  { navn:"Bagte løg", ingredients:["løg","olie","salt"] },
  { navn:"Bagt kartoffelsnack", ingredients:["kartofler","salt"] },
  { navn:"Toast med syltetøj", ingredients:["brød","syltetøj"] },
  { navn:"Rugbrød med ost", ingredients:["rugbrød","ost"] },
  { navn:"Kold risengrød med mælk og kanel", ingredients:["risengrød med kanel","mælk"] },
  { navn:"Omelet med løg", ingredients:["æg","løg"] },
  { navn:"Bagt pølse", ingredients:["pølse","smør"] },
  { navn:"Frisk tomatsalat", ingredients:["tomat","løg","olie og oregano"] },
  { navn:"Rugbrød med makrel", ingredients:["rugbrød","makrel"] },
  { navn:"Skyr med peanutbutter og blåbær", ingredients:["skyr","peanutbutter og blåbær"] },
  { navn:"Kyllingesandwich", ingredients:["brød","kylling","salat"] },
  { navn:"Æg og spinat", ingredients:["æg","spinat og peber"] },
  { navn:"Ris med grønt", ingredients:["ris","valgfrie grøntsager"] },
];

// Get elements
const ingredientsInput = document.getElementById('ingredients');
const findBtn = document.getElementById('findBtn');
const recipeList = document.getElementById('recipeList');
function findRecipes() {
  const userIngredients = ingredientsInput.value
    .toLowerCase()
    .split(',')
    .map(i => i.trim())
    .filter(i => i);

  recipeList.innerHTML = '';

  if (userIngredients.length === 0) {
    recipeList.innerHTML = '<li>Skriv mindst én ingrediens.</li>';
    return;
  }

  // 🔍 Filtrér kun opskrifter hvor man har mindst en af ingredienserne
  const possibleRecipes = recipes.filter(recipe =>
    recipe.ingredients.every(i => userIngredients.includes(i))
  );

  // 📭 Hvis ingen opskrifter matcher
  if (possibleRecipes.length === 0) {
    recipeList.innerHTML = '<li>Desværre får du ingen mad i dag med de ingredienser</li>';
    return;
  }

  // 🍳 Vis alle opskrifter du kan lave
  possibleRecipes.forEach(recipe => {
    const li = document.createElement('li');
    li.className = 'ready';
    li.innerHTML = `<strong>${recipe.name}</strong><br>
      <em>Ingredienser:</em> ${recipe.ingredients.join(', ')}<br>
      <em>Er dette til din smag?</em>`;
    recipeList.appendChild(li);
  });
}


// Event listener
findBtn.addEventListener('click', findRecipes);
</script>
</body>
</html>
