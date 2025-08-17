<!DOCTYPE html>
<html>
<head>
  <title>Quiz Adventure - Multi Subject</title>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Roboto', sans-serif;
      background: linear-gradient(to bottom right, #c3f0ca, #d2ebf9);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      margin: 0;
    }

    #container {
      width: 550px;
      padding: 25px;
      background-color: #fffefc;
      border-radius: 20px;
      box-shadow: 0px 8px 20px rgba(0,0,0,0.2);
      text-align: center;
    }

    #mess1, #message2, #gameWindow {
      width: 93%;
      background-color: #fff9c4; /* light yellow */
      padding: 18px;
      border-radius: 12px;
      border: 1px solid #ccc;
      font-size: 18px;
      margin-bottom: 12px;
      display: none;
      white-space: pre-line;
    }

    #level {
      font-size: 22px;
      font-weight: bold;
      margin-bottom: 15px;
      color: #333;
    }

    .btn1, .optionBtn, #submitName, #btn2, #startGameBtn, #nextBtn, #exitBtn, .menuBtn {
      width: 220px;
      height: 45px;
      font-size: 18px;
      font-weight: bold;
      color: white;
      background-color: #3f51b5; /* indigo */
      border: none;
      border-radius: 10px;
      cursor: pointer;
      margin: 10px auto;
      display: block;
      transition: background-color 0.3s;
    }

    .menuBtn.locked {
      background-color: gray !important;
      cursor: not-allowed;
    }

    .optionBtn:hover, .btn1:hover, #submitName:hover, #btn2:hover, #startGameBtn:hover, 
    #nextBtn:hover, #exitBtn:hover, .menuBtn:hover {
      background-color: #303f9f;
    }

    .optionBtn.correct {
      background-color: #4caf50 !important;
    }

    .optionBtn.wrong {
      background-color: #f44336 !important;
    }

    #feedback {
      margin-top: 15px;
      font-size: 18px;
      font-weight: bold;
    }

    #scoreBoard {
      margin-top: 15px;
      font-size: 18px;
      font-weight: bold;
    }

    input[type="text"] {
      width: 220px;
      height: 35px;
      font-size: 16px;
      border-radius: 8px;
      border: 1px solid #ccc;
      padding: 5px;
      margin-bottom: 10px;
    }
  </style>
  <script>
    let currentQuestion = 0;
    let score = 0;
    let totalLevels = 0;
    let playerName = "";
    let selectedSubject = "";

    // --- SCIENCE QUESTIONS (yours) ---
    const scienceQuestions = [
      { q:"1. What is the Chemical symbol of water?", options:["O2","H2O","CO2","HO"], answer:"H2O" },
      { q:"2. Also known as the Red Planet?", options:["Mars","Earth","Saturn","Sun"], answer:"Mars" },
      { q:"3. Part of cell with genetic material?", options:["Cytoplasm","Cell Membrane","Nucleus","Ribosome"], answer:"Nucleus" },
      { q:"4. Main gas in Earth's atmosphere?", options:["Oxygen","Carbon Dioxide","Nitrogen","Hydrogen"], answer:"Nitrogen" },
      { q:"5. Speed of light?", options:["300,000 km/s","299,000 km/s","3,000 km/s","156,000 km/s"], answer:"300,000 km/s" },
      { q:"6. Energy from the Sun?", options:["Nuclear","Solar","Chemical","Electrical"], answer:"Solar" },
      { q:"7. Human organ pumps blood?", options:["Pumpers","Heart","Liver","Brain"], answer:"Heart" },
      { q:"8. Which is the correct spelling?", options:["Phorsyntesis","Potosynthesis","Photosinthesis","Photosynthesis"], answer:"Photosynthesis" },
      { q:"9. Center of atom?", options:["Nucleus","Electron","Proton","Neutron"], answer:"Nucleus" },
      { q:"10. LI in periodic table?", options:["Mami Oni","Lighter","Lhitium","Lithium"], answer:"Lithium" },
      { q:"11. Planet closest to Sun?", options:["Mercury","Venus","Earth","Mars"], answer:"Mercury" },
      { q:"12. Boiling point of water?", options:["90°C","100°C","110°C","120°C"], answer:"100°C" },
      { q:"13. Largest organ in human body?", options:["Heart","Skin","Liver","Lungs"], answer:"Skin" },
      { q:"14. Hottest planet?", options:["Mercury","Venus","Mars","Earth"], answer:"Venus" },
      { q:"15. Gas we breathe in?", options:["Oxygen","Carbon Dioxide","Nitrogen","Helium"], answer:"Oxygen" },
      { q:"16. Liquid metal?", options:["Mercury","Gold","Silver","Copper"], answer:"Mercury" },
      { q:"17. Light travels at?", options:["300,000 km/s","150,000 km/s","299,000 km/s","200,000 km/s"], answer:"300,000 km/s" },
      { q:"18. Smallest particle?", options:["Atom","Molecule","Electron","Proton"], answer:"Atom" },
      { q:"19. Force that pulls objects?", options:["Magnetism","Gravity","Friction","Tension"], answer:"Gravity" },
      { q:"20. Planet with rings?", options:["Earth","Mars","Saturn","Venus"], answer:"Saturn" },
      { q:"21. Study of life?", options:["Biology","Physics","Chemistry","Geology"], answer:"Biology" },
      { q:"22. Study of stars?", options:["Astronomy","Biology","Geology","Physics"], answer:"Astronomy" },
      { q:"23. Gas for balloons?", options:["Oxygen","Hydrogen","Helium","Nitrogen"], answer:"Helium" },
      { q:"24. Human body's powerhouse?", options:["Brain","Liver","Heart","Mitochondria"], answer:"Mitochondria" },
      { q:"25. Fastest land animal?", options:["Cheetah","Lion","Tiger","Leopard"], answer:"Cheetah" },
      { q:"26. Largest planet?", options:["Earth","Jupiter","Saturn","Mars"], answer:"Jupiter" },
      { q:"27. Planet with water?", options:["Mars","Venus","Earth","Mercury"], answer:"Earth" },
      { q:"28. Animal with trunk?", options:["Elephant","Rhino","Hippo","Giraffe"], answer:"Elephant" },
      { q:"29. Tallest mammal?", options:["Giraffe","Elephant","Whale","Human"], answer:"Giraffe" },
      { q:"30. Gas humans exhale?", options:["Oxygen","Carbon Dioxide","Nitrogen","Hydrogen"], answer:"Carbon Dioxide" },
      { q:"31. Device to see stars?", options:["Microscope","Telescope","Binoculars","Periscope"], answer:"Telescope" },
      { q:"32. Organ filters blood?", options:["Heart","Kidney","Lungs","Liver"], answer:"Kidney" },
      { q:"33. First element in periodic table?", options:["Hydrogen","Helium","Oxygen","Carbon"], answer:"Hydrogen" },
      { q:"34. Vitamin from sun?", options:["Vitamin A","Vitamin B","Vitamin C","Vitamin D"], answer:"Vitamin D" },
      { q:"35. Gas for photosynthesis?", options:["Oxygen","Carbon Dioxide","Nitrogen","Helium"], answer:"Carbon Dioxide" },
      { q:"36. Nearest star?", options:["Sun","Sirius","Proxima Centauri","Alpha Centauri"], answer:"Sun" },
      { q:"37. Largest mammal?", options:["Elephant","Blue Whale","Giraffe","Hippopotamus"], answer:"Blue Whale" },
      { q:"38. Blood type universal donor?", options:["A","B","O","AB"], answer:"O" },
      { q:"39. Fastest bird?", options:["Eagle","Peregrine Falcon","Sparrow","Ostrich"], answer:"Peregrine Falcon" },
      { q:"40. First human in space?", options:["Yuri Gagarin","Neil Armstrong","Buzz Aldrin","Alan Shepard"], answer:"Yuri Gagarin" },
      { q:"41. Planet known as Morning Star?", options:["Venus","Mars","Mercury","Jupiter"], answer:"Venus" },
      { q:"42. Largest volcano?", options:["Mauna Loa","Mount Everest","Mount Fuji","Krakatoa"], answer:"Mauna Loa" },
      { q:"43. Hottest element?", options:["Iron","Platinum","Tungsten","Gold"], answer:"Tungsten" },
      { q:"44. Longest river?", options:["Nile","Amazon","Yangtze","Mississippi"], answer:"Nile" },
      { q:"45. Deepest ocean?", options:["Atlantic","Pacific","Indian","Arctic"], answer:"Pacific" },
      { q:"46. Largest desert?", options:["Sahara","Gobi","Kalahari","Arabian"], answer:"Sahara" },
      { q:"47. Fastest fish?", options:["Marlin","Tuna","Salmon","Shark"], answer:"Marlin" },
      { q:"48. Heaviest metal?", options:["Lead","Uranium","Gold","Mercury"], answer:"Uranium" },
      { q:"49. First president of USA?", options:["George Washington","Abraham Lincoln","Thomas Jefferson","John Adams"], answer:"George Washington" },
      { q:"50. Closest planet to Earth?", options:["Venus","Mars","Mercury","Jupiter"], answer:"Venus" }
    ];      

