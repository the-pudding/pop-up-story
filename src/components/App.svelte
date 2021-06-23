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
  import {select, selectAll, groups, ascending, format} from "d3";
  import "intersection-observer";
  import scrollama from "scrollama";
  import { fly } from "svelte/transition";
  import _ from "lodash";
  import { songPlayingSTORE } from "../stores/global.js";

  // AUDIO
  let audioEl;
  let songSpans;
  let songPlaying = false;
  let songData;
  let songUrl;
  let songTitle;
  let songArtist;
  
  // SCROLL
  let scrollContainer;
  let scrollGraphic;
  let scrollText;
  let songHighlights;

  // DATA TO PASS TO COMPONENTS
  let duplicateID = data.map(d => ({
    ...d,
    songArtistCombo: stripSpecialChar(d.song + d.artist)
  }))
  let noDuplicates = _.uniqBy(duplicateID, "songArtistCombo")
  let dataFiltered = noDuplicates.filter(d => d.queerFlag == 1);
  dataFiltered = dataFiltered.sort((a,b) => ascending(a.relationshipType, b.relationshipType));
  let dataQueerSpotlight = groups(dataFiltered, d => d.artistID);
  dataQueerSpotlight = dataQueerSpotlight.sort((a,b) => ascending(a[0][0], b[0][0]))

  // NUMS TO REPLACE SPANS
  let percentSongs = ((noDuplicates.filter(d => d.lyricFlag == 1).length)/(noDuplicates.length)*100).toFixed(1);
  let formatComma = format(",")
  let countSGSongs = noDuplicates.filter(d => d.lyricFlag == 1).length;
  let countSGSongsMinusKaty = countSGSongs - 1;
  let countSongs = noDuplicates.length;
  let countQueerArtistSongs = dataFiltered.length;
  let countSameGenderSongs = dataFiltered.filter(d => d.lyricFlag == 1).length;

  let percentSongsSpan;
  let countSongsSpan;
  let countSGSongsSpan;
  let countSGSongsMinusKatySpan;
  let countQueerArtistSongsSpan;
  let countSameGenderSongsSpan;


  // FUNCTIONS
  function stripSpecialChar(text) { return text.replace(/[^A-Za-z]/g, '') }

  let prevEvent;

  function playSong(e) {
    let currEvent = this.id;

    const songId = stripSpecialChar(this.id);

    songData = previews.filter(d => d.shortname == songId)[0];

    songUrl = songData.preview
    audioEl.src = songUrl;

    songTitle = songData.title;
    songArtist = songData.artist;

    // equal + not playing
    if (prevEvent == currEvent && $songPlayingSTORE) {
      audioEl.pause();
      songSpans.classed("spanplay", false);
      this.classList.remove("spanplay");
      if ($songPlayingSTORE) {
        songPlayingSTORE.set(false)
      }
      else {
        songPlayingSTORE.set(true)
      }
    }
    // not equal + not playing
    else if (prevEvent !== currEvent && $songPlayingSTORE) {
      audioEl.play();
      songSpans.classed("spanplay", false);
      this.classList.add("spanplay");
      if ($songPlayingSTORE) { 
        songPlayingSTORE.set(true)
      }
      else {
        songPlayingSTORE.set(false)
      }
    }
    // equal + playing
    else if (prevEvent == currEvent && !$songPlayingSTORE) {
      audioEl.play();
      songSpans.classed("spanplay", false);
      this.classList.add("spanplay");
      if ($songPlayingSTORE) {
        songPlayingSTORE.set(false)
      }
      else {
        songPlayingSTORE.set(true)
      }
    }
    // not equal + playing
    else {
      audioEl.play();
      songSpans.classed("spanplay", false);
      this.classList.add("spanplay");
      songPlayingSTORE.set(false)
      if ($songPlayingSTORE) {
        songPlayingSTORE.set(false)
      }
      else {
        songPlayingSTORE.set(true)
      }
    }

    prevEvent = this.id;
  }

  // SCROLL
  const scroller = scrollama();
  let scrollStep;

  function scrollDimensions() {
    const stepHeight = Math.floor(window.innerHeight * 1.25);
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
      offset: 0.75, // set the trigger to be 1/2 way down screen
      debug: false, // display the trigger offset for testing
    })
    .onStepEnter(handleStepEnter)
    .onStepExit(handleStepExit)
  }

  function handleStepExit(response) {
    if (response.index == 0 && response.direction == "up") { 
      songHighlights.classed("is-highlighted", false);
      songHighlights.style("background-image", "none");
    }
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
    // AUDIO
    songSpans = selectAll('.song-snippet');
    songSpans.on("click", playSong);

    // SCROLL
    songHighlights = selectAll(".song-highlight")
    scrollStep = selectAll(".step");
    scrollSetup();
    scrollDimensions();

    // NUM SPANS
    percentSongsSpan = selectAll(".percentSongs");
    percentSongsSpan.text(percentSongs);
    countSGSongsSpan = selectAll(".countSGSongs");
    countSGSongsSpan.text(countSGSongs);
    countSGSongsMinusKatySpan = selectAll(".countSGSongsMinusKaty")
    countSGSongsMinusKatySpan.text(countSGSongsMinusKaty);
    countSongsSpan = selectAll(".countSongs");
    countSongsSpan.text(formatComma(countSongs));
    countQueerArtistSongsSpan = selectAll(".countQueerArtistSongs");
    countQueerArtistSongsSpan.text(countQueerArtistSongs);
    countSameGenderSongsSpan = selectAll(".countSameGenderSongs");
    countSameGenderSongsSpan.text(countSameGenderSongs);
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
<SmallChart data="{dataQueerSpotlight}"/>
<Text copy="{copy.prose6}" section=6/>
<Text copy="{copy.method}" section=7/>
<div class="playlist">
  <iframe title="Queer Lyric References Spotify Playlist" src="https://open.spotify.com/embed/playlist/3g0RrpYM6zZ7d4nfP3XeEh" width="100%" height="380" frameborder="0" allowtransparency="true" allow="encrypted-media"></iframe>
</div>


<div class="playing">
  {#if $songPlayingSTORE}
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
    z-index: 1000;
  }

  .disco-drop {
    display: flex;
    flex-direction: row;
    padding: 0 0.5rem 0.5rem 0.5rem;
    opacity: 1;
    transition: opacity 0.5s linear;
  }

  .display-none {
    opacity: 0;
  }

  .playing img {
    width: 50px;
    height: 58px;
    margin: 0 0.5rem;
  }

  .details {
    margin: 0.5rem 0 0 0;
    max-width: 15rem;
  }

  .details p {
    margin: 0;
    font-size: 0.75rem;
    text-shadow: 0px 0px 2px white, 0px 0px 4px rgba(255, 255, 255, 0.75), 0px 0px 6px rgba(255, 255, 255, 0.5);
  }

  .playing-intro {
    font-weight: 700;
  }

  .playlist {
    margin: 0 auto;
    padding: 0 1rem;
    max-width: 40rem;
    display: flex;
    justify-self: center;
  }

  @media only screen and (max-width: 800px) {
    .playing {
      display: none;
    }
  }
</style>