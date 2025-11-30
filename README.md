<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Det tomme køkken</title>
<!-- Det her er en prototype, ikke den rigtige app -->
  </style>
  body { font-family: Cursive, sans-serif; background:#a9c7ee; margin:0; padding:20px; }
  h1 { text-align:center; color:#29353C; }
  .container { max-width:700px; margin:0 auto; background:#FFE8E1; padding:20px; border-radius:12px; box-shadow:0 5px 15px rgba(0,0,0,0.1);}
  label { font-weight:bold; display:block; margin-bottom:8px; }
  input, button, select { padding:8px; font-size:14px; border-radius:6px; border:1px solid #ccc; margin-bottom:12px; width:100%; }
  button { background:#768A96; color:white; border:none; cursor:pointer; }
  button:hover { background:#E6E6E6; }
  ul { list-style:none; padding:0; }
  li { padding:10px; border-bottom:1px solid #eee; }
  .missing { color:#F3E4F5; }
  .ready { color:#A7ABDE; }
</head>
<body>
<div class="container">
  <h1> Det tomme køkken </h1>
  <label for="ingredients">Enter ingredients you have (comma separated):</label>
  <input type="text" id="ingredients" placeholder="fx. æg, mælk, tomater, ost">
  <button id="findBtn">Find Recipes</button>
  <h2>Matching Recipes:</h2>
  <ul id="recipeList"></ul>
</div>

<script>
// Sample recipe database
const recipes = [
  { name:"Tomatpasta", ingredients:["tomat","pasta","olivenolie","hvidløg","salt"] },
  { name:"Ostomelet", ingredients:["æg","mælk","ost","salt","peber"] },
  { name:"Grillet ostesandwich", ingredients:["brød","ost","smør"] },
  { name:"Pandekager", ingredients:["mel","mælk","æg","sukker","smør"] },
  { name:"Simpel salat", ingredients:["salat","tomat","agurk","olivenolie","salt"] },
  { name:"Kartoffelmos", ingredients:["kartofler","smør","mælk","salt","peber"] },
  { name:"Kyllingesalat", ingredients:["kylling","salat","majs","agurk","dressing"] },
  { name:"Røræg", ingredients:["æg","mælk","smør","salt","peber"] },
  { name:"Havregrød", ingredients:["havregryn","mælk","sukker","smør"] },
  { name:"Frugtsalat", ingredients:["æble","banan","appelsin","yoghurt","honning"] },
  { name:"Tunsalat", ingredients:["tun","majs","løg","mayonnaise","citron"] },
  { name:"Karrysuppe", ingredients:["kylling","løg","gulerod","karry","fløde"] },
  { name:"Lasagne", ingredients:["lasagneplader","oksekød","tomatsovs","ost","løg"] },
  { name:"Boller", ingredients:["mel","gær","vand","sukker","smør"] },
  { name:"Pizzadej", ingredients:["mel","gær","vand","olivenolie","salt"] },
  { name:"Pizza Margherita", ingredients:["pizzadej","tomatsovs","ost","basilikum"] },
  { name:"Smoothie", ingredients:["banan","jordbær","mælk","honning"] },
  { name:"Græsk salat", ingredients:["feta","tomat","agurk","oliven","løg"] },
  { name:"Stegt ris", ingredients:["ris","æg","sojasovs","ærter","gulerod"] },
  { name:"Pastasalat", ingredients:["pasta","majs","skinke","ærter","creme fraiche"] },
  { name:"Kylling i karry", ingredients:["kylling","karry","løg","fløde","ris"] },
  { name:"Æblekage", ingredients:["æble","sukker","smør","havregryn","kanel"] },
  { name:"Chokoladekage", ingredients:["mel","sukker","æg","kakao","smør"] },
  { name:"Brownies", ingredients:["smør","sukker","æg","mel","kakao"] },
  { name:"Banankage", ingredients:["banan","mel","æg","sukker","smør"] },
  { name:"Tomatsuppe", ingredients:["tomat","løg","hvidløg","bouillon","fløde"] },
  { name:"Ratatouille", ingredients:["aubergine","squash","tomat","peberfrugt","løg"] },
  { name:"Kyllingesandwich", ingredients:["brød","kylling","salat","tomat","mayonnaise"] },
  { name:"Linsesuppe", ingredients:["linser","løg","gulerod","hvidløg","bouillon"] },
  { name:"Spaghetti Bolognese", ingredients:["oksekød","tomat","spaghetti","løg","hvidløg"] },
  { name:"Fiskefrikadeller", ingredients:["fisk","æg","mel","salt","peber"] },
  { name:"Kartoffelsalat", ingredients:["kartofler","mayonnaise","sennep","purløg"] },
  { name:"Æggemadder", ingredients:["æg","rugbrød","smør","salt"] },
  { name:"Hotdog", ingredients:["pølse","brød","sennep","ketchup","ristet løg"] },
  { name:"Burger", ingredients:["oksekød","bolle","ost","salat","tomat"] },
  { name:"Falafel", ingredients:["kikærter","løg","hvidløg","persille","mel"] },
  { name:"Hummus", ingredients:["kikærter","tahini","citron","hvidløg","olivenolie"] },
  { name:"Tzatziki", ingredients:["yoghurt","agurk","hvidløg","citron","salt"] },
  { name:"Rugbrødsmad med laks", ingredients:["rugbrød","røget laks","flødeost","dild","citron"] },
  { name:"Taco", ingredients:["tacobrød","oksekød","tomat","salat","ost"] },
  { name:"Wraps", ingredients:["tortilla","kylling","salat","agurk","dressing"] },
  { name:"Muffins", ingredients:["mel","sukker","æg","smør","bagepulver"] },
  { name:"Bananpandekager", ingredients:["banan","æg","havregryn","mælk"] },
  { name:"Smoothie bowl", ingredients:["banan","jordbær","yoghurt","honning","havregryn"] },
  { name:"Vafler", ingredients:["mel","æg","mælk","sukker","smør"] },
  { name:"Toast", ingredients:["brød","ost","skinke","smør"] },
  { name:"Pasta med pesto", ingredients:["pasta","pesto","parmesan","olivenolie"] },
  { name:"Koldskål", ingredients:["kærnemælk","æg","sukker","vanilje","kammerjunker"] },
  { name:"Rugbrødsmad med leverpostej", ingredients:["rugbrød","leverpostej","agurk"] },
  { name:"Bagte kartofler", ingredients:["kartofler","smør","salt","peber"] },
  { name:"Æggekage", ingredients:["æg","mælk","bacon","purløg","salt"] },
  { name:"Omelet med rester", ingredients:["æg","mælk","ost","grøntsager","salt"] },
  { name:"Pastarester med ketchup", ingredients:["pasta","ketchup","smør","salt"] },
  { name:"Toast med ost og skinke", ingredients:["brød","ost","skinke","smør"] },
  { name:"Rugbrødspizza", ingredients:["rugbrød","tomatsovs","ost","peberfrugt"] },
  { name:"Kartoffelomelet", ingredients:["æg","kartofler","løg","salt","olie"] },
  { name:"Yoghurt med frugt og honning", ingredients:["yoghurt","banan","honning","havregryn"] },
  { name:"Grøntsagssuppe af rester", ingredients:["gulerod","kartoffel","løg","bouillon","vand"] },
  { name:"Æg i kop", ingredients:["æg","smør","salt","peber"] },
  { name:"Stegte ris med æg", ingredients:["ris","æg","sojasovs","ærter","olie"] },
  { name:"Kold pasta med mayo", ingredients:["pasta","mayonnaise","majs","ærter"] },
  { name:"Tortilla med ost", ingredients:["tortilla","ost","smør","krydderier"] },
  { name:"Æblegrød", ingredients:["æble","sukker","vand","kanel"] },
  { name:"Bagte gulerødder", ingredients:["gulerod","olie","salt","peber"] },
  { name:"Røræg", ingredients:["æg","mælk","smør","salt"] },
  { name:"Mælk og havregryn", ingredients:["havregryn","mælk","sukker"] },
  { name:"Toast med peanutbutter og banan", ingredients:["brød","peanutbutter","banan"] },
  { name:"Grillet ostesandwich", ingredients:["brød","ost","smør"] },
  { name:"Mini-pizza af toastbrød", ingredients:["brød","tomatsovs","ost","skinke"] },
  { name:"Kold kartoffelsalat", ingredients:["kartofler","mayonnaise","sennep","purløg"] },
  { name:"Bagte æbler", ingredients:["æble","smør","kanel","sukker"] },
  { name:"Rester-wrap", ingredients:["tortilla","kylling","salat","dressing"] },
  { name:"Pasta med smør og ost", ingredients:["pasta","smør","ost","salt"] },
  { name:"Tunmayo sandwich", ingredients:["tun","mayonnaise","brød","citron"] },
  { name:"Spejlæg på rugbrød", ingredients:["æg","smør","rugbrød","salt"] },
  { name:"Bananpandekager", ingredients:["banan","æg","havregryn"] },
  { name:"Resterisotto", ingredients:["ris","ost","smør","bouillon"] },
  { name:"Koldskål med frugt", ingredients:["kærnemælk","æg","sukker","frugt"] },
  { name:"Æggekage i ovn", ingredients:["æg","mælk","ost","bacon"] },
  { name:"Grøntsagsfritter", ingredients:["gulerod","kartoffel","olie","salt"] },
  { name:"Nudler med grøntsager", ingredients:["nudler","sojasovs","grøntsager","æg"] },
  { name:"Bagt toast med tomat", ingredients:["brød","tomat","ost","oregano"] },
  { name:"Pølsehorn med rester", ingredients:["brød","pølse","ost","ketchup"] },
  { name:"Wrap med æg og salat", ingredients:["æg","tortilla","salat","dressing"] },
  { name:"Bagte rester i fad", ingredients:["grøntsager","ost","æg","fløde"] },
  { name:"Yoghurtis", ingredients:["yoghurt","honning","frosne bær"] },
  { name:"Brændende kærlighed", ingredients:["kartofler","bacon","løg","smør"] },
  { name:"Tunsalat", ingredients:["tun","majs","løg","mayonnaise"] },
  { name:"Pasta med ketchup og ost", ingredients:["pasta","ketchup","ost"] },
  { name:"French toast", ingredients:["æg","mælk","brød","sukker"] },
  { name:"Bagte æg i muffinsform", ingredients:["æg","ost","bacon","peber"] },
  { name:"Restergryde", ingredients:["grøntsager","ris","tomatsovs","krydderier"] },
  { name:"Suppe med nudler", ingredients:["nudler","bouillon","grøntsager","sojasovs"] },
  { name:"Bagt havregrød", ingredients:["havregryn","mælk","banan","honning"] },
  { name:"Mælkeshake", ingredients:["mælk","banan","sukker","is"] },
  { name:"Pølseomelet", ingredients:["æg","pølse","løg","smør"] },
  { name:"Kold pastasalat", ingredients:["pasta","majs","ærter","skinke","dressing"] },
  { name:"Grøntsagspandekager", ingredients:["mel","æg","mælk","gulerod"] },
  { name:"Pizzasnegle af rester", ingredients:["dej","tomatsovs","ost","skinke"] },
  { name:"Æg med tomat og ost", ingredients:["æg","tomat","ost","salt"] },
  { name:"Rugbrødsmad deluxe", ingredients:["rugbrød","ost","tomat","pesto"] },
  { name:"Banan på brød", ingredients:["brød","banan"] },
  { name:"Ost på brød", ingredients:["brød","ost"] },
  { name:"Smør på brød", ingredients:["brød","smør"] },
  { name:"Æble med honning", ingredients:["æble","honning"] },
  { name:"Banan med honning", ingredients:["banan","honning"] },
  { name:"Æg med salt", ingredients:["æg","salt"] },
  { name:"Æg med peber", ingredients:["æg","peber"] },
  { name:"Yoghurt med honning", ingredients:["yoghurt","honning"] },
  { name:"Tomat med ost", ingredients:["tomat","ost"] },
  { name:"Agurk med salt", ingredients:["agurk","salt"] },
  { name:"Kartoffel med smør", ingredients:["kartofler","smør"] },
  { name:"Gulerod med olie", ingredients:["gulerod","olie"] },
  { name:"Ris med olie", ingredients:["ris","olie"] },
  { name:"Pasta med olie", ingredients:["pasta","olie"] },
  { name:"Brød med peanutbutter", ingredients:["brød","peanutbutter"] },
  { name:"Rugbrød med ost", ingredients:["rugbrød","ost"] },
  { name:"Æble med kanel", ingredients:["æble","kanel"] },
  { name:"Banan med smør", ingredients:["banan","smør"] },
  { name:"Tomat med olie", ingredients:["tomat","olie"] },
  { name:"Agurk med olie", ingredients:["agurk","olie"] },
  { name:"Pølse med brød", ingredients:["pølse","brød"] },
  { name:"Skinke med brød", ingredients:["skinke","brød"] },
  { name:"Koldskål med sukker", ingredients:["koldskål","sukker"] },
  { name:"Havregryn med mælk", ingredients:["havregryn","mælk"] },
  { name:"Banan med yoghurt", ingredients:["banan","yoghurt"] },
  { name:"Æble med yoghurt", ingredients:["æble","yoghurt"] },
  { name:"Smør med ost", ingredients:["smør","ost"] },
  { name:"Kartoffel med salt", ingredients:["kartofler","salt"] },
  { name:"Gulerod med salt", ingredients:["gulerod","salt"] },
  { name:"Tomat med salt", ingredients:["tomat","salt"] },
  { name:"Agurk med peber", ingredients:["agurk","peber"] },
  { name:"Rugbrød med smør", ingredients:["rugbrød","smør"] },
  { name:"Pasta med ost", ingredients:["pasta","ost"] },
  { name:"Ris med smør", ingredients:["ris","smør"] },
  { name:"Brød med ost", ingredients:["brød","ost"] },
  { name:"Æg med olie", ingredients:["æg","olie"] },
  { name:"Banan med peber", ingredients:["banan","peber"] },
  { name:"Æble med peber", ingredients:["æble","peber"] },
  { name:"Tomat med peber", ingredients:["tomat","peber"] },
  { name:"Kartoffel med peber", ingredients:["kartofler","peber"] },
  { name:"Gulerod med honning", ingredients:["gulerod","honning"] },
  { name:"Ris med salt", ingredients:["ris","salt"] },
  { name:"Pasta med salt", ingredients:["pasta","salt"] },
  { name:"Yoghurt med sukker", ingredients:["yoghurt","sukker"] },
  { name:"Æg med honning", ingredients:["æg","honning"] },
  { name:"Brød med honning", ingredients:["brød","honning"] },
  { name:"Rugbrød med honning", ingredients:["rugbrød","honning"] },
  { name:"Banan med kanel", ingredients:["banan","kanel"] },
  { name:"Æble med smør", ingredients:["æble","smør"] },
  { name:"Tomat med honning", ingredients:["tomat","honning"] },
  { name:"Agurk med honning", ingredients:["agurk","honning"] },
  { name:"Pølse med ost", ingredients:["pølse","ost"] },
  { name:"Skinke med ost", ingredients:["skinke","ost"] },
  { name:"Kartoffel med peber", ingredients:["kartofler","peber"] },
  { name:"Gulerod med peber", ingredients:["gulerod","peber"] },
  { name:"Brød med kanel", ingredients:["brød","kanel"] },
  { name:"Rugbrød med kanel", ingredients:["rugbrød","kanel"] },
  { name:"Æg med kanel", ingredients:["æg","kanel"] },
  { name:"Yoghurt med kanel", ingredients:["yoghurt","kanel"] },
  { name:"Ris med peber", ingredients:["ris","peber"] },
  { name:"Pasta med peber", ingredients:["pasta","peber"] },
  { name:"Kartoffel med olie", ingredients:["kartofler","olie"] },
  { name:"Gulerod med smør", ingredients:["gulerod","smør"] },
  { name:"Brød med olie", ingredients:["brød","olie"] },
  { name:"Rugbrød med olie", ingredients:["rugbrød","olie"] },
  { name:"Æble med olie", ingredients:["æble","olie"] },
  { name:"Banan med olie", ingredients:["banan","olie"] },
  { name:"Tomat med smør", ingredients:["tomat","smør"] },
  { name:"Agurk med smør", ingredients:["agurk","smør"] },
  { name:"Æg med ost", ingredients:["æg","ost"] },
  { name:"Yoghurt med smør", ingredients:["yoghurt","smør"] },
  { name:"Ris med smør og salt", ingredients:["ris","smør"] },
  { name:"Pasta med smør og ost", ingredients:["pasta","smør"] },
  { name:"Kartoffel med ost", ingredients:["kartofler","ost"] },
  { name:"Gulerod med ost", ingredients:["gulerod","ost"] },
  { name:"Brød med ost og smør", ingredients:["brød","ost"] },
  { name:"Rugbrød med ost og smør", ingredients:["rugbrød","ost"] },
  { name:"Æble med ost", ingredients:["æble","ost"] },
  { name:"Banan med ost", ingredients:["banan","ost"] },
  { name:"Tomat med ost og smør", ingredients:["tomat","ost"] },
  { name:"Agurk med ost", ingredients:["agurk","ost"] },
  { name:"Pølse med smør", ingredients:["pølse","smør"] },
  { name:"Skinke med smør", ingredients:["skinke","smør"] },
  { name:"Kartoffel med honning", ingredients:["kartofler","honning"] },
  { name:"Gulerod med kanel", ingredients:["gulerod","kanel"] },
  { name:"Brød med kanel og smør", ingredients:["brød","kanel"] },
  { name:"Rugbrød med kanel og smør", ingredients:["rugbrød","kanel"] },
  { name:"Æg med olie og salt", ingredients:["æg","olie"] },
  { name:"Yoghurt med honning og kanel", ingredients:["yoghurt","honning"] },
  { name:"Ris med honning", ingredients:["ris","honning"] },
  { name:"Pasta med honning", ingredients:["pasta","honning"] },
  { name:"Kartoffel med kanel", ingredients:["kartofler","kanel"] },
  { name:"Gulerod med honning og kanel", ingredients:["gulerod","honning"] },
  { name:"Brød med peanutbutter og honning", ingredients:["brød","peanutbutter"] },
  { name:"Rugbrød med peanutbutter og honning", ingredients:["rugbrød","peanutbutter"] },
  { name:"Æble med honning og kanel", ingredients:["æble","honning"] },
  { name:"Banan med peanutbutter", ingredients:["banan","peanutbutter"] },
  { name:"Tomat med ketchup", ingredients:["tomat","ketchup"] },
  { name:"Agurk med mayonnaise", ingredients:["agurk","mayonnaise"] },
  { name:"Ristet rugbrød med avocado", ingredients:["rugbrød","avocado","salt"] },
  { name:"Yoghurt med honning", ingredients:["yoghurt","honning"] },
  { name:"Kold havregrød", ingredients:["havregryn","mælk","honning"] },
  { name:"Æggetoast", ingredients:["æg","brød","smør"] },
  { name:"Bananpandekager", ingredients:["banan","æg"] },
  { name:"Tomatskiver med mozzarella", ingredients:["tomat","mozzarella","olivenolie"] },
  { name:"Kold pasta med pesto", ingredients:["pasta","pesto"] },
  { name:"Grøntsagsomelet", ingredients:["æg","grøntsager","ost"] },
  { name:"Bagte kartoffelbåde", ingredients:["kartofler","olie","salt"] },
  { name:"Agurkesalat", ingredients:["agurk","eddike","sukker"] },
  { name:"Æblechips", ingredients:["æble","kanel"] },
  { name:"Rugbrød med leverpostej", ingredients:["rugbrød","leverpostej"] },
  { name:"Tun med majs", ingredients:["tun","majs","mayonnaise"] },
  { name:"Wrap med ost og skinke", ingredients:["tortilla","ost","skinke"] },
  { name:"Græsk yoghurt med bær", ingredients:["græsk yoghurt","bær","honning"] },
  { name:"Ris med smør", ingredients:["ris","smør","salt"] },
  { name:"Kyllingesalat", ingredients:["kylling","mayonnaise","æble"] },
  { name:"Stegt æg på rugbrød", ingredients:["æg","rugbrød","smør"] },
  { name:"Tomatsuppe", ingredients:["tomat","fløde","løg"] },
  { name:"Bagte gulerødder", ingredients:["gulerødder","olie","honning"] },
  { name:"Pasta med ketchup", ingredients:["pasta","ketchup"] },
  { name:"Græskarsuppe", ingredients:["græskar","fløde","løg"] },
  { name:"Skyr med müsli", ingredients:["skyr","müsli"] },
  { name:"Kold risengrød", ingredients:["risengrød","kanel","sukker"] },
  { name:"Æg med tomat", ingredients:["æg","tomat","salt"] },
  { name:"Bagte æg i peberfrugt", ingredients:["æg","peberfrugt"] },
  { name:"Kikærtesalat", ingredients:["kikærter","tomat","citron"] },
  { name:"Æble med peanutbutter", ingredients:["æble","peanutbutter"] },
  { name:"Gulerodssnack", ingredients:["gulerod","dip"] },
  { name:"Kartoffelmos med smør", ingredients:["kartofler","smør","mælk"] },
  { name:"Tunpasta", ingredients:["tun","pasta","mayonnaise"] },
  { name:"Toast med banan", ingredients:["brød","banan","smør"] },
  { name:"Kold kakao", ingredients:["mælk","kakaopulver"] },
  { name:"Æg med bacon", ingredients:["æg","bacon"] },
  { name:"Smoothie med banan og mælk", ingredients:["banan","mælk"] },
  { name:"Avocadomad", ingredients:["avocado","rugbrød"] },
  { name:"Mini æggepizza", ingredients:["æg","ost","tomat"] },
  { name:"Bagt tomat med ost", ingredients:["tomat","ost"] },
  { name:"Agurkestænger med dip", ingredients:["agurk","yoghurt"] },
  { name:"Bagte champignoner", ingredients:["champignoner","smør","salt"] },
  { name:"Frugtsalat", ingredients:["æble","banan","yoghurt"] },
  { name:"Kold ris med kanel", ingredients:["ris","kanel","mælk"] },
  { name:"Wrap med æg og salat", ingredients:["tortilla","æg","salat"] },
  { name:"Pasta med smør", ingredients:["pasta","smør"] },
  { name:"Æblegrød", ingredients:["æble","sukker","vand"] },
  { name:"Røræg med ost", ingredients:["æg","ost","smør"] },
  { name:"Tomat sandwich", ingredients:["brød","tomat","smør"] },
  { name:"Kylling på rugbrød", ingredients:["kylling","rugbrød","mayonnaise"] },
  { name:"Bagte æbler med honning", ingredients:["æble","honning"] },
  { name:"Æble og ost snack", ingredients:["æble","ost"] },
  { name:"Pære med yoghurt", ingredients:["pære","yoghurt"] },
  { name:"Rugbrød med avocado og æg", ingredients:["rugbrød","avocado","æg"] },
  { name:"Skinkeomelet", ingredients:["æg","skinke"] },
  { name:"Bagt kartoffel med ost", ingredients:["kartoffel","ost"] },
  { name:"Kold kakao med is", ingredients:["mælk","is"] },
  { name:"Banan med chokolade", ingredients:["banan","chokolade"] },
  { name:"Toast med æg", ingredients:["brød","æg"] },
  { name:"Tunmad", ingredients:["rugbrød","tun","mayonnaise"] },
  { name:"Pasta med majs", ingredients:["pasta","majs","smør"] },
  { name:"Karryris", ingredients:["ris","karry","olie"] },
  { name:"Æg i kop med ost", ingredients:["æg","ost"] },
  { name:"Kylling i karry", ingredients:["kylling","karry","fløde"] },
  { name:"Kold pastasalat", ingredients:["pasta","ærter","mayonnaise"] },
  { name:"Grøntsagswrap", ingredients:["tortilla","salat","tomat"] },
  { name:"Bagt banan med honning", ingredients:["banan","honning"] },
  { name:"Mælkesmoothie", ingredients:["mælk","frugt"] },
  { name:"Æg og ris", ingredients:["æg","ris"] },
  { name:"Ostemad", ingredients:["brød","ost"] },
  { name:"Bagte ærter", ingredients:["ærter","olie","salt"] },
  { name:"Tomat med salt", ingredients:["tomat","salt"] },
  { name:"Æg og kylling", ingredients:["æg","kylling"] },
  { name:"Kold pasta med æg", ingredients:["pasta","æg","mayonnaise"] },
  { name:"Rugbrød med æg", ingredients:["rugbrød","æg","smør"] },
  { name:"Gulerod og hummus", ingredients:["gulerod","hummus"] },
  { name:"Bagte løg", ingredients:["løg","olie","salt"] },
  { name:"Kartoffelsnack", ingredients:["kartofler","salt"] },
  { name:"Brød med smør", ingredients:["brød","smør"] },
  { name:"Toast med syltetøj", ingredients:["brød","syltetøj"] },
  { name:"Rugbrød med ost", ingredients:["rugbrød","ost"] },
  { name:"Æg og majs", ingredients:["æg","majs"] },
  { name:"Kold risengrød med mælk", ingredients:["risengrød","mælk"] },
  { name:"Omelet med løg", ingredients:["æg","løg"] },
  { name:"Bagt pølse", ingredients:["pølse","smør"] },
  { name:"Frisk tomatsalat", ingredients:["tomat","løg","olie"] },
  { name:"Yoghurt med æble", ingredients:["yoghurt","æble"] },
  { name:"Æg og tomatsovs", ingredients:["æg","tomatsovs"] },
  { name:"Rugbrød med makrel", ingredients:["rugbrød","makrel"] },
  { name:"Skyr med peanutbutter", ingredients:["skyr","peanutbutter"] },
  { name:"Kyllingesandwich", ingredients:["brød","kylling","salat"] },
  { name:"Ris med majs", ingredients:["ris","majs"] },
  { name:"Bagte æg i form", ingredients:["æg","smør"] },
  { name:"Toast med kylling", ingredients:["brød","kylling"] },
  { name:"Æg og spinat", ingredients:["æg","spinat"] },
  { name:"Rugbrød med syltetøj", ingredients:["rugbrød","syltetøj"] },
  { name:"Bagt avocado", ingredients:["avocado","æg"] },
  { name:"Kartoffelæg", ingredients:["kartoffel","æg"] },
  { name:"Mælk og cornflakes", ingredients:["mælk","cornflakes"] },
  { name:"Æble med honning", ingredients:["æble","honning"] },
  { name:"Ris og ærter", ingredients:["ris","ærter"] },
  { name:"Tomatskiver med peber", ingredients:["tomat","peber"] },
  { name:"Rugbrød med smør", ingredients:["rugbrød","smør"] }
  { name:"", ingredients:["rugbrød","smør"] }
  

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
    .filter(i => i);

  // Nulstil listen
  recipeList.innerHTML = '';

  if (userIngredients.length === 0) {
    recipeList.innerHTML = '<li>Skriv mindst én ingrediens.</li>';
    return;
  }

  // Filtrér opskrifter hvor man har ALLE ingredienserne
  const possibleRecipes = recipes.filter(recipe =>
    recipe.ingredients.every(i => userIngredients.includes(i))
  );

  // Hvis ingen opskrifter matcher
  if (possibleRecipes.length === 0) {
    recipeList.innerHTML = '<li>Desværre får du ingen mad i dag med de ingredienser.</li>';
    return;
  }

  // Vis alle mulige opskrifter
  possibleRecipes.forEach(recipe => {
    const li = document.createElement('li');
    li.className = 'ready';
    li.innerHTML = `
      <strong>${recipe.name}</strong><br>
      <em>Ingredienser:</em> ${recipe.ingredients.join(', ')}<br>
      <em>Du kan lave denne!</em>
    `;
    recipeList.appendChild(li);
  });
}


// Event listener
findBtn.addEventListener('click', findRecipes);
</script>
</body>
</html>
