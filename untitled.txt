<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JARVIS Mobile AI</title>
    <style>
        /* CSS Styling - Jarvis Aesthetic */
        body {
            background-color: #050505;
            color: #00d4ff;
            font-family: 'Courier New', Courier, monospace;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
        }

        #container {
            text-align: center;
            width: 100%;
        }

        #arc-reactor {
            width: 180px;
            height: 180px;
            border-radius: 50%;
            border: 4px solid #00d4ff;
            box-shadow: 0 0 25px #00d4ff, inset 0 0 25px #00d4ff;
            margin: 0 auto;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            position: relative;
            background: radial-gradient(circle, rgba(0,212,255,0.1) 0%, rgba(0,0,0,1) 70%);
        }

        .active {
            animation: pulse 1.2s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); box-shadow: 0 0 25px #00d4ff; }
            50% { transform: scale(1.1); box-shadow: 0 0 60px #00d4ff; }
            100% { transform: scale(1); box-shadow: 0 0 25px #00d4ff; }
        }

        h1 { font-size: 1.5rem; letter-spacing: 5px; margin-top: 30px; }
        #status { color: #888; font-size: 0.9rem; margin-bottom: 20px; }
        #transcript { 
            color: white; 
            margin-top: 20px; 
            min-height: 50px; 
            padding: 0 20px;
            font-size: 1.1rem;
        }
    </style>
</head>
<body>

    <div id="container">
        <div id="arc-reactor" onclick="startJarvis()">
            <span>POWER ON</span>
        </div>
        <h1>JARVIS</h1>
        <div id="status">STANDING BY</div>
        <div id="transcript">Tap the reactor to speak...</div>
    </div>

    <script>
        // JavaScript - The Brain
        const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
        const synth = window.speechSynthesis;
        
        const reactor = document.getElementById('arc-reactor');
        const statusText = document.getElementById('status');
        const transcriptText = document.getElementById('transcript');

        recognition.lang = 'en-US';
        recognition.interimResults = false;

        function startJarvis() {
            recognition.start();
            reactor.classList.add('active');
            statusText.innerText = "LISTENING...";
        }

        recognition.onresult = (event) => {
            const command = event.results[0][0].transcript.toLowerCase();
            transcriptText.innerText = `You said: "${command}"`;
            handleCommand(command);
        };

        recognition.onend = () => {
            reactor.classList.remove('active');
            statusText.innerText = "STANDING BY";
        };

        function speak(text) {
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.rate = 1.0;
            utterance.pitch = 0.9;
            
            // Jarvis-like voice selection (optional)
            const voices = synth.getVoices();
            if (voices.length > 0) {
                // Try to find a male voice for Jarvis effect
                utterance.voice = voices.find(v => v.name.includes('Google US English Male')) || voices[0];
            }
            
            synth.speak(utterance);
        }

        function handleCommand(cmd) {
            if (cmd.includes('hello') || cmd.includes('jarvis')) {
                speak("Hello! Systems are online. How can I assist you today?");
            } else if (cmd.includes('time')) {
                const now = new Date();
                speak(`It is currently ${now.getHours()} ${now.getMinutes()}`);
            } else if (cmd.includes('date')) {
                speak(`Today is ${new Date().toDateString()}`);
            } else if (cmd.includes('who are you')) {
                speak("I am JARVIS, your personal digital assistant.");
            } else if (cmd.includes('clear')) {
                transcriptText.innerText = "";
                speak("Screen cleared.");
            } else {
                speak("Command received, but I don't have instructions for that yet.");
            }
        }

        // Fix for mobile voice load
        window.speechSynthesis.onvoiceschanged = () => {
            console.log("Voices loaded");
        };
    </script>
</body>
</html>
