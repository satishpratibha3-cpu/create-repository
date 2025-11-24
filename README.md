<!DOCTYPE html>
<html lang="mr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>सोलो प्रवासी: एआय संवाद गेम</title>
    <style>
        /* CSS (गेमची रचना) */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #e8e8e8;
            color: #333;
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
        }
        .game-container {
            width: 100%;
            max-width: 800px;
            background: #fff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 8px 16px rgba(0,0,0,0.2);
        }
        h2 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 25px;
        }
        #output {
            min-height: 250px;
            border: 2px solid #3498db;
            padding: 15px;
            margin-bottom: 20px;
            background-color: #ecf0f1;
            border-radius: 8px;
            overflow-y: auto;
            max-height: 400px;
        }
        #input-container {
            display: flex;
            gap: 10px;
        }
        #user-input {
            flex-grow: 1;
            padding: 12px;
            border: 1px solid #bdc3c7;
            border-radius: 6px;
            font-size: 16px;
        }
        button {
            padding: 12px 25px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #2980b9;
        }
        .char-dialogue {
            color: #16a085; /* पात्राचा संवाद (हिरवा-निळा) */
            font-weight: bold;
            margin: 8px 0;
            line-height: 1.5;
        }
        .user-dialogue {
            color: #e67e22; /* खेळाडूचा संवाद (नारंगी) */
            text-align: right;
            margin: 8px 0;
            line-height: 1.5;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h2>🌍 सोलो प्रवासी: एआय संवाद गेम</h2>
        <div id="output"></div>
        <div id="input-container">
            <input type="text" id="user-input" placeholder="तुमचा प्रश्न/संवाद टाइप करा (उदा. कसा आहेस, टोकियोबद्दल सांग)...">
            <button onclick="processInput()">पाठवा</button>
        </div>
    </div>

    <script>
        // JavaScript (गेम लॉजिक आणि AI सिम्युलेशन)

        const output = document.getElementById('output');
        const input = document.getElementById('user-input');

        // पात्राचे डेटाबेस आणि AI लॉजिक
        const characters = {
            'Guru': {
                location: 'टोकियो, जपान',
                charName: 'गुरू',
                greeting: "कोननिचीवा! मी गुरू, डिजिटल युगातील एक जुना मार्गदर्शक. तुम्ही काय जाणून घेऊ इच्छिता?",
                responses: {
                    // विशिष्ट कीवर्ड उत्तरे
                    'city': "टोकियो हे परंपरा आणि तंत्रज्ञान यांचा संगम असलेले शहर आहे. तुम्ही शिबुया क्रॉसिंगला भेट दिली आहे का?",
                    'tech': "तंत्रज्ञान हे केवळ एक साधन आहे. ते चांगले की वाईट, हे त्याचा वापर करणारा ठरवतो.",
                    'default': "मला माफ करा. तुमचा प्रश्न अधिक स्पष्ट करा. माझा AI भाग तुमच्या प्रश्नाचा अर्थ लावण्याचा प्रयत्न करत आहे."
                }
            }
        };

        let activeCharacter = characters['Guru']; // सध्याचा पात्र

        function printMessage(message, isCharacter = true) {
            const p = document.createElement('p');
            p.innerHTML = message;
            if (isCharacter) {
                p.className = 'char-dialogue';
            } else {
                p.className = 'user-dialogue';
            }
            output.appendChild(p);
            // सर्वात नवीन मेसेज दिसावा म्हणून स्क्रोल करणे
            output.scrollTop = output.scrollHeight;
        }

        // ⭐ हा 'AI' फंक्शन आहे जो प्रश्नांची उत्तरे देतो ⭐
        function getCharacterResponse(query) {
            const lowerQuery = query.toLowerCase().replace(/[?,.!] /g, ''); // विरामचिन्हे काढून टाका
            
            // 1. सामान्य आणि मूलभूत प्रश्न (General/Basic Questions):
            if (lowerQuery.includes('काय चालले आहे') || lowerQuery.includes('कसा आहेस')) {
                return "मी ठीक आहे. टोकियोमध्ये जीवन नेहमीप्रमाणे वेगवान आहे. तुम्ही कसा आहात?";
            }
            
            // 2. व्यक्तिगत माहिती (Personal Info):
            if (lowerQuery.includes('कोण आहेस') || lowerQuery.includes('तुझे नाव')) {
                return `मी ${activeCharacter.charName}, एक जुना मार्गदर्शक. माझा जन्म डेटाबेसमध्ये झाला आहे!`;
            }

            // 3. AI आणि गेमबद्दल प्रश्न:
            if (lowerQuery.includes('ai') || lowerQuery.includes('कृत्रिम बुद्धिमत्ता')) {
                return activeCharacter.responses['tech'];
            }
            
            // 4. ठिकाणाबद्दल प्रश्न:
            if (lowerQuery.includes('टोकियो') || lowerQuery.includes('शहर')) {
                return activeCharacter.responses['city'];
            }

            // 5. जगभरातील मूलभूत माहिती (World Knowledge Simulation):
            if (lowerQuery.includes('जगातील सर्वात उंच इमारत')) {
                return "माझ्या माहितीनुसार, जगातील सर्वात उंच इमारत बुर्ज खलिफा आहे. मी तुमच्या प्रश्नाची नोंद घेतली आहे.";
            }
            if (lowerQuery.includes('भारत')) {
                return "भारत हा संस्कृती आणि इतिहासाचा देश आहे. कोणता भाग तुम्हाला अधिक आकर्षित करतो?";
            }
            
            // 6. अनपेक्षित प्रश्न (Fallback):
            return activeCharacter.responses['default'];
        }

        function processInput() {
            const userQuery = input.value.trim();
            if (userQuery === "") return;

            // 1. खेळाडूचा संवाद प्रिंट करा
            printMessage(`**प्रवासी:** ${userQuery}`, false);

            // 2. पात्राकडून उत्तर मिळवा
            const characterResponse = getCharacterResponse(userQuery);

            // 3. पात्राचे उत्तर प्रिंट करा
            setTimeout(() => {
                printMessage(`**${activeCharacter.location} येथील ${activeCharacter.charName}:** ${characterResponse}`, true);
            }, 500); // 0.5 सेकंदाचा विलंब उत्तरासाठी

            // 4. इनपुट साफ करा
            input.value = '';
        }

        // गेम सुरू करताना (Initialization)
        window.onload = () => {
            printMessage(`**गेम सुरू:** तुम्ही ${activeCharacter.location} मध्ये आहात. तुम्हाला ${activeCharacter.charName} भेटले.`);
            printMessage(`**${activeCharacter.charName}:** ${activeCharacter.greeting}`);
            
            // एंटर दाबल्यावर इनपुट प्रोसेस करण्याची सोय
            input.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    processInput();
                }
            });
        };
    </script>
</body>
</html>
