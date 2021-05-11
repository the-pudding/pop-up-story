<script>
  import { onMount } from 'svelte';
  import Meta from "./Meta.svelte";
  import Text from "./Text.svelte";
  import Pole from "./Pole.svelte";
  import Chapstick from "./Chapstick.svelte";
  import BestBuy from "./BestBuy.svelte";
  import BlockChart from "./BlockChart.svelte";
  import Header from "./pudding/Header.svelte";
  import Footer from "./pudding/Footer.svelte";
  import copy from "../data/doc.json";
  import data from "../data/sheet.csv";
  import previews from "../data/song-previews.csv";
  import {selection, select, selectAll} from "d3-selection";
  import {filter} from "d3-array";
  import inView from "../actions/inView.js";


  let section;

  // AUDIO
  let audioEl;
  let songSpans;
  let songPlaying = false;

  onMount(() => {
    songSpans = selectAll('.song-snippet');
    songSpans.on("click", playSong);
	});

  function playSong(e) {
    const songId = this.id

    this.style.backgroundImage = `url("assets/images/volume-x.svg")`

    console.log(this);

    const songUrl = previews.filter(d => d.shortname == songId)[0].preview
    audioEl.src = songUrl;

    if (songPlaying) {
      audioEl.pause();
      this.style.backgroundImage = `url("assets/images/volume-2.svg")`
    }
    else {
      audioEl.play();
      this.style.backgroundImage = `url("assets/images/volume-x.svg")`
    }

    songPlaying = !songPlaying;
  }

</script>

<Meta />

<Header />

<Pole />
<Chapstick />
<Text copy="{copy.prose1}" section=1/>
<Text copy="{copy.prose2}" section=2/>
<Text copy="{copy.prose3}" section=3/>
<BestBuy />
<Text copy="{copy.prose4}" section=4/>
<BlockChart data="{data}" />
<Text copy="{copy.prose5}" section=5/>


<audio bind:this="{audioEl}" src=""> </audio>

<Footer />