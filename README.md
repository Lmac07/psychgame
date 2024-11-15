<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Psychology Wordle Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f3f3f3;
            flex-direction: column;
        }
        h1 {
            color: #333;
        }
        #game-board {
            display: grid;
            gap: 5px;
            margin-top: 20px;
            justify-content: center;
        }
        .tile {
            width: 50px;
            height: 50px;
            font-size: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 2px solid #ddd;
            text-transform: uppercase;
            font-weight: bold;
        }
        .correct { background-color: green; color: white; border-color: green; }
        .misplaced { background-color: yellow; color: black; border-color: yellow; }
        .incorrect { background-color: gray; color: white; border-color: gray; }
        #controls {
            margin-top: 20px;
        }
        #guess-input {
            width: 150px;
            height: 30px;
            text-align: center;
            font-size: 18px;
        }
        #feedback, #definition {
            margin-top: 20px;
            font-size: 18px;
        }
        #definition {
            display: none;
            color: #555;
        }
    </style>
</head>
<body>
    <h1>Psychology Wordle Game</h1>
    <div id="game-board"></div>
    <div id="controls">
        <input type="text" id="guess-input" maxlength="9" placeholder="Enter your guess">
        <button onclick="submitGuess()">Submit</button>
    </div>
    <p id="feedback"></p>
    <p id="definition"></p>

    <script>
        const wordList = [
            "anxiety", "trauma", "emotion", "empathy", "apathy", "phobia", "behavior", "cognition", "memory",
            "delirium", "autism", "narcissist", "repress", "impulse", "denial", "persona", "therapy", "hysteria",
            "paranoia", "euphoria", "psychotic", "stress", "dementia", "bipolar", "hallucinate", "obsessive",
            "amnesia", "attachment", "compulsion", "desensitize", "projection", "somatic", "insomnia", "schizoid",
            "placebo", "catharsis", "conform", "freud", "pavlov", "maslow", "introvert", "extrovert", "psychosis",
            "superego", "syndrome", "neurosis", "fixation", "dyslexia", "mechanism", "disorder"
        ];

        const definitions = {
            "anxiety": "A mental state of worry or fear that impacts well-being.",
            "trauma": "A distressing experience with lasting psychological effects.",
            "emotion": "A psychological state involving subjective experience.",
            "empathy": "The ability to understand and share another's feelings.",
            "apathy": "Lack of interest or concern, often linked to depression.",
            "phobia": "An intense, irrational fear of specific objects or situations.",
            "behavior": "Actions or reactions, studied to understand mental states.",
            "cognition": "The mental processes of understanding and knowledge.",
            "memory": "The mental function of storing and recalling information.",
            "delirium": "A mental state of confusion, often with hallucinations.",
            "autism": "A developmental disorder impacting social skills and communication.",
            "narcissist": "A person with an excessive focus on themselves.",
            "repress": "To unconsciously push away unwanted feelings or thoughts.",
            "impulse": "A sudden urge to act, connected to behavior control.",
            "denial": "A defense mechanism where one refuses to accept reality.",
            "persona": "The social role or identity one presents to the world.",
            "therapy": "Treatment intended to relieve or heal mental health conditions.",
            "hysteria": "A psychological state of uncontrollable emotion or panic.",
            "paranoia": "Excessive suspicion or mistrust, associated with various mental disorders.",
            "euphoria": "A feeling of intense excitement or happiness.",
            "psychotic": "Relating to or affected by a loss of connection with reality.",
            "stress": "Mental or emotional strain from challenging situations.",
            "dementia": "A decline in cognitive ability severe enough to interfere with life.",
            "bipolar": "A mood disorder marked by alternating periods of mania and depression.",
            "hallucinate": "Experiencing sensations that aren't present.",
            "obsessive": "Excessive preoccupation with a thought or idea.",
            "amnesia": "A loss of memory that may be temporary or permanent.",
            "attachment": "A strong emotional bond to someone or something.",
            "compulsion": "An irresistible urge to perform certain actions.",
            "desensitize": "Reducing emotional sensitivity to a stimulus over time.",
            "projection": "Attributing one's own feelings or thoughts to others.",
            "somatic": "Physical symptoms caused or influenced by the mind.",
            "insomnia": "Difficulty falling or staying asleep, affecting mental health.",
            "schizoid": "A personality disorder marked by emotional detachment.",
            "placebo": "A substance with no therapeutic effect, used as a control in testing.",
            "catharsis": "Emotional release that relieves tension or distress.",
            "conform": "To act in accordance with socially accepted standards.",
            "freud": "Reference to Sigmund Freud, who developed psychoanalysis.",
            "pavlov": "Refers to Ivan Pavlov, known for classical conditioning theory.",
            "maslow": "Refers to Abraham Maslow, known for the hierarchy of needs.",
            "introvert": "A person more focused on internal thoughts and feelings.",
            "extrovert": "A person more oriented toward the external world.",
            "psychosis": "A mental disorder marked by disconnection from reality.",
            "superego": "The part of the mind reflecting societal standards and morals.",
            "syndrome": "A group of symptoms that occur together.",
            "neurosis": "A mild mental disorder causing distress but not delusions.",
            "fixation": "An obsessive focus or attachment to a particular object or idea.",
            "dyslexia": "A learning disorder characterized by difficulties with reading.",
            "mechanism": "A mental process that protects against stress or trauma.",
            "disorder": "A disruption in normal physical or mental function."
        };

        const targetWord = wordList[Math.floor(Math.random() * wordList.length)];
        const wordLength = targetWord.length;
        let attempts = 0;

        document.getElementById("game-board").style.gridTemplateColumns = `repeat(${wordLength}, 1fr)`;

        function submitGuess() {
            const guessInput = document.getElementById("guess-input");
            const guess = guessInput.value.toLowerCase();
            if (guess.length !== wordLength) {
                document.getElementById("feedback").innerText = `Enter a ${wordLength}-letter word.`;
                return;
            }

            attempts++;
            const feedback = [];
            for (let i = 0; i < guess.length; i++) {
                const letter = guess[i];
                if (letter === targetWord[i]) {
                    feedback.push("correct");
                } else if (targetWord.includes(letter)) {
                    feedback.push("misplaced");
                } else {
                    feedback.push("incorrect");
                }
            }

            displayFeedback(guess, feedback);

            if (guess === targetWord) {
                document.getElementById("feedback").innerText = `Congratulations! You guessed the word in ${attempts} attempts!`;
                document.getElementById("definition").innerText = `Definition: ${definitions[targetWord]}`;
                document.getElementById("definition").style.display = "block";
            } else if (attempts >= 6) {
                document.getElementById("feedback").innerText = `Game over! The word was: ${targetWord}`;
                document.getElementById("definition").innerText = `Definition: ${definitions[targetWord]}`;
                document.getElementById("definition").style.display = "block";
            } else {
                guessInput.value = "";
                document.getElementById("feedback").innerText = `Attempt ${attempts}/6: Try again!`;
            }
        }

        function displayFeedback(guess, feedback) {
            const board = document.getElementById("game-board");
            const row = document.createElement("div");

            for (let i = 0; i < guess.length; i++) {
                const tile = document.createElement("div");
                tile.classList.add("tile", feedback[i]);
                tile.innerText = guess[i];
                row.appendChild(tile);
            }

            board.appendChild(row);
        }
    </script>
</body>
</html>
