

<script>
  import languages from './languages.js'; 
  import { Button } from 'flowbite-svelte';
 
  let userInput = "";
  let translatedText = "";
  let isLoading = false;
  let sourceLanguage = "en-GB"; 
  let targetLanguage = ""; 
  const fetchTranslation = async () => {
    isLoading = true;

   
    const textToTranslate = encodeURIComponent(userInput);
    const url = `https://translated-mymemory---translation-memory.p.rapidapi.com/get?langpair=${sourceLanguage}%7C${targetLanguage}&q=${textToTranslate}&mt=1&onlyprivate=0&de=a%40b.c`;
    const options = {
      method: 'GET',
      headers: {
        'X-RapidAPI-Key': 'c9c63dee9dmsh9db810fe16f5b89p139ca2jsn4b2f1d006850',
        'X-RapidAPI-Host': 'translated-mymemory---translation-memory.p.rapidapi.com'
      }
    };

    try {
      const response = await fetch(url, options);
      const result = await response.json();
      
      // Extract the translated text
      if (result && result.responseData && result.responseData.translatedText) {
        translatedText = result.responseData.translatedText;
      } else {
        translatedText = "Translation not available";
      }
    } catch (error) {
      console.error(error);
      translatedText = "Error occurred during translation";
    } finally {
      isLoading = false;
    }
  };

  const handleInputChange = (event) => {
    userInput = event.target.value;
  };

  const handleSourceLanguageChange = (event) => {
    sourceLanguage = event.target.value;
  };

  const handleTargetLanguageChange = (event) => {
    targetLanguage = event.target.value;
  };

  const handleTranslateClick = () => {
    fetchTranslation();
  };
</script>
<main>
  <h1 style="color:green; margin-left:500px ; font-size:40px; font-weight:bold"> Language Translation </h1>
  <div class="main-container">
  <div class="container1">
    <label for="textInput" style="font-size: 20;font-weight:bold; color:green">Enter Text to Translate:</label>
    <input 
      type="text"
      id="textInput"
      bind:value={userInput}
      on:input={handleInputChange} class="text" 
    />
  </div>

  <div class= "container5">
    <label for="sourceLanguage" style="font-size:20px; font-weight:bold;color:blue" >Select text Language:</label>
   <select id="sourceLanguage" bind:value={sourceLanguage} on:change={handleSourceLanguageChange}  >
      {#each Object.keys(languages) as languageCode}
        <option value={languageCode}>{languages[languageCode]}</option>
      {/each}
    </select>
  </div>


    <div class="container2">
      <label for="targetLanguage" style="font-size:20px; font-weight:bold;color:blue;margin-left:-230px" >Select translate Language:</label>
    <select id="targetLanguage" bind:value={targetLanguage} on:change={handleTargetLanguageChange}>
      {#each Object.keys(languages) as languageCode}
        <option value={languageCode}>{languages[languageCode]}</option>
      {/each}
    </select>
    
   
  </div>
  
  <Button style="margin-top:130px; margin-left:50px ; width:800px" on:click={handleTranslateClick}>Translate</Button>
 
  <div class="container3">
    <h2 style="font-size: 20;font-weight:bold; color:green"> Result:</h2>
    {#if isLoading}
      <p>Loading...</p>
    {:else}
      <p>{translatedText}</p>
    {/if}
  </div>


</main>


<style>


.text {
  width: 400px;
  height: 220px;
  margin-top: 5px;
  margin-left: 10px;
  vertical-align: top; /* Align text to the top */
  
}

 .main-container{
  height:550px;
  width: 950px;
  background-color: rgb(229, 224, 224);
  margin-left: 200px;
  border: 1px solid  rgb(87, 245, 98); /* Add a border for styling */
  margin-top: 30px;
 }
 .container1{

  height: 270px;
  width:420px;
  background-color:white;
  margin-top: 20px;
  margin-left: 10px;
  border: 1px solid  rgb(113, 21, 233); /* Add a border for styling */
  
 }
 .container3{

height: 270px;
width:450px;
background-color: white;
margin-top: -500px;
margin-left:450px;
border: 1px solid  rgb(113, 21, 233); /* Add a border for styling */
  
}
.container5{

margin-top:10px;
margin-left: 10px;
}
.container2{

margin-top:-32px;
margin-left: 700px;
}


.button-container{
  height:50px;
  width:600px;
  background-color: red;
  margin-top: 400px;
  margin-left:-300px;
}

</style>