const mathQuestions = [
  {q:"What is 5 + 3?", options:["6","7","8","9"], answer:"8"},
  {q:"What is 9 - 4?", options:["3","4","5","6"], answer:"5"},
  {q:"What is 7 × 6?", options:["42","36","40","48"], answer:"42"},
  {q:"What is 56 ÷ 7?", options:["6","7","8","9"], answer:"8"},
  {q:"What is 12 + 15?", options:["25","26","27","28"], answer:"27"},
  {q:"What is 18 - 9?", options:["7","8","9","10"], answer:"9"},
  {q:"What is 15 × 2?", options:["25","30","35","40"], answer:"30"},
  {q:"What is 81 ÷ 9?", options:["7","8","9","10"], answer:"9"},
  {q:"What is 100 - 45?", options:["45","50","55","60"], answer:"55"},
  {q:"What is 11 + 14?", options:["23","24","25","26"], answer:"25"},
  {q:"What is 13 × 4?", options:["42","48","52","56"], answer:"52"},
  {q:"What is 144 ÷ 12?", options:["10","11","12","13"], answer:"12"},
  {q:"What is 20% of 200?", options:["20","30","40","50"], answer:"40"},
  {q:"Simplify: 2(3 + 5)", options:["14","15","16","18"], answer:"16"},
  {q:"What is the square of 9?", options:["72","80","81","90"], answer:"81"},
  {q:"What is the cube of 3?", options:["9","18","27","36"], answer:"27"},
  {q:"What is √64?", options:["6","7","8","9"], answer:"8"},
  {q:"What is √121?", options:["10","11","12","13"], answer:"11"},
  {q:"What is 7²?", options:["47","48","49","50"], answer:"49"},
  {q:"What is 15 × 15?", options:["200","210","220","225"], answer:"225"},
  {q:"What is 1/2 + 1/4?", options:["2/4","3/4","4/4","5/4"], answer:"3/4"},
  {q:"What is 2/3 of 9?", options:["3","4","5","6"], answer:"6"},
  {q:"Convert 0.75 to fraction.", options:["1/2","2/3","3/4","4/5"], answer:"3/4"},
  {q:"Convert 1/5 to decimal.", options:["0.1","0.2","0.25","0.5"], answer:"0.2"},
  {q:"What is 10% of 500?", options:["40","50","60","70"], answer:"50"},
  {q:"What is 25% of 80?", options:["15","18","20","25"], answer:"20"},
  {q:"Simplify: 4/8", options:["1/2","2/3","3/4","4/5"], answer:"1/2"},
  {q:"Simplify: 12/16", options:["2/3","3/4","4/5","5/6"], answer:"3/4"},
  {q:"What is 2.5 × 4?", options:["8","9","10","12"], answer:"10"},
  {q:"What is 7.2 ÷ 0.6?", options:["10","11","12","13"], answer:"12"},
  {q:"Solve: x + 5 = 12", options:["5","6","7","8"], answer:"7"},
  {q:"Solve: 2x = 18", options:["7","8","9","10"], answer:"9"},
  {q:"Solve: 3x - 4 = 11", options:["4","5","6","7"], answer:"5"},
  {q:"If x = 3, what is 2x + 7?", options:["11","12","13","14"], answer:"13"},
  {q:"Solve: 5x = 45", options:["7","8","9","10"], answer:"9"},
  {q:"Solve: x/4 = 6", options:["22","23","24","25"], answer:"24"},
  {q:"Solve: 7x + 2 = 30", options:["3","4","5","6"], answer:"4"},
  {q:"If y = 2, what is y² + 3?", options:["5","6","7","8"], answer:"7"},
  {q:"If a = 5, b = 2, what is a × b?", options:["8","9","10","11"], answer:"10"},
  {q:"Solve: x² = 49", options:["6","7","8","9"], answer:"7"},
  {q:"How many sides does a hexagon have?", options:["5","6","7","8"], answer:"6"},
  {q:"What is the sum of angles in a triangle?", options:["90°","120°","180°","360°"], answer:"180°"},
  {q:"How many degrees in a circle?", options:["180°","270°","360°","400°"], answer:"360°"},
  {q:"Area of a square with side 8?", options:["56","60","64","72"], answer:"64"},
  {q:"Perimeter of rectangle 5 × 8?", options:["22","24","26","28"], answer:"26"},
  {q:"Area of triangle (base=10, height=6)?", options:["25","28","30","35"], answer:"30"},
  {q:"What is the radius if diameter=14?", options:["5","6","7","8"], answer:"7"},
  {q:"How many faces does a cube have?", options:["4","6","8","12"], answer:"6"},
  {q:"Volume of cube side 3?", options:["9","18","27","36"], answer:"27"},
  {q:"What is the longest side of right triangle called?", options:["Base","Height","Hypotenuse","Diagonal"], answer:"Hypotenuse"},
  {q:"If 5 pens cost ₱100, how much is 1 pen?", options:["₱15","₱18","₱20","₱25"], answer:"₱20"},
  {q:"If car runs 60 km/hr, how far in 2 hrs?", options:["100 km","110 km","120 km","130 km"], answer:"120 km"},
  {q:"If 8 apples are shared by 4 kids, each gets?", options:["1","2","3","4"], answer:"2"},
  {q:"A box has 12 chocolates, if you eat 3, left?", options:["7","8","9","10"], answer:"9"},
  {q:"If 1 book costs ₱50, how much for 7 books?", options:["₱300","₱320","₱340","₱350"], answer:"₱350"}
];

