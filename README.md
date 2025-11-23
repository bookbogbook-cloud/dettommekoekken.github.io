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
  { navn:"Pandekager", ingredients:["mel","mælk","æg","sukker","smør"] },
  { navn:"Simpel salat", ingredients:["salat","tomat","agurk","olivenolie","salt"] },
  { navn:"Kartoffelmos", ingredients:["kartofler","smør","mælk","salt","peber"] },
  { navn:"Kyllingesalat", ingredients:["kylling","salat","majs","agurk","dressing"] },
  { navn:"Røræg", ingredients:["æg","mælk","smør","salt","peber"] },
  { navn:"Havregrød", ingredients:["havregryn","mælk","sukker","smør"] },
  { navn:"Frugtsalat", ingredients:["æble","banan","appelsin","yoghurt","honning"] },
  { navn:"Tunsalat", ingredients:["tun","majs","løg","mayonnaise","citron"] },
  { navn:"Karrysuppe", ingredients:["kylling","løg","gulerod","karry","fløde"] },
  { navn:"Lasagne", ingredients:["lasagneplader","oksekød","tomatsovs","ost","løg"] },
  { navn:"Boller", ingredients:["mel","gær","vand","sukker","smør"] },
  { navn:"Pizzadej", ingredients:["mel","gær","vand","olivenolie","salt"] },
  { navn:"Pizza Margherita", ingredients:["pizzadej","tomatsovs","ost","basilikum"] },
  { navn:"Smoothie", ingredients:["banan","jordbær","mælk","honning"] },
  { navn:"Græsk salat", ingredients:["feta","tomat","agurk","oliven","løg"] },
  { navn:"Stegt ris", ingredients:["ris","æg","sojasovs","ærter","gulerod"] },
  { navn:"Pastasalat", ingredients:["pasta","majs","skinke","ærter","creme fraiche"] },
  { navn:"Kylling i karry", ingredients:["kylling","karry","løg","fløde","ris"] },
  { navn:"Æblekage", ingredients:["æble","sukker","smør","havregryn","kanel"] },
  { navn:"Chokoladekage", ingredients:["mel","sukker","æg","kakao","smør"] },
  { navn:"Brownies", ingredients:["smør","sukker","æg","mel","kakao"] },
  { navn:"Banankage", ingredients:["banan","mel","æg","sukker","smør"] },
  { navn:"Tomatsuppe", ingredients:["tomat","løg","hvidløg","bouillon","fløde"] },
  { navn:"Ratatouille", ingredients:["aubergine","squash","tomat","peberfrugt","løg"] },
  { navn:"Kyllingesandwich", ingredients:["brød","kylling","salat","tomat","mayonnaise"] },
  { navn:"Linsesuppe", ingredients:["linser","løg","gulerod","hvidløg","bouillon"] },
  { navn:"Spaghetti Bolognese", ingredients:["oksekød","tomat","spaghetti","løg","hvidløg"] },
  { navn:"Fiskefrikadeller", ingredients:["fisk","æg","mel","salt","peber"] },
  { navn:"Kartoffelsalat", ingredients:["kartofler","mayonnaise","sennep","purløg"] },
  { navn:"Æggemadder", ingredients:["æg","rugbrød","smør","salt"] },
  { navn:"Hotdog", ingredients:["pølse","brød","sennep","ketchup","ristet løg"] },
  { navn:"Burger", ingredients:["oksekød","bolle","ost","salat","tomat"] },
  { navn:"Falafel", ingredients:["kikærter","løg","hvidløg","persille","mel"] },
  { navn:"Hummus", ingredients:["kikærter","tahini","citron","hvidløg","olivenolie"] },
  { navn:"Tzatziki", ingredients:["yoghurt","agurk","hvidløg","citron","salt"] },
  { navn:"Rugbrødsmad med laks", ingredients:["rugbrød","røget laks","flødeost","dild","citron"] },
  { navn:"Taco", ingredients:["tacobrød","oksekød","tomat","salat","ost"] },
  { navn:"Wraps", ingredients:["tortilla","kylling","salat","agurk","dressing"] },
  { navn:"Muffins", ingredients:["mel","sukker","æg","smør","bagepulver"] },
  { navn:"Bananpandekager", ingredients:["banan","æg","havregryn","mælk"] },
  { navn:"Smoothie bowl", ingredients:["banan","jordbær","yoghurt","honning","havregryn"] },
  { navn:"Vafler", ingredients:["mel","æg","mælk","sukker","smør"] },
  { navn:"Toast", ingredients:["brød","ost","skinke","smør"] },
  { navn:"Pasta med pesto", ingredients:["pasta","pesto","parmesan","olivenolie"] },
  { navn:"Koldskål", ingredients:["kærnemælk","æg","sukker","vanilje","kammerjunker"] },
  { navn:"Rugbrødsmad med leverpostej", ingredients:["rugbrød","leverpostej","agurk"] },
  { navn:"Bagte kartofler", ingredients:["kartofler","smør","salt","peber"] },
  { navn:"Æggekage", ingredients:["æg","mælk","bacon","purløg","salt"] },
  { navn:"Omelet med rester", ingredients:["æg","mælk","ost","grøntsager","salt"] },
  { navn:"Pastarester med ketchup", ingredients:["pasta","ketchup","smør","salt"] },
  { navn:"Toast med ost og skinke", ingredients:["brød","ost","skinke","smør"] },
  { navn:"Rugbrødspizza", ingredients:["rugbrød","tomatsovs","ost","peberfrugt"] },
  { navn:"Kartoffelomelet", ingredients:["æg","kartofler","løg","salt","olie"] },
  { navn:"Yoghurt med frugt og honning", ingredients:["yoghurt","banan","honning","havregryn"] },
  { navn:"Grøntsagssuppe af rester", ingredients:["gulerod","kartoffel","løg","bouillon","vand"] },
  { navn:"Æg i kop", ingredients:["æg","smør","salt","peber"] },
  { navn:"Stegte ris med æg", ingredients:["ris","æg","sojasovs","ærter","olie"] },
  { navn:"Kold pasta med mayo", ingredients:["pasta","mayonnaise","majs","ærter"] },
  { navn:"Tortilla med ost", ingredients:["tortilla","ost","smør","krydderier"] },
  { navn:"Æblegrød", ingredients:["æble","sukker","vand","kanel"] },
  { navn:"Bagte gulerødder", ingredients:["gulerod","olie","salt","peber"] },
  { navn:"Røræg", ingredients:["æg","mælk","smør","salt"] },
  { navn:"Mælk og havregryn", ingredients:["havregryn","mælk","sukker"] },
  { navn:"Toast med peanutbutter og banan", ingredients:["brød","peanutbutter","banan"] },
  { navn:"Grillet ostesandwich", ingredients:["brød","ost","smør"] },
  { navn:"Mini-pizza af toastbrød", ingredients:["brød","tomatsovs","ost","skinke"] },
  { navn:"Kold kartoffelsalat", ingredients:["kartofler","mayonnaise","sennep","purløg"] },
  { navn:"Bagte æbler", ingredients:["æble","smør","kanel","sukker"] },
  { navn:"Rester-wrap", ingredients:["tortilla","kylling","salat","dressing"] },
  { navn:"Pasta med smør og ost", ingredients:["pasta","smør","ost","salt"] },
  { navn:"Tunmayo sandwich", ingredients:["tun","mayonnaise","brød","citron"] },
  { navn:"Spejlæg på rugbrød", ingredients:["æg","smør","rugbrød","salt"] },
  { navn:"Bananpandekager", ingredients:["banan","æg","havregryn"] },
  { navn:"Resterisotto", ingredients:["ris","ost","smør","bouillon"] },
  { navn:"Koldskål med frugt", ingredients:["kærnemælk","æg","sukker","frugt"] },
  { navn:"Æggekage i ovn", ingredients:["æg","mælk","ost","bacon"] },
  { navn:"Grøntsagsfringre", ingredients:["gulerod","kartoffel","olie","salt"] },
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
  { navn:"Mælkeshake", ingredients:["mælk","banan","sukker","is"] },
  { navn:"Pølseomelet", ingredients:["æg","pølse","løg","smør"] },
  { navn:"Kold pastasalat", ingredients:["pasta","majs","ærter","skinke","dressing"] },
  { navn:"Grøntsagspandekager", ingredients:["mel","æg","mælk","gulerod"] },
  { navn:"Pizzasnegle af rester", ingredients:["dej","tomatsovs","ost","skinke"] },
  { navn:"Æg med tomat og ost", ingredients:["æg","tomat","ost","salt"] },
  { navn:"Rugbrødsmad deluxe", ingredients:["rugbrød","ost","tomat","pesto"] },
  { navn:"Banan på brød", ingredients:["brød","banan"] },
  { navn:"Ost på brød", ingredients:["brød","ost"] },
  { navn:"Smør på brød", ingredients:["brød","smør"] },
  { navn:"Æble med honning", ingredients:["æble","honning"] },
  { navn:"Banan med honning", ingredients:["banan","honning"] },
  { navn:"Æg med salt", ingredients:["æg","salt"] },
  { navn:"Æg med peber", ingredients:["æg","peber"] },
  { navn:"Yoghurt med honning", ingredients:["yoghurt","honning"] },
  { navn:"Tomat med ost", ingredients:["tomat","ost"] },
  { navn:"Agurk med salt", ingredients:["agurk","salt"] },
  { navn:"Kartoffel med smør", ingredients:["kartofler","smør"] },
  { navn:"Gulerod med olie", ingredients:["gulerod","olie"] },
  { navn:"Ris med olie", ingredients:["ris","olie"] },
  { navn:"Pasta med olie", ingredients:["pasta","olie"] },
  { navn:"Brød med peanutbutter", ingredients:["brød","peanutbutter"] },
  { navn:"Rugbrød med ost", ingredients:["rugbrød","ost"] },
  { navn:"Æble med kanel", ingredients:["æble","kanel"] },
  { navn:"Banan med smør", ingredients:["banan","smør"] },
  { navn:"Tomat med olie", ingredients:["tomat","olie"] },
  { navn:"Agurk med olie", ingredients:["agurk","olie"] },
  { navn:"Pølse med brød", ingredients:["pølse","brød"] },
  { navn:"Skinke med brød", ingredients:["skinke","brød"] },
  { navn:"Koldskål med sukker", ingredients:["koldskål","sukker"] },
  { navn:"Havregryn med mælk", ingredients:["havregryn","mælk"] },
  { navn:"Banan med yoghurt", ingredients:["banan","yoghurt"] },
  { navn:"Æble med yoghurt", ingredients:["æble","yoghurt"] },
  { navn:"Smør med ost", ingredients:["smør","ost"] },
  { navn:"Kartoffel med salt", ingredients:["kartofler","salt"] },
  { navn:"Gulerod med salt", ingredients:["gulerod","salt"] },
  { navn:"Tomat med salt", ingredients:["tomat","salt"] },
  { navn:"Agurk med peber", ingredients:["agurk","peber"] },
  { navn:"Rugbrød med smør", ingredients:["rugbrød","smør"] },
  { navn:"Pasta med ost", ingredients:["pasta","ost"] },
  { navn:"Ris med smør", ingredients:["ris","smør"] },
  { navn:"Brød med ost", ingredients:["brød","ost"] },
  { navn:"Æg med olie", ingredients:["æg","olie"] },
  { navn:"Banan med peber", ingredients:["banan","peber"] },
  { navn:"Æble med peber", ingredients:["æble","peber"] },
  { navn:"Tomat med peber", ingredients:["tomat","peber"] },
  { navn:"Kartoffel med peber", ingredients:["kartofler","peber"] },
  { navn:"Gulerod med honning", ingredients:["gulerod","honning"] },
  { navn:"Ris med salt", ingredients:["ris","salt"] },
  { navn:"Pasta med salt", ingredients:["pasta","salt"] },
  { navn:"Yoghurt med sukker", ingredients:["yoghurt","sukker"] },
  { navn:"Æg med honning", ingredients:["æg","honning"] },
  { navn:"Brød med honning", ingredients:["brød","honning"] },
  { navn:"Rugbrød med honning", ingredients:["rugbrød","honning"] },
  { navn:"Banan med kanel", ingredients:["banan","kanel"] },
  { navn:"Æble med smør", ingredients:["æble","smør"] },
  { navn:"Tomat med honning", ingredients:["tomat","honning"] },
  { navn:"Agurk med honning", ingredients:["agurk","honning"] },
  { navn:"Pølse med ost", ingredients:["pølse","ost"] },
  { navn:"Skinke med ost", ingredients:["skinke","ost"] },
  { navn:"Kartoffel med peber", ingredients:["kartofler","peber"] },
  { navn:"Gulerod med peber", ingredients:["gulerod","peber"] },
  { navn:"Brød med kanel", ingredients:["brød","kanel"] },
  { navn:"Rugbrød med kanel", ingredients:["rugbrød","kanel"] },
  { navn:"Æg med kanel", ingredients:["æg","kanel"] },
  { navn:"Yoghurt med kanel", ingredients:["yoghurt","kanel"] },
  { navn:"Ris med peber", ingredients:["ris","peber"] },
  { navn:"Pasta med peber", ingredients:["pasta","peber"] },
  { navn:"Kartoffel med olie", ingredients:["kartofler","olie"] },
  { navn:"Gulerod med smør", ingredients:["gulerod","smør"] },
  { navn:"Brød med olie", ingredients:["brød","olie"] },
  { navn:"Rugbrød med olie", ingredients:["rugbrød","olie"] },
  { navn:"Æble med olie", ingredients:["æble","olie"] },
  { navn:"Banan med olie", ingredients:["banan","olie"] },
  { navn:"Tomat med smør", ingredients:["tomat","smør"] },
  { navn:"Agurk med smør", ingredients:["agurk","smør"] },
  { navn:"Æg med ost", ingredients:["æg","ost"] },
  { navn:"Yoghurt med smør", ingredients:["yoghurt","smør"] },
  { navn:"Ris med smør og salt", ingredients:["ris","smør"] },
  { navn:"Pasta med smør og ost", ingredients:["pasta","smør"] },
  { navn:"Kartoffel med ost", ingredients:["kartofler","ost"] },
  { navn:"Gulerod med ost", ingredients:["gulerod","ost"] },
  { navn:"Brød med ost og smør", ingredients:["brød","ost"] },
  { navn:"Rugbrød med ost og smør", ingredients:["rugbrød","ost"] },
  { navn:"Æble med ost", ingredients:["æble","ost"] },
  { navn:"Banan med ost", ingredients:["banan","ost"] },
  { navn:"Tomat med ost og smør", ingredients:["tomat","ost"] },
  { navn:"Agurk med ost", ingredients:["agurk","ost"] },
  { navn:"Pølse med smør", ingredients:["pølse","smør"] },
  { navn:"Skinke med smør", ingredients:["skinke","smør"] },
  { navn:"Kartoffel med honning", ingredients:["kartofler","honning"] },
  { navn:"Gulerod med kanel", ingredients:["gulerod","kanel"] },
  { navn:"Brød med kanel og smør", ingredients:["brød","kanel"] },
  { navn:"Rugbrød med kanel og smør", ingredients:["rugbrød","kanel"] },
  { navn:"Æg med olie og salt", ingredients:["æg","olie"] },
  { navn:"Yoghurt med honning og kanel", ingredients:["yoghurt","honning"] },
  { navn:"Ris med honning", ingredients:["ris","honning"] },
  { navn:"Pasta med honning", ingredients:["pasta","honning"] },
  { navn:"Kartoffel med kanel", ingredients:["kartofler","kanel"] },
  { navn:"Gulerod med honning og kanel", ingredients:["gulerod","honning"] },
  { navn:"Brød med peanutbutter og honning", ingredients:["brød","peanutbutter"] },
  { navn:"Rugbrød med peanutbutter og honning", ingredients:["rugbrød","peanutbutter"] },
  { navn:"Æble med honning og kanel", ingredients:["æble","honning"] },
  { navn:"Banan med peanutbutter", ingredients:["banan","peanutbutter"] },
  { navn:"Tomat med ketchup", ingredients:["tomat","ketchup"] },
  { navn:"Agurk med mayonnaise", ingredients:["agurk","mayonnaise"] },
  { navn:"Ristet rugbrød med avocado", ingredients:["rugbrød","avocado","salt"] },
  { navn:"Yoghurt med honning", ingredients:["yoghurt","honning"] },
  { navn:"Kold havregrød", ingredients:["havregryn","mælk","honning"] },
  { navn:"Æggetoast", ingredients:["æg","brød","smør"] },
  { navn:"Bananpandekager", ingredients:["banan","æg"] },
  { navn:"Tomatskiver med mozzarella", ingredients:["tomat","mozzarella","olivenolie"] },
  { navn:"Kold pasta med pesto", ingredients:["pasta","pesto"] },
  { navn:"Grøntsagsomelet", ingredients:["æg","grøntsager","ost"] },
  { navn:"Bagte kartoffelbåde", ingredients:["kartofler","olie","salt"] },
  { navn:"Agurkesalat", ingredients:["agurk","eddike","sukker"] },
  { navn:"Æblechips", ingredients:["æble","kanel"] },
  { navn:"Rugbrød med leverpostej", ingredients:["rugbrød","leverpostej"] },
  { navn:"Tun med majs", ingredients:["tun","majs","mayonnaise"] },
  { navn:"Wrap med ost og skinke", ingredients:["tortilla","ost","skinke"] },
  { navn:"Græsk yoghurt med bær", ingredients:["græsk yoghurt","bær","honning"] },
  { navn:"Ris med smør", ingredients:["ris","smør","salt"] },
  { navn:"Kyllingesalat", ingredients:["kylling","mayonnaise","æble"] },
  { navn:"Stegt æg på rugbrød", ingredients:["æg","rugbrød","smør"] },
  { navn:"Tomatsuppe", ingredients:["tomat","fløde","løg"] },
  { navn:"Bagte gulerødder", ingredients:["gulerødder","olie","honning"] },
  { navn:"Pasta med ketchup", ingredients:["pasta","ketchup"] },
  { navn:"Græskarsuppe", ingredients:["græskar","fløde","løg"] },
  { navn:"Skyr med müsli", ingredients:["skyr","müsli"] },
  { navn:"Kold risengrød", ingredients:["risengrød","kanel","sukker"] },
  { navn:"Æg med tomat", ingredients:["æg","tomat","salt"] },
  { navn:"Bagte æg i peberfrugt", ingredients:["æg","peberfrugt"] },
  { navn:"Kikærtesalat", ingredients:["kikærter","tomat","citron"] },
  { navn:"Æble med peanutbutter", ingredients:["æble","peanutbutter"] },
  { navn:"Gulerodssnack", ingredients:["gulerod","dip"] },
  { navn:"Kartoffelmos med smør", ingredients:["kartofler","smør","mælk"] },
  { navn:"Tunpasta", ingredients:["tun","pasta","mayonnaise"] },
  { navn:"Toast med banan", ingredients:["brød","banan","smør"] },
  { navn:"Kold kakao", ingredients:["mælk","kakaopulver"] },
  { navn:"Æg med bacon", ingredients:["æg","bacon"] },
  { navn:"Smoothie med banan og mælk", ingredients:["banan","mælk"] },
  { navn:"Avocadomad", ingredients:["avocado","rugbrød"] },
  { navn:"Mini æggepizza", ingredients:["æg","ost","tomat"] },
  { navn:"Bagt tomat med ost", ingredients:["tomat","ost"] },
  { navn:"Agurkestænger med dip", ingredients:["agurk","yoghurt"] },
  { navn:"Bagte champignoner", ingredients:["champignoner","smør","salt"] },
  { navn:"Frugtsalat", ingredients:["æble","banan","yoghurt"] },
  { navn:"Kold ris med kanel", ingredients:["ris","kanel","mælk"] },
  { navn:"Wrap med æg og salat", ingredients:["tortilla","æg","salat"] },
  { navn:"Pasta med smør", ingredients:["pasta","smør"] },
  { navn:"Æblegrød", ingredients:["æble","sukker","vand"] },
  { navn:"Røræg med ost", ingredients:["æg","ost","smør"] },
  { navn:"Tomat sandwich", ingredients:["brød","tomat","smør"] },
  { navn:"Kylling på rugbrød", ingredients:["kylling","rugbrød","mayonnaise"] },
  { navn:"Bagte æbler med honning", ingredients:["æble","honning"] },
  { navn:"Æble og ost snack", ingredients:["æble","ost"] },
  { navn:"Pære med yoghurt", ingredients:["pære","yoghurt"] },
  { navn:"Rugbrød med avocado og æg", ingredients:["rugbrød","avocado","æg"] },
  { navn:"Skinkeomelet", ingredients:["æg","skinke"] },
  { navn:"Bagt kartoffel med ost", ingredients:["kartoffel","ost"] },
  { navn:"Kold kakao med is", ingredients:["mælk","is"] },
  { navn:"Banan med chokolade", ingredients:["banan","chokolade"] },
  { navn:"Toast med æg", ingredients:["brød","æg"] },
  { navn:"Tunmad", ingredients:["rugbrød","tun","mayonnaise"] },
  { navn:"Pasta med majs", ingredients:["pasta","majs","smør"] },
  { navn:"Karryris", ingredients:["ris","karry","olie"] },
  { navn:"Æg i kop med ost", ingredients:["æg","ost"] },
  { navn:"Kylling i karry", ingredients:["kylling","karry","fløde"] },
  { navn:"Kold pastasalat", ingredients:["pasta","ærter","mayonnaise"] },
  { navn:"Grøntsagswrap", ingredients:["tortilla","salat","tomat"] },
  { navn:"Bagt banan med honning", ingredients:["banan","honning"] },
  { navn:"Mælkesmoothie", ingredients:["mælk","frugt"] },
  { navn:"Æg og ris", ingredients:["æg","ris"] },
  { navn:"Ostemad", ingredients:["brød","ost"] },
  { navn:"Bagte ærter", ingredients:["ærter","olie","salt"] },
  { navn:"Tomat med salt", ingredients:["tomat","salt"] },
  { navn:"Æg og kylling", ingredients:["æg","kylling"] },
  { navn:"Kold pasta med æg", ingredients:["pasta","æg","mayonnaise"] },
  { navn:"Rugbrød med æg", ingredients:["rugbrød","æg","smør"] },
  { navn:"Gulerod og hummus", ingredients:["gulerod","hummus"] },
  { navn:"Bagte løg", ingredients:["løg","olie","salt"] },
  { navn:"Kartoffelsnack", ingredients:["kartofler","salt"] },
  { navn:"Brød med smør", ingredients:["brød","smør"] },
  { navn:"Toast med syltetøj", ingredients:["brød","syltetøj"] },
  { navn:"Rugbrød med ost", ingredients:["rugbrød","ost"] },
  { navn:"Æg og majs", ingredients:["æg","majs"] },
  { navn:"Kold risengrød med mælk", ingredients:["risengrød","mælk"] },
  { navn:"Omelet med løg", ingredients:["æg","løg"] },
  { navn:"Bagt pølse", ingredients:["pølse","smør"] },
  { navn:"Frisk tomatsalat", ingredients:["tomat","løg","olie"] },
  { navn:"Yoghurt med æble", ingredients:["yoghurt","æble"] },
  { navn:"Æg og tomatsovs", ingredients:["æg","tomatsovs"] },
  { navn:"Rugbrød med makrel", ingredients:["rugbrød","makrel"] },
  { navn:"Skyr med peanutbutter", ingredients:["skyr","peanutbutter"] },
  { navn:"Kyllingesandwich", ingredients:["brød","kylling","salat"] },
  { navn:"Ris med majs", ingredients:["ris","majs"] },
  { navn:"Bagte æg i form", ingredients:["æg","smør"] },
  { navn:"Toast med kylling", ingredients:["brød","kylling"] },
  { navn:"Æg og spinat", ingredients:["æg","spinat"] },
  { navn:"Rugbrød med syltetøj", ingredients:["rugbrød","syltetøj"] },
  { navn:"Bagt avocado", ingredients:["avocado","æg"] },
  { navn:"Kartoffelæg", ingredients:["kartoffel","æg"] },
  { navn:"Mælk og cornflakes", ingredients:["mælk","cornflakes"] },
  { navn:"Æble med honning", ingredients:["æble","honning"] },
  { navn:"Ris og ærter", ingredients:["ris","ærter"] },
  { navn:"Tomatskiver med peber", ingredients:["tomat","peber"] },
  { navn:"Rugbrød med smør", ingredients:["rugbrød","smør"] }
  { navn:"Dexters livret", ingredients:["dødt menneske","blod"] }
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
