<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8" />
    <title>Tirage au sort - Weekend Copain</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 2rem;
            background-color: #f5f5f5;
        }
        h1 {
            text-align: center;
        }
        #formContainer {
            margin-bottom: 2rem;
            text-align: center;
        }
        input {
            padding: 0.5rem;
            font-size: 1rem;
            width: 200px;
            margin-right: 1rem;
        }
        button {
            padding: 0.5rem 1rem;
            font-size: 1rem;
            cursor: pointer;
        }
        .commission {
            margin: 1rem 0;
            padding: 1rem;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        ul {
            padding-left: 1.5rem;
        }
    </style>
</head>
<body>
    <h1>Tirage au sort des commissions</h1>
    <div id="formContainer">
        <input type="text" id="nameInput" placeholder="Ton prénom" />
        <button onclick="assignCommission()">Tirer au sort</button>
        <p id="message" style="margin-top: 1rem; font-weight: bold;"></p>
    </div>

    <div id="commissionsContainer"></div>

    <script>
        const commissions = {
            "Nourriture": { max: 4, members: [] },
            "Boissons": { max: 4, members: [] },
            "Animations": { max: 3, members: [] },
            "Covoiturage / Matériel": { max: 3, members: [] },
            "Rien": { max: 6, members: [] },
        };

        function shuffle(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        function updateDisplay() {
            const container = document.getElementById("commissionsContainer");
            container.innerHTML = "";
            for (const [name, data] of Object.entries(commissions)) {
                const div = document.createElement("div");
                div.className = "commission";
                const title = document.createElement("h3");
                title.textContent = name + ` (${data.members.length}/${data.max})`;
                div.appendChild(title);
                const list = document.createElement("ul");
                data.members.forEach(person => {
                    const item = document.createElement("li");
                    item.textContent = person;
                    list.appendChild(item);
                });
                div.appendChild(list);
                container.appendChild(div);
            }
        }

        function assignCommission() {
            const name = document.getElementById("nameInput").value.trim();
            const msg = document.getElementById("message");

            if (!name) {
                msg.textContent = "Merci d’entrer ton prénom.";
                return;
            }

            for (const com of Object.values(commissions)) {
                if (com.members.includes(name)) {
                    msg.textContent = "Tu es déjà inscrit·e dans une commission.";
                    return;
                }
            }

            const available = shuffle(Object.entries(commissions).filter(([_, data]) => data.members.length < data.max));
            if (available.length === 0) {
                msg.textContent = "Toutes les commissions sont complètes.";
                return;
            }

            const [selectedName, selectedData] = available[0];
            selectedData.members.push(name);
            msg.textContent = `Tu as été tiré·e au sort dans la commission : ${selectedName}`;
            updateDisplay();
        }

        updateDisplay();
    </script>
</body>
</html>
