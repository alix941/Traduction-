<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Alix Translate</title>

  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: Arial, sans-serif;
    }

    body {
      min-height: 100vh;
      background: linear-gradient(135deg, #667eea, #764ba2);
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
    }

    .container {
      width: 100%;
      max-width: 900px;
      background: white;
      border-radius: 20px;
      padding: 30px;
      box-shadow: 0 15px 40px rgba(0,0,0,0.2);
    }

    h1 {
      text-align: center;
      margin-bottom: 10px;
      color: #333;
    }

    .subtitle {
      text-align: center;
      color: #777;
      margin-bottom: 25px;
    }

    .languages {
      display: flex;
      gap: 15px;
      align-items: center;
      margin-bottom: 20px;
    }

    select {
      flex: 1;
      padding: 12px;
      border: 2px solid #ddd;
      border-radius: 10px;
      font-size: 16px;
    }

    #swap {
      border: none;
      background: #667eea;
      color: white;
      width: 45px;
      height: 45px;
      border-radius: 50%;
      font-size: 20px;
      cursor: pointer;
    }

    .translation-box {
      display: flex;
      gap: 15px;
    }

    .box {
      flex: 1;
    }

    textarea {
      width: 100%;
      height: 200px;
      resize: none;
      padding: 15px;
      border: 2px solid #ddd;
      border-radius: 12px;
      font-size: 17px;
    }

    #result {
      width: 100%;
      height: 200px;
      padding: 15px;
      border: 2px solid #ddd;
      border-radius: 12px;
      background: #f7f7f7;
      font-size: 17px;
      overflow-y: auto;
    }

    .buttons {
      display: flex;
      justify-content: center;
      gap: 15px;
      margin-top: 25px;
    }

    button {
      padding: 12px 25px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      font-size: 16px;
    }

    #translate {
      background: #667eea;
      color: white;
    }

    #copy {
      background: #333;
      color: white;
    }

    @media (max-width: 650px) {
      .translation-box {
        flex-direction: column;
      }

      .languages {
        flex-direction: column;
      }

      select {
        width: 100%;
      }
    }
  </style>
</head>

<body>

  <div class="container">

    <h1>🌍 Alix Translate</h1>
    <p class="subtitle">Ton propre traducteur en ligne</p>

    <div class="languages">
      <select id="sourceLanguage">
        <option value="fr">🇫🇷 Français</option>
        <option value="en">🇬🇧 Anglais</option>
        <option value="es">🇪🇸 Espagnol</option>
        <option value="it">🇮🇹 Italien</option>
        <option value="de">🇩🇪 Allemand</option>
        <option value="pt">🇵🇹 Portugais</option>
      </select>

      <button id="swap">⇄</button>

      <select id="targetLanguage">
        <option value="en">🇬🇧 Anglais</option>
        <option value="fr">🇫🇷 Français</option>
        <option value="es">🇪🇸 Espagnol</option>
        <option value="it">🇮🇹 Italien</option>
        <option value="de">🇩🇪 Allemand</option>
        <option value="pt">🇵🇹 Portugais</option>
      </select>
    </div>

    <div class="translation-box">

      <div class="box">
        <textarea
          id="text"
          placeholder="Écris ton texte ici..."
        ></textarea>
      </div>

      <div class="box">
        <div id="result">
          La traduction apparaîtra ici...
        </div>
      </div>

    </div>

    <div class="buttons">
      <button id="translate">🌍 Traduire</button>
      <button id="copy">📋 Copier</button>
    </div>

  </div>

  <script>

    const text = document.getElementById("text");
    const result = document.getElementById("result");
    const sourceLanguage = document.getElementById("sourceLanguage");
    const targetLanguage = document.getElementById("targetLanguage");

    document.getElementById("translate").addEventListener("click", async () => {

      const textToTranslate = text.value.trim();

      if (!textToTranslate) {
        result.innerText = "⚠️ Écris un texte à traduire.";
        return;
      }

      result.innerText = "⏳ Traduction en cours...";

      try {

        const response = await fetch(
          "https://api.mymemory.translated.net/get?q=" +
          encodeURIComponent(textToTranslate) +
          "&langpair=" +
          sourceLanguage.value +
          "|" +
          targetLanguage.value
        );

        const data = await response.json();

        result.innerText = data.responseData.translatedText;

      } catch (error) {

        result.innerText = "❌ Une erreur est survenue.";

      }

    });


    document.getElementById("swap").addEventListener("click", () => {

      const oldSource = sourceLanguage.value;

      sourceLanguage.value = targetLanguage.value;
      targetLanguage.value = oldSource;

      const oldText = text.value;
      text.value = result.innerText;
      result.innerText = oldText;

    });


    document.getElementById("copy").addEventListener("click", () => {

      navigator.clipboard.writeText(result.innerText);

      alert("✅ Traduction copiée !");

    });

  </script>

</body>
</HTML 
