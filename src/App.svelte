<script>
  import Loader from "./Loader.svelte";
  import WordData from "./WordData.svelte";
  let word = ''
  let loading = false;
  let wordData = null;
  let count = 0;
  function increment() {
    console.log('hi')
    ++count;
  }
  $:console.log('Landed hahaha')
  $:increment()
  $:console.log(count)

  async function searchWord() {
    if(word.length === 0) {
      console.log('Enter a word god damn it!')
      return;
    }
    loading = true;
    wordData =  null;
    try{
      let res = await fetch('https://api.dictionaryapi.dev/api/v2/entries/en/' + word)
      let data = await res.json();
      if(Array.isArray(data)) {
        console.log(data)
        wordData  = data[0];
      }else{
        wordData = 'No Result!';
        loading = false;
      }
    } catch(err) {
      loading = false
      console.error(err);
    }
  }
</script>
<main>
  <div class="header">Dictionary App</div>
  <div class="center">
    <div class="form">
      <div class="form-group">
        <label for="word">Search Word</label>
        <input type="text" name = 'word' placeholder="type the word here" bind:value={word}/>
      </div>
      <div class="form-group">
        <button on:click={searchWord}>Search Word</button>
      </div>
      {#if loading === true || wordData !== null}
      <div class="result">
        {#if wordData !== null && typeof wordData!== 'string'}
          <p>Found Data</p>
          <WordData wordData = {wordData}/>
        {:else if wordData === null && loading === true}
          <Loader/>
        {:else}
          <p class="padding">No result found!</p>
        {/if}
      </div>
      {/if}

    </div>
  </div>
</main>