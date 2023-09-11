<script>
import { onMount } from 'svelte';
import Modal from './Modal.svelte';
import GroupItem from './GroupItem.svelte';
import Group from './Group.svelte';
import Tiles from './Tiles.svelte';
import Table from './Table.svelte';

let birds = []
let sounds = []
let showModal = true
let volumeAll = 0.1

let orden = []
let familie = []

$: if(sounds.length > 0 ) {
    //console.log("setSettings")
    setSettings(birds, volumeAll)
}

$: setVolumes(orden, familie)

onMount(async () => {
    const res = await fetch('./birds.json');
    birds = await res.json();
    initSettings()
    loadSounds()
    createLists()
});

function setVolumes(orden, familie) {
    for(let i=0; i<birds.length; i++) {
        let on = birds[i].settings.volume
        orden.forEach(ord => {
            if (birds[i].orden === ord.title.toLowerCase()) {
                if (ord.volume === true) {
                    on = 1.0
                } else {
                    on = 0.0
                }
            }
        })
        familie.forEach(fam => {
            if (birds[i].familie === fam.title.toLowerCase()) {
                if (fam.volume === true) on = 1.0
            }
        })
        birds[i].settings.volume = on
    }
    setSettings(birds, volumeAll)
}

function createLists() {
    birds.forEach(bird => {    
        if (!orden.includes(capitalizeFirstLetter(bird.orden))) {
            orden.push(capitalizeFirstLetter(bird.orden))
        }
        if (!familie.includes(capitalizeFirstLetter(bird.familie))) {
            familie.push(capitalizeFirstLetter(bird.familie))
        }
    })

    orden = orden.sort()
    familie = familie.sort()

    for(let o=0; o<orden.length; o++) {
        let ordenCount = 0
        for(let b=0; b<birds.length; b++) {
            if(orden[o] === capitalizeFirstLetter(birds[b].orden)) {
                ordenCount++
            }
        }
        orden[o] = {"title": orden[o], "volume": 0, "count": ordenCount}
    }

    for(let f=0; f<familie.length; f++) {
        let familieCount = 0
        for(let b=0; b<birds.length; b++) {
            if(familie[f] === capitalizeFirstLetter(birds[b].familie)) {
                familieCount++
            }
        }
        familie[f] = {"title": familie[f], "volume": 0, "count": familieCount}
    }
}

function initSettings() {
    birds.forEach(bird => bird.settings = {"volume": 0})
}

function setSettings(birds, volumeAll) {
    sounds.forEach((sound, index) => sound.volume = birds[index].settings.volume * volumeAll) 
}

function start() {
    showModal = false
    play()
}

function play() {
    sounds.forEach(sound => sound.play())
}

function pause() {
    sounds.forEach(sound => sound.pause())
}

function selectAll() {
    birds.forEach(bird => bird.settings.volume = 1)
    birds = birds
}

function selectNone() {
    birds.forEach(bird => bird.settings.volume = 0)
    orden.forEach(ord => ord.volume = 0)
    familie.forEach(fam => fam.volume = 0)
    birds = birds
}

function loadSounds() {
    birds.forEach(bird => {
        var sound = new Audio("wav/" + bird.wav)
        sound.loop = true
        sounds.push(sound)
    })
}

function capitalizeFirstLetter(string) {
  return string.charAt(0).toUpperCase() + string.slice(1);
}

</script>
<main> 
    {#if showModal}
	    <Modal on:close={start} />
    {:else}
        <div class="bar">
            <button on:click={selectAll} >Vælg alle</button>
            <button on:click={selectNone} >Vælg ingen</button>
            <label for="volume" class="title">Volume:</label>
            <input type="range" id="volume" style="vertical-align:middle;" min="0" max="1" step="0.01" bind:value={volumeAll} >
        </div>
        <br><br>
        <div style="margin-left: 4px;">
            <div>
                <!-- 
                <p class="groupheader">Orden</p>
                <Group bind:data={orden} />
                -->
                <p class="groupheader">Familie</p>
                <Group bind:data={familie} />
                <Tiles bind:data={birds} />
            </div>
        </div>
    {/if}
</main>
<style>

.groupheader {
    margin: 0;
    margin-left: 2px;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 12px;
}

.bar {
    width: 100%;
    position: fixed; 
    background-color: #bbbbbb;
    margin: 0px;
    padding: 6px;
}

.title {
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 11px;
    margin: 0px;
}

input[type=range] {
    width: 100px;
}
</style>