const englishQuestions = [
  {q:"Choose the correct spelling:", options:["Recieve","Receive","Recive","Receeve"], answer:"Receive"},
  {q:"Synonym of 'Happy':", options:["Sad","Joyful","Angry","Tired"], answer:"Joyful"},
  {q:"Antonym of 'Big':", options:["Large","Huge","Small","Tall"], answer:"Small"},
  {q:"Fill in: I ____ to school every day.", options:["Go","Going","Gone","Goes"], answer:"Go"},
  {q:"Choose the correct word: He is ____ than me.", options:["Tall","Taller","Tallest","More tall"], answer:"Taller"},
  {q:"Past tense of 'Run':", options:["Runned","Ran","Run","Running"], answer:"Ran"},
  {q:"Plural of 'Child':", options:["Childs","Children","Childes","Child"], answer:"Children"},
  {q:"Which is a noun?", options:["Run","Happiness","Quickly","Beautiful"], answer:"Happiness"},
  {q:"Which is a verb?", options:["Book","Write","Blue","Beautiful"], answer:"Write"},
  {q:"Choose the article: I saw ____ elephant.", options:["a","an","the","none"], answer:"an"},
  {q:"Correct preposition: He is good ____ math.", options:["in","at","on","for"], answer:"at"},
  {q:"Synonym of 'Big':", options:["Small","Tiny","Large","Little"], answer:"Large"},
  {q:"Antonym of 'Hot':", options:["Cold","Warm","Cool","Boiling"], answer:"Cold"},
  {q:"Choose correct: She ____ playing.", options:["Is","Are","Am","Be"], answer:"Is"},
  {q:"Choose correct: They ____ happy.", options:["Is","Are","Am","Be"], answer:"Are"},
  {q:"Past tense of 'Eat':", options:["Eat","Eaten","Ate","Eating"], answer:"Ate"},
  {q:"Plural of 'Mouse':", options:["Mouses","Mice","Mices","Mouse"], answer:"Mice"},
  {q:"Synonym of 'Fast':", options:["Quick","Slow","Late","Lazy"], answer:"Quick"},
  {q:"Antonym of 'Strong':", options:["Weak","Powerful","Hard","Tough"], answer:"Weak"},
  {q:"Choose correct: I ____ a book now.", options:["Read","Reads","Am reading","Reading"], answer:"Am reading"},
  {q:"Past tense of 'Go':", options:["Went","Gone","Go","Going"], answer:"Went"},
  {q:"Synonym of 'Beautiful':", options:["Ugly","Pretty","Bad","Good"], answer:"Pretty"},
  {q:"Antonym of 'Light':", options:["Dark","Bright","Heavy","Small"], answer:"Dark"},
  {q:"Plural of 'Tooth':", options:["Tooths","Teeth","Toothes","Toothe"], answer:"Teeth"},
  {q:"Choose the correct pronoun: ____ is my friend.", options:["He","Him","His","Her"], answer:"He"},
  {q:"Which is an adjective?", options:["Run","Beautiful","Quickly","Happiness"], answer:"Beautiful"},
  {q:"Fill in: I have ____ apple.", options:["a","an","the","none"], answer:"an"},
  {q:"Choose correct: She ____ not like ice cream.", options:["Do","Does","Did","Doing"], answer:"Does"},
  {q:"Past tense of 'See':", options:["Saw","Seen","See","Seeing"], answer:"Saw"},
  {q:"Plural of 'Leaf':", options:["Leafs","Leaves","Leafes","Leavs"], answer:"Leaves"},
  {q:"Synonym of 'Angry':", options:["Mad","Happy","Sad","Joyful"], answer:"Mad"},
  {q:"Antonym of 'Old':", options:["Young","Ancient","Aged","Elder"], answer:"Young"},
  {q:"Choose correct: He ____ his homework.", options:["Do","Does","Did","Doing"], answer:"Did"},
  {q:"Past tense of 'Take':", options:["Taken","Took","Take","Taking"], answer:"Took"},
  {q:"Plural of 'Woman':", options:["Womans","Women","Womene","Womanses"], answer:"Women"},
  {q:"Synonym of 'Begin':", options:["Start","End","Stop","Finish"], answer:"Start"},
  {q:"Antonym of 'Full':", options:["Empty","Complete","Filled","Plenty"], answer:"Empty"},
  {q:"Choose correct: I ____ reading a book.", options:["Am","Is","Are","Be"], answer:"Am"},
  {q:"Past tense of 'Write':", options:["Wrote","Written","Write","Writing"], answer:"Wrote"},
  {q:"Plural of 'City':", options:["Citys","Cities","Citiyes","Cites"], answer:"Cities"},
  {q:"Synonym of 'Help':", options:["Assist","Hurt","Block","Stop"], answer:"Assist"},
  {q:"Antonym of 'Open':", options:["Close","Shut","Locked","All"], answer:"Close"},
  {q:"Fill in: She is taller ____ me.", options:["Than","Then","When","Who"], answer:"Than"},
  {q:"Choose correct: I ____ been there.", options:["Have","Has","Had","Haves"], answer:"Have"},
  {q:"Past tense of 'Drink':", options:["Drank","Drunk","Drink","Drinks"], answer:"Drank"},
  {q:"Plural of 'Child':", options:["Childs","Children","Childes","Child"], answer:"Children"},
  {q:"Synonym of 'Cold':", options:["Hot","Chilly","Warm","Boiling"], answer:"Chilly"},
  {q:"Antonym of 'Fast':", options:["Slow","Quick","Rapid","Swift"], answer:"Slow"},
  {q:"Choose correct: They ____ playing football.", options:["Is","Are","Am","Be"], answer:"Are"},
  {q:"Past tense of 'Buy':", options:["Bought","Buyed","Buying","Buys"], answer:"Bought"},
  {q:"Plural of 'Foot':", options:["Foots","Feet","Feets","Fut"], answer:"Feet"},
  {q:"Synonym of 'Sad':", options:["Happy","Unhappy","Joyful","Glad"], answer:"Unhappy"},
  {q:"Antonym of 'Soft':", options:["Hard","Smooth","Gentle","Light"], answer:"Hard"}
];

