<script>
  import { onMount } from 'svelte';
  import Meta from "./Meta.svelte";
  import Text from "./Text.svelte";
  import Pole from "./Pole.svelte";
  import Header from "./pudding/Header.svelte";
  import Footer from "./pudding/Footer.svelte";
  import copy from "../data/doc.json";
  import previews from "../data/song-previews.csv";
  import {selection, select, selectAll} from "d3-selection";
  import {filter} from "d3-array";

  let audioEl;
  let songSpans;
  let songPlaying = false;


  onMount(() => {
    songSpans = selectAll('.song-snippet')
		console.log(songSpans)

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

<Text copy="{copy}"/>
<Pole />

<audio bind:this="{audioEl}" src=""> </audio>

<Footer />