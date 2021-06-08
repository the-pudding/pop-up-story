<script>
  import { onMount } from 'svelte';
  import Meta from "./Meta.svelte";
  import Title from "./Title.svelte";
  import Text from "./Text.svelte";
  import Pole from "./Pole.svelte";
  import Chapstick from "./Chapstick.svelte";
  import BestBuy from "./BestBuy.svelte";
  import BlockChart from "./BlockChart.svelte";
  import SmallChart from "./SmallChart.svelte";
  import Header from "./pudding/Header.svelte";
  import Footer from "./pudding/Footer.svelte";
  import copy from "../data/doc.json";
  import data from "../data/data.csv";
  import previews from "../data/song-previews.csv";
  import {select, selectAll, groups, ascending} from "d3";
  import inView from "../actions/inView.js";
  import "intersection-observer";
  import scrollama from "scrollama";
  import { fly } from 'svelte/transition';

  let section;

  // AUDIO
  let audioEl;
  let songSpans;
  let songPlaying = false;
  let songHighlights;
  let scrollContainer;
  let scrollGraphic;
  let scrollText;

  let dataFiltered = data.filter(d => d.queerFlag == 1);
  dataFiltered = dataFiltered.sort((a,b) => ascending(a.relationshipType, b.relationshipType));
  let dataQueerArtists = dataFiltered.filter(d => d.gender !== "nb");
  let dataNBArtists = dataFiltered.filter(d => d.gender == "nb");
  let dataQueerSpotlight = groups(dataQueerArtists, d => d.artistID);
  let dataNBSpotlight = groups(dataNBArtists, d => d.artistID);

  let songData;
  let songUrl;
  let songTitle;
  let songArtist;

  function playSong(e) {
    const songId = this.id

    this.style.backgroundImage = `url("assets/images/volume-x.svg")`

    songData = previews.filter(d => d.shortname == songId)[0];

    songUrl = songData.preview
    audioEl.src = songUrl;

    songTitle = songData.title;
    songArtist = songData.artist;

    if (songPlaying) {
      audioEl.pause();
      songSpans.style.backgroundImage = `url("assets/images/volume-x.svg")`;
      this.style.backgroundImage = `url("assets/images/volume-2.svg")`;
    }
    else {
      audioEl.play();
      songSpans.style.backgroundImage = `url("assets/images/volume-2.svg")`;
      this.style.backgroundImage = `url("assets/images/volume-x.svg")`
    }

    songPlaying = !songPlaying;
  }

  // SCROLL
  const scroller = scrollama();
  let scrollStep;

  function scrollDimensions() {
    const stepHeight = Math.floor(window.innerHeight * 0.75);
    scrollStep.style('height', stepHeight + 'px');

    const bodyWidth = select('body').node().offsetWidth;
    scrollGraphic.style('height', (window.innerHeight) + 'px');
  }

  function scrollSetup() {
    scrollContainer = select('#scroll');
    scrollGraphic = scrollContainer.select('.scroll__graphic');
    scrollText = scrollContainer.select('.scroll__text');

    scroller.setup({
      container: '#scroll', // our outermost scrollytelling element
      graphic: '.scroll__graphic', // the graphic
      text: '.scroll__text', // the step container
      step: '.scroll__text .step', // the step elements
      offset: 0.5, // set the trigger to be 1/2 way down screen
      debug: false, // display the trigger offset for testing
    })
    .onStepEnter(handleStepEnter)
  }

  function handleStepEnter(response) {
    scrollStep.classed('is-active', (d, i) => i === response.index);

    renderStep(response.index)
  }

  function renderStep(index) {
    songHighlights.classed("is-highlighted", false);
    songHighlights.style("background-image", "none");
    const song = songHighlights.filter((d, i) => i ==index)
    song.classed("is-highlighted", true)

    if (index == 0) { song.style('background-image', `url('assets/images/ikissedagirl.jpg')`) }
    if (index == 1) { song.style('background-image', `url('assets/images/pokerface.jpg')`) }
    if (index == 2) { song.style('background-image', `url('assets/images/ifyouseekamy.jpg')`) }
    if (index == 3) { song.style('background-image', `url('assets/images/samelove.jpg')`) }
    if (index == 4) { song.style('background-image', `url('assets/images/girlcrush.jpg')`) }
    if (index == 5) { song.style('background-image', `url('assets/images/badatlove.jpg')`) }
    if (index == 6) { song.style('background-image', `url('assets/images/bartiercardi.jpg')`) }
    if (index == 7) { song.style('background-image', `url('assets/images/money.jpg')`) }
  }

  // MOUNT
  onMount(() => {
    songSpans = selectAll('.song-snippet');
    songSpans.on("click", playSong);

    songHighlights = selectAll(".song-highlight")
    scrollStep = selectAll(".step");
    scrollSetup();
    scrollDimensions();
	});

</script>

<Meta />

<Header />

<Pole />
<Chapstick />
<Title copy="{copy.meta}" />
<Text copy="{copy.prose1}" section=1/>
<BestBuy />
<Text copy="{copy.prose2}" section=2/>
<Text copy="{copy.prose3}" section=3/>
<BlockChart data="{data}" />
<Text copy="{copy.prose5}" section=5/>
<SmallChart data="{dataQueerSpotlight}" type="cis" />
<Text copy="{copy.prose6}" section=6/>
<SmallChart data="{dataNBSpotlight}" type="nb" />
<Text copy="{copy.prose7}" section=7/>
<Text copy="{copy.method}" section=8/>
<div class="playlist">
  <iframe src="https://open.spotify.com/embed/playlist/3g0RrpYM6zZ7d4nfP3XeEh" width="480" height="380" frameborder="0" allowtransparency="true" allow="encrypted-media"></iframe>

</div>


<div class="playing">
  {#if songPlaying}
  <div transition:fly="{{ y: -200, duration: 1000 }}" class="disco-drop">
    <img src="assets/images/discoball.gif" alt="spinning disco ball">
    <div class="details">
      <p class="playing-intro">Now Playing</p>
      <p id="song-title">"{songTitle}"</p>
      <p>by <span class="song-artist">{songArtist}</span></p>
    </div>
  </div>
  {:else}
  <div class="disco-drop display-none">
    <img src="assets/images/discoball.gif" alt="spinning disco ball">
    <div class="details">
      <p class="playing-intro">Now Playing</p>
      <p id="song-title">"{songTitle}"</p>
      <p>by <span class="song-artist">{songArtist}</span></p>
    </div>
  </div>
  {/if}

  <audio bind:this="{audioEl}" src="">
    <track kind="captions">
  </audio>
</div>

<Footer />

<style>
  .playing {
    position: fixed;
    top: 0;
    left: 0;
    height: 5rem;
  }

  .disco-drop {
    display: flex;
    flex-direction: row;
  }

  .display-none {
    display: none;
  }

  .playing img {
    width: 50px;
    height: 58px;
    margin: 0 0.5rem;
  }

  .details {
    margin: 0.5rem 0 0 0;
  }

  .details p {
    margin: 0;
    font-size: 0.75rem;
  }

  .playing-intro {
    font-weight: 700;
  }

  .playlist {
    margin: 0 auto;
    max-width: 40rem;
    display: flex;
    justify-self: center;
  }
</style>