const filipinoQuestions = [
  {q:"Ano ang kasingkahulugan ng 'Mabilis'?", options:["Matulin","Mabagal","Matahimik","Mabigat"], answer:"Matulin"},
  {q:"Ano ang salitang kabaligtaran ng 'Mataas'?", options:["Mababa","Maliit","Mahina","Malakas"], answer:"Mababa"},
  {q:"Piliin ang wastong pangungusap:", options:["Ako ay mag-aaral bukas.","Mag-aaral ako bukas.","Bukas ako mag-aaral.","Mag-aaral bukas ako."], answer:"Ako ay mag-aaral bukas."},
  {q:"Ano ang hulapi sa salitang 'Ganda'?", options:["-an","-in","-ng","-i"], answer:"-ng"},
  {q:"Ano ang kasingkahulugan ng 'Masaya'?", options:["Malungkot","Masigla","Malakas","Mabilis"], answer:"Masigla"},
  {q:"Piliin ang wastong pangungusap sa panuto:", options:["Magluto ka ng pagkain.","Ka magluto pagkain.","Magluto pagkain ka.","Ikaw magluto ng pagkain."], answer:"Magluto ka ng pagkain."},
  {q:"Ano ang tamang panghalip sa: Si Ana at ako ay pupunta sa palengke. ____ ay masaya.", options:["Kami","Sila","Ako","Ikaw"], answer:"Kami"},
  {q:"Piliin ang tamang anyo ng pandiwa: Kumain siya ng mansanas.", options:["Kumain","Kumakain","Kakain","Kinain"], answer:"Kumain"},
  {q:"Ano ang kabaligtaran ng 'Mabuti'?", options:["Masama","Maganda","Malakas","Maliit"], answer:"Masama"},
  {q:"Piliin ang wastong baybay: ____ ng mga bata sa parke.", options:["Naglaro","Nag laro","Naglaro","Nagl arou"], answer:"Naglaro"},
  {q:"Ano ang kasingkahulugan ng 'Malakas'?", options:["Mabigat","Matatag","Mahina","Maliit"], answer:"Matatag"},
  {q:"Pumili ng panghalip na pamatlig: ____ bahay ay akin.", options:["Ang","Si","Ito","Iyan"], answer:"Ang"},
  {q:"Ano ang wastong anyo ng pandiwang: Ako ay ____ ng libro.", options:["Nagbasa","Nabasa","Babasa","Binasa"], answer:"Nagbasa"},
  {q:"Ano ang kasingkahulugan ng 'Matalino'?", options:["Magaling","Bobo","Mabagal","Mahina"], answer:"Magaling"},
  {q:"Pumili ng wastong pangungusap: ____ ay maganda.", options:["Si Maria","Maria si","Si si Maria","Maria si si"], answer:"Si Maria"},
  {q:"Ano ang kabaligtaran ng 'Mahaba'?", options:["Maikli","Maliit","Mababa","Mabigat"], answer:"Maikli"},
  {q:"Ano ang wastong baybay: ____ siya sa eskwela.", options:["Pumunta","Pumuntae","Pumuntaa","Pumuntah"], answer:"Pumunta"},
  {q:"Ano ang kasingkahulugan ng 'Malungkot'?", options:["Masaya","Maligalig","Maligalig","Malungkot"], answer:"Maligalig"},
  {q:"Piliin ang tamang panghalip panao: ____ ay mag-aaral.", options:["Ako","Siya","Kami","Sila"], answer:"Ako"},
  {q:"Ano ang tamang anyo ng pandiwa: Nagluto siya ng adobo.", options:["Nagluto","Magluluto","Nagluluto","Magluto"], answer:"Nagluto"},
  {q:"Ano ang kabaligtaran ng 'Mabagal'?", options:["Mabilis","Malakas","Maliksi","Mahina"], answer:"Mabilis"},
  {q:"Pumili ng wastong pangungusap: ____ ay kumain ng mangga.", options:["Si Juan","Juan si","Si si Juan","Juan si si"], answer:"Si Juan"},
  {q:"Ano ang kasingkahulugan ng 'Malinis'?", options:["Marumi","Maayos","Madumi","Masama"], answer:"Maayos"},
  {q:"Pumili ng wastong pangungusap: Ako ____ sa palengke.", options:["Pumunta","Pumuntae","Pumuntaa","Pumuntah"], answer:"Pumunta"},
  {q:"Ano ang kabaligtaran ng 'Maliit'?", options:["Malaki","Mahaba","Mabigat","Mababa"], answer:"Malaki"},
  {q:"Ano ang tamang anyo ng pandiwa: Kumanta siya ng kanta.", options:["Kumanta","Kumakanta","Kakanta","Kinanta"], answer:"Kumanta"},
  {q:"Ano ang kasingkahulugan ng 'Masigla'?", options:["Masaya","Malungkot","Malakas","Mahina"], answer:"Masaya"},
  {q:"Pumili ng tamang pangungusap: ____ ay maganda ang tanawin.", options:["Ang bundok","Bundok ang","Ang ang bundok","Bundok ang ang"], answer:"Ang bundok"},
  {q:"Ano ang kabaligtaran ng 'Mahina'?", options:["Malakas","Mabagal","Maliit","Mahaba"], answer:"Malakas"},
  {q:"Ano ang wastong baybay: ____ ng bata ang bola.", options:["Habulin","Haboolin","Habulinn","Habulin"], answer:"Habulin"},
  {q:"Ano ang kasingkahulugan ng 'Mabuti'?", options:["Masama","Magaling","Mahina","Malakas"], answer:"Magaling"},
  {q:"Pumili ng tamang panghalip: ____ ay mag-aaral ng mabuti.", options:["Siya","Ako","Kami","Sila"], answer:"Siya"},
  {q:"Ano ang tamang anyo ng pandiwa: Nag-aral siya ng leksyon.", options:["Nag-aral","Mag-aaral","Nag-aaral","Mag-aral"], answer:"Nag-aral"},
  {q:"Ano ang kabaligtaran ng 'Mabigat'?", options:["Magaan","Malakas","Malaki","Mahina"], answer:"Magaan"},
  {q:"Pumili ng wastong pangungusap: ____ ay naglalaro sa parke.", options:["Ang bata","Bata ang","Ang ang bata","Bata ang ang"], answer:"Ang bata"},
  {q:"Ano ang kasingkahulugan ng 'Matalas'?", options:["Mabalahibo","Matalino","Malabo","Malinis"], answer:"Matalino"},
  {q:"Pumili ng wastong panghalip panao: ____ ay nagluto ng hapunan.", options:["Siya","Ako","Kami","Sila"], answer:"Siya"},
  {q:"Ano ang kabaligtaran ng 'Mainit'?", options:["Malamig","Maliit","Mahina","Malakas"], answer:"Malamig"},
  {q:"Ano ang tamang anyo ng pandiwa: Naglakad siya sa kalsada.", options:["Naglakad","Maglalakad","Naglalakad","Maglakad"], answer:"Naglakad"},
  {q:"Ano ang kasingkahulugan ng 'Maganda'?", options:["Marumi","Magaling","Kaakit-akit","Mabaho"], answer:"Kaakit-akit"},
  {q:"Pumili ng wastong pangungusap: ____ ay pumunta sa tindahan.", options:["Si Pedro","Pedro si","Si si Pedro","Pedro si si"], answer:"Si Pedro"},
  {q:"Ano ang kabaligtaran ng 'Bukas'?", options:["Sarado","Maliit","Mahina","Malakas"], answer:"Sarado"},
  {q:"Ano ang tamang baybay: ____ ng guro ang leksyon.", options:["Ituro","Iturro","Iturro","Ituro"], answer:"Ituro"},
  {q:"Ano ang kasingkahulugan ng 'Mabilis'?", options:["Mabagal","Matulin","Malakas","Mahina"], answer:"Matulin"},
  {q:"Pumili ng tamang panghalip: ____ ay naglalaro ng basketball.", options:["Sila","Siya","Ako","Kami"], answer:"Sila"},
  {q:"Ano ang tamang anyo ng pandiwa: Kumain siya ng kanin.", options:["Kumain","Kumakain","Kakain","Kinain"], answer:"Kumain"},
  {q:"Ano ang kabaligtaran ng 'Mahaba'?", options:["Maikli","Maliit","Malaki","Mababa"], answer:"Maikli"},
  {q:"Pumili ng wastong pangungusap: ____ ay nag-aral ng mabuti.", options:["Si Maria","Maria si","Si si Maria","Maria si si"], answer:"Si Maria"},
  {q:"Ano ang kasingkahulugan ng 'Malakas'?", options:["Matatag","Mahina","Maliit","Mahaba"], answer:"Matatag"},
  {q:"Pumili ng tamang panghalip panao: ____ ay pupunta sa eskwela.", options:["Kami","Sila","Siya","Ako"], answer:"Kami"}
];

