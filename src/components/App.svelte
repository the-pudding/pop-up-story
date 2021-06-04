<script>
  import { onMount } from 'svelte';
  import Meta from "./Meta.svelte";
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
  import {select, selectAll} from "d3-selection";
  import {filter} from "d3-array";
  import inView from "../actions/inView.js";
  import "intersection-observer";
  import scrollama from "scrollama";

  let section;

  // AUDIO
  let audioEl;
  let songSpans;
  let songPlaying = false;
  let songHighlights;

  function playSong(e) {
    const songId = this.id

    this.style.backgroundImage = `url("assets/images/volume-x.svg")`

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

  // SCROLL
  const scroller = scrollama();
  const scrollContainer = select('#scroll');
  const scrollGraphic = scrollContainer.select('.scroll__graphic');
  const scrollText = scrollContainer.select('.scroll__text');
  let scrollStep;

  function scrollDimensions() {
    const stepHeight = Math.floor(window.innerHeight * 0.75);
    scrollStep.style('height', stepHeight + 'px');

    const bodyWidth = select('body').node().offsetWidth;
    scrollGraphic.style('height', (window.innerHeight) + 'px');
  }

  function scrollSetup() {
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
    scrollDimensions();
    scrollSetup();
	});

</script>

<Meta />

<Header />

<Pole />
<Chapstick />
<Text copy="{copy.prose1}" section=1/>
<BestBuy />
<Text copy="{copy.prose2}" section=2/>
<Text copy="{copy.prose3}" section=3/>
<BlockChart data="{data}" />
<Text copy="{copy.prose5}" section=5/>
<SmallChart data="{data}" />
<Text copy="{copy.prose6}" section=6/>
<Text copy="{copy.method}" section=7/>


<audio bind:this="{audioEl}" src="">
  <track kind="captions">
</audio>

<Footer />