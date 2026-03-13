
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Master Guessing Hub</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body id="body-bg" class="bg-slate-900 text-slate-100 min-h-screen flex items-center justify-center p-4 transition-colors">

  <div id="card" class="container mx-auto max-w-lg p-8 bg-slate-800 rounded-3xl shadow-2xl border border-slate-700 text-center">
    
    <div class="flex flex-wrap justify-between items-center gap-4 mb-8">
      <button onclick="toggleDarkMode()" class="bg-slate-700 px-4 py-2 rounded-xl text-xs font-bold uppercase hover:bg-slate-600 transition">
        Toggle Theme
      </button>
      <div class="flex gap-2">
        <button onclick="changeTheme('bg-blue-600', 'text-blue-400')" class="w-8 h-8 rounded-full bg-blue-500"></button>
        <button onclick="changeTheme('bg-emerald-600', 'text-emerald-400')" class="w-8 h-8 rounded-full bg-emerald-500"></button>
        <button onclick="changeTheme('bg-purple-600', 'text-purple-400')" class="w-8 h-8 rounded-full bg-purple-500"></button>
      </div>
    </div>

    <div class="flex justify-center gap-2 mb-8 bg-slate-900/50 p-2 rounded-2xl border border-slate-700">
      <button onclick="setGameMode('numbers')" class="flex-1 py-2 rounded-xl text-xs font-bold uppercase text-slate-300">Numbers</button>
      <button onclick="setGameMode('animals')" class="flex-1 py-2 rounded-xl text-xs font-bold uppercase text-slate-300">Animals</button>
      <button onclick="setGameMode('places')" class="flex-1 py-2 rounded-xl text-xs font-bold uppercase text-slate-300">Places</button>
    </div>

    <h1 id="title" class="text-4xl font-black mb-2 text-blue-400 uppercase">Guessing Hub</h1>
    <p id="hint" class="text-slate-400 text-sm mb-8 italic h-10">Select a category to start!</p>

    <div id="feedback" class="h-12 flex items-center justify-center mb-6 text-lg font-bold text-amber-400">
      Choose your challenge
    </div>

    <input type="text" id="userInput" placeholder="Type here..." 
           class="w-full bg-slate-700 border-2 border-slate-600 rounded-2xl py-4 px-6 text-2xl text-center focus:outline-none mb-6">

    <button id="main-btn" onclick="checkAnswer()" 
            class="w-full bg-blue-600 py-4 rounded-2xl font-black text-xl shadow-lg transition active:scale-95 text-white">
      SUBMIT ANSWER
    </button>
  </div>

  <script>
    const data = {
      numbers: { hint: "Guess a number between 1-20", list: [5, 12, 18, 3, 7, 15] },
      animals: { 
        list: [
          { name: "lion", hint: "I am a golden predator that roars." },
          { name: "elephant", hint: "I am a huge mammal with a long trunk." },
          { name: "shark", hint: "I have sharp teeth and live in the deep ocean." }
        ]
      },
      places: { 
        list: [
          { name: "paris", hint: "Home to the famous Eiffel Tower." },
          { name: "india", hint: "Known for the Taj Mahal and vibrant culture." },
          { name: "tokyo", hint: "A massive, neon-lit capital city in Japan." }
        ]
      }
    };

    let currentGameMode = 'numbers';
    let currentTarget = null;

    function setGameMode(mode) {
      currentGameMode = mode;
      pickNewTarget();
    }

    function pickNewTarget() {
      const pool = data[currentGameMode].list;
      currentTarget = pool[Math.floor(Math.random() * pool.length)];
      
      if (currentGameMode === 'numbers') {
        document.getElementById('hint').innerText = data.numbers.hint;
      } else {
        document.getElementById('hint').innerText = currentTarget.hint;
      }
      document.getElementById('feedback').innerText = "New question generated!";
    }

    function checkAnswer() {
      const input = document.getElementById('userInput');
      const feedback = document.getElementById('feedback');
      const val = input.value.trim().toLowerCase();
      let isCorrect = false;

      if (currentGameMode === 'numbers') {
        const num = parseInt(val);
        if (num === currentTarget) isCorrect = true;
        else feedback.innerText = num > currentTarget ? "TOO HIGH!" : "TOO LOW!";
      } else {
        if (val === currentTarget.name) isCorrect = true;
        else feedback.innerText = "WRONG! TRY AGAIN.";
      }
      
      if (isCorrect) {
        feedback.innerText = "CORRECT! NEW CHALLENGE!";
        feedback.className = "h-12 flex items-center justify-center mb-6 text-lg font-bold text-emerald-500";
        setTimeout(pickNewTarget, 1000);
      } else {
        feedback.className = "h-12 flex items-center justify-center mb-6 text-lg font-bold text-rose-500";
      }
      input.value = '';
    }

    function changeTheme(bgColor, textColor) {
      document.getElementById('main-btn').className = `w-full ${bgColor} py-4 rounded-2xl font-black text-xl shadow-lg transition active:scale-95 text-white`;
      document.getElementById('title').className = `text-4xl font-black mb-2 ${textColor} uppercase`;
    }

    function toggleDarkMode() {
      document.body.classList.toggle('bg-slate-900');
      document.body.classList.toggle('bg-slate-100');
      document.getElementById('card').classList.toggle('bg-slate-800');
      document.getElementById('card').classList.toggle('bg-white');
    }
  </script>
</body>
</html>