const genQuestions = [
  {q:"What is the capital of the Philippines?", options:["Manila","Cebu","Davao","Quezon City"], answer:"Manila"},
  {q:"Who is the national hero of the Philippines?", options:["Jose Rizal","Andres Bonifacio","Emilio Aguinaldo","Apolinario Mabini"], answer:"Jose Rizal"},
  {q:"What is the national fruit of the Philippines?", options:["Mango","Banana","Pineapple","Durian"], answer:"Mango"},
  {q:"Which Philippine volcano erupted in 1991?", options:["Mount Pinatubo","Mayon","Taal","Bulusan"], answer:"Mount Pinatubo"},
  {q:"Which city is known as the 'Queen City of the South'?", options:["Cebu","Manila","Davao","Baguio"], answer:"Cebu"},
  {q:"Who wrote the Philippine national anthem?", options:["Jose Palma","Julian Felipe","Francisco Balagtas","Antonio Luna"], answer:"Jose Palma"},
  {q:"What is the longest river in the Philippines?", options:["Cagayan River","Pasig River","Agusan River","Mindanao River"], answer:"Cagayan River"},
  {q:"Which Philippine hero started the Katipunan?", options:["Andres Bonifacio","Jose Rizal","Emilio Aguinaldo","Marcelo H. del Pilar"], answer:"Andres Bonifacio"},
  {q:"What is the smallest province in the Philippines?", options:["Batanes","Palawan","Cebu","Quezon"], answer:"Batanes"},
  {q:"Which Philippine island is known for the Chocolate Hills?", options:["Bohol","Negros","Palawan","Leyte"], answer:"Bohol"},
  {q:"What is the national bird of the Philippines?", options:["Philippine Eagle","Maya","Kingfisher","Parrot"], answer:"Philippine Eagle"},
  {q:"Which region is called the 'Summer Capital of the Philippines'?", options:["Baguio","Tagaytay","Cebu","Davao"], answer:"Baguio"},
  {q:"What is the national flower of the Philippines?", options:["Sampaguita","Rosal","Gumamela","Orchid"], answer:"Sampaguita"},
  {q:"Which Philippine president declared Martial Law in 1972?", options:["Ferdinand Marcos","Corazon Aquino","Ramon Magsaysay","Manuel Quezon"], answer:"Ferdinand Marcos"},
  {q:"Which Philippine island is the largest?", options:["Luzon","Mindanao","Visayas","Palawan"], answer:"Luzon"},
  {q:"Who is the 'Father of the Philippine Revolution'?", options:["Andres Bonifacio","Jose Rizal","Emilio Aguinaldo","Apolinario Mabini"], answer:"Andres Bonifacio"},
  {q:"What is the main language spoken in Davao?", options:["Cebuano","Tagalog","Ilocano","Hiligaynon"], answer:"Cebuano"},
  {q:"Which Philippine province is famous for its Banaue Rice Terraces?", options:["Ifugao","Benguet","Nueva Vizcaya","Mountain Province"], answer:"Ifugao"},
  {q:"Who was the first president of the Philippines?", options:["Emilio Aguinaldo","Manuel Quezon","Jose P. Laurel","Sergio Osmena"], answer:"Emilio Aguinaldo"},
  {q:"Which Philippine city is known for its Sinulog Festival?", options:["Cebu","Manila","Davao","Iloilo"], answer:"Cebu"},
  {q:"Which Philippine island is called the 'Paradise of the South'?", options:["Palawan","Cebu","Bohol","Negros"], answer:"Palawan"},
  {q:"Which Philippine hero was executed in Luneta (Rizal Park)?", options:["Jose Rizal","Andres Bonifacio","Marcelo H. del Pilar","Apolinario Mabini"], answer:"Jose Rizal"},
  {q:"What is the national sport of the Philippines?", options:["Arnis","Basketball","Boxing","Sepak Takraw"], answer:"Arnis"},
  {q:"Which Philippine region is known for the Ifugao Rice Terraces?", options:["Cordillera","Cagayan Valley","Ilocos","Central Luzon"], answer:"Cordillera"},
  {q:"What is the official currency of the Philippines?", options:["Peso","Dollar","Yen","Ringgit"], answer:"Peso"},
  {q:"Which Philippine province is called the 'Surfing Capital of the Philippines'?", options:["Siargao","Cebu","Bicol","Leyte"], answer:"Siargao"},
  {q:"Which city is known as the 'City of Smiles' in the Philippines?", options:["Davao","Cebu","Manila","Baguio"], answer:"Davao"},
  {q:"What is the main ingredient in adobo?", options:["Chicken or Pork","Beef","Fish","Vegetables"], answer:"Chicken or Pork"},
  {q:"Which Philippine province is famous for its white sand beaches in Boracay?", options:["Aklan","Palawan","Cebu","Bohol"], answer:"Aklan"},
  {q:"Who is the author of the novel 'Noli Me Tangere'?", options:["Jose Rizal","Andres Bonifacio","Marcelo H. del Pilar","Emilio Aguinaldo"], answer:"Jose Rizal"},
  {q:"Which Philippine hero was known as the 'Brains of the Revolution'?", options:["Apolinario Mabini","Jose Rizal","Andres Bonifacio","Emilio Aguinaldo"], answer:"Apolinario Mabini"},
  {q:"Which Philippine festival is celebrated with colorful masks and dances in Bacolod?", options:["MassKara Festival","Sinulog","Panagbenga","Kadayawan"], answer:"MassKara Festival"},
  {q:"What is the largest lake in the Philippines?", options:["Laguna de Bay","Taal Lake","Lake Lanao","Lake Sebu"], answer:"Laguna de Bay"},
  {q:"Which Philippine president was the first woman to lead the country?", options:["Corazon Aquino","Gloria Macapagal Arroyo","Imelda Marcos","Leni Robredo"], answer:"Corazon Aquino"},
  {q:"Which Philippine city is known as the 'Gateway to Mindanao'?", options:["Davao","Cagayan de Oro","Zamboanga","General Santos"], answer:"Cagayan de Oro"},
  {q:"Which Philippine island is known for the Underground River?", options:["Palawan","Bohol","Cebu","Negros"], answer:"Palawan"},
  {q:"What is the traditional Filipino martial art?", options:["Arnis","Karate","Taekwondo","Judo"], answer:"Arnis"},
  {q:"Which Philippine province is known as the 'Summer Capital'?", options:["Benguet","Cebu","Palawan","Leyte"], answer:"Benguet"},
  {q:"Which Philippine volcano is located in Batangas?", options:["Taal","Mayon","Pinatubo","Bulusan"], answer:"Taal"},
  {q:"Which Philippine hero was known as the 'Father of the Philippine Revolution'?", options:["Andres Bonifacio","Jose Rizal","Emilio Aguinaldo","Marcelo H. del Pilar"], answer:"Andres Bonifacio"},
  {q:"Which city is called the 'Heart of the Philippines'?", options:["Davao","Cebu","Manila","Iloilo"], answer:"Iloilo"},
  {q:"Which Philippine city is famous for its lechon?", options:["Cebu","Manila","Davao","Bacolod"], answer:"Cebu"},
  {q:"Which Philippine region is called the 'Land of the Rising Sun' in the country?", options:["Eastern Visayas","Ilocos","Cordillera","Cagayan Valley"], answer:"Eastern Visayas"},
  {q:"Which Philippine island group is known as the Visayas?", options:["Central Philippines","Luzon","Mindanao","Palawan"], answer:"Central Philippines"},
  {q:"What is the national dance of the Philippines?", options:["Tinikling","Cariñosa","Singkil","Pandanggo sa Ilaw"], answer:"Tinikling"},
  {q:"Which Philippine city is known for its giant durian fruit?", options:["Davao","Cebu","Baguio","Iloilo"], answer:"Davao"},
  {q:"Which Philippine hero was a key figure in the Propaganda Movement?", options:["Marcelo H. del Pilar","Jose Rizal","Andres Bonifacio","Emilio Aguinaldo"], answer:"Marcelo H. del Pilar"},
  {q:"Which Philippine province is famous for its Mayon Volcano?", options:["Albay","Bohol","Cebu","Palawan"], answer:"Albay"},
  {q:"Which Philippine region is known as the Cordilleras?", options:["Northern Luzon","Central Luzon","Visayas","Mindanao"], answer:"Northern Luzon"}
];



