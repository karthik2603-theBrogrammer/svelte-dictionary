<script>
    export let wordData = null  
    function playAudio(src) {
        let audio = new Audio(src)
        audio.play()
    }  

</script>
<style>

</style>
<div class="word-data">
    {#if wordData?.phonetics}
        <div class="audios">
            {#each wordData?.phonetics as data  }
                {#if data?.audio !== ''}
                <div class="audio">
                    <p>{data?.text}
                        <i class="fa fa-play-circle" on:click={() => playAudio(data?.audio)}>Take audio</i>
                    </p>
                </div>
                {/if}
            {/each}
        </div>
    {/if}
    {#each wordData.meanings  as meaning }
        <div class="meaning">
            <div class="part-of-speech">&bull; {meaning.partOfSpeech}</div>
            <div class="definitions">
                {#each meaning.definitions  as data }
                    <div class="defintion">
                        <p>Definition: <b>{data.definition}</b></p>
                        {#if data.example}
                            <p>Example: <i>{data.example}</i> </p>
                        {/if}
                    </div>
                    
                {/each}
            </div>
        </div>
        
    {/each}
</div>