function shuffleOptions(opts){
  return opts.map(value => ({value, sort: Math.random()}))
             .sort((a,b) => a.sort - b.sort)
             .map(({value}) => value);
}

function nextMessage(){
  document.getElementById('mess1').style.display='block';
  document.getElementById('mess1').innerText = "We would appreciate it if you could tell us your name.";
  document.querySelector('.btn1').style.display = 'none';
  document.getElementById('nameInput').style.display = 'inline';
  document.getElementById('submitName').style.display = 'inline';
}

function submitName(){
  const name = document.getElementById('nameInput').value;
  if(name.trim()!==""){
    playerName = name;
    openMainMenu();
  } else { alert("Please enter your name."); }
}

// --- MAIN MENU ---
function openMainMenu(){
  document.getElementById('mess1').style.display='block';
  document.getElementById('message2').style.display='none';
  document.getElementById('nameInput').style.display='none';
  document.getElementById('submitName').style.display='none';
  document.getElementById('btn2').style.display='none';
  document.querySelector('.btn1').style.display='none';

  document.getElementById('mess1').innerHTML = `<strong>Hi ${playerName}, Welcome to Quiz Adventure!</strong>\n\nChoose a Subject:`;

  let subjects = `
    <button class="menuBtn" onclick="startSubject('science')">Science</button>
    <button class="menuBtn" onclick="startSubject('math')">Math</button>
    <button class="menuBtn" onclick="startSubject('english')">English</button>
    <button class="menuBtn" onclick="startSubject('filipino')">Filipino</button>
    <button class="menuBtn" onclick="startSubject('general')">General Knowledge</button>

  `;
  document.getElementById('mess1').innerHTML += subjects;
}

function startSubject(subject){
  selectedSubject = subject;
  
    if(subject === "science"){
    openGame(scienceQuestions);
  } 

    else if(subject === "math"){
    openGame(mathQuestions);
  }

  
    else if(subject === "english"){
    openGame(englishQuestions);
  } 

     else if(subject === "filipino"){
    openGame(filipinoQuestions);
  } 

    else if(subject === "general"){
    openGame(genQuestions);
  } 





}

// --- GAME START ---
function openGame(questionSet){
  document.getElementById('mess1').style.display='none';
  document.getElementById('message2').style.display='none';
  document.getElementById('startGameBtn').style.display='none';
  document.getElementById('gameWindow').style.display='block';
  currentQuestion = 0;
  score = 0;
  totalLevels = 0;
  window.currentQuestions = questionSet; 
  showQuestion();
  showExitButton();
}

function showQuestion(){
  const q = window.currentQuestions[currentQuestion % window.currentQuestions.length]; 
  const shuffled = shuffleOptions(q.options);
  totalLevels++;
  let html = `<div id="level">Level ${totalLevels}</div>`;
  html+=`<p>${q.q}</p>`;
  shuffled.forEach(opt=>{
    html+=`<button class="optionBtn" id="option${opt}" onclick="checkAnswer('${opt}')">${opt}</button>`;
  });
  html+=`<div id="feedback"></div>`;
  html+=`<div id="scoreBoard">Score: ${score}</div>`;
  html+=`<button id="nextBtn" onclick="nextQuestion()" style="display:none;">Next</button>`;
  document.getElementById('gameWindow').innerHTML=html;
  showExitButton();
}

function checkAnswer(selected){
  const q = window.currentQuestions[currentQuestion % window.currentQuestions.length];
  const optionButtons=document.querySelectorAll(".optionBtn");
  const feedback=document.getElementById('feedback');
  optionButtons.forEach(btn=>{
    btn.disabled=true;
    if(btn.innerText===q.answer) btn.classList.add("correct");
  });
  const chosenBtn=document.getElementById("option"+selected);
  if(selected===q.answer){
    feedback.innerText="Correct!";
    feedback.style.color="green";
    score++;
  } else {
    chosenBtn.classList.add("wrong");
    feedback.innerText="Wrong!";
    feedback.style.color="red";
  }
  document.getElementById('scoreBoard').innerText=`Score: ${score}`;
  document.getElementById('nextBtn').style.display='inline';
}

function nextQuestion(){
  currentQuestion++;
  if(currentQuestion < window.currentQuestions.length){
    showQuestion();
  } else {
    endGame();
  }
}

function showExitButton() {
  if(document.getElementById('exitBtn')) return;
  const exitBtn = document.createElement("button");
  exitBtn.id = "exitBtn";
  exitBtn.textContent = "Exit Game";
  exitBtn.onclick = confirmExit;
  document.getElementById('gameWindow').appendChild(exitBtn);
}

function confirmExit() {
  if(confirm("Are you sure you want to exit the game?")) {
    endGame();
  }
}

function endGame() {
  document.getElementById('gameWindow').style.display = 'none';
  document.getElementById('mess1').style.display = 'block';
  document.getElementById('mess1').innerText = `🎉 Congratulations, ${playerName}! 🎉\nYour total score is: ${score}/${window.currentQuestions.length}`;
  const restartBtn = document.createElement("button");
  restartBtn.textContent = "Main Menu";
  restartBtn.onclick = openMainMenu;
  document.getElementById('mess1').appendChild(document.createElement("br"));
  document.getElementById('mess1').appendChild(restartBtn);
}
</script>
</head>
<body>
  <div id="container">
    <div id="mess1">Hello There Travelers!</div>
    <div id="message2"></div>
    <input type="button" class="btn1" value="Hello" onclick="nextMessage()">
    <input type="text" id="nameInput" placeholder="Enter your name" style="display:none;">
    <input type="button" id="submitName" value="Submit" onclick="submitName()" style="display:none;">
    <input type="button" id="btn2" value="Great Lets Start" onclick="startAdventure()" style="display:none;">
    <input type="button" id="startGameBtn" value="Take me to the Game" onclick="openMainMenu()" style="display:none;">
    <div id="gameWindow"></div>
  </div>
</body>
</html>
