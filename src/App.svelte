<script>
  let time = 0;
  let restTime = 0;
  let timeIsGoing = false;
  let restTimeIsGoing = false;
  let timeWord = "Start";
  let setTime = 0;
  let setNumber = 0;
  let setRest = 0;
  let output = "";
  let set = 1;
  let interval;
  let isWorking = "";
  let workAudio;
  let restAudio;
let paused = 1
  function startInterval() {
    clearInterval(interval);
    interval = setInterval(() => {
      if (timeIsGoing) {
        isWorking = "Working";
        if(paused != -1)         time += 0.01;

        if (time >= setTime) {
          time = 0;
          restTimeIsGoing = true;
          timeIsGoing = false;
          restAudio.play();
        }
      }
      if (restTimeIsGoing) {
        isWorking = "Rest";
        if(paused != -1)restTime += 0.01;
          
        if (restTime >= setRest) {
          restTime = 0;
          restTimeIsGoing = false;
          if (set < setNumber) {
            timeIsGoing = true;
            set += 1;
            workAudio.play();
          } else {
            clearInterval(interval);
          }
        }
      }
      output = timeIsGoing ? time.toFixed(2) : restTime.toFixed(2);
    }, 10);
  }

  function onStart(ev) {
    ev.preventDefault();
    const data = new FormData(ev.target);
    setNumber = Number(data.get("setNumber"));
    setTime = Number(data.get("setTime"));
    setRest = Number(data.get("setRest"));
    time = 0;
    restTime = 0;
    set = 1;
    timeIsGoing = true;
    restTimeIsGoing = false;
    startInterval();
    document.getElementById("pause_btn").classList.remove('hidden')

  }
</script>

<div class="center">
  <p class="text">Set number: {set}</p>
  <p class="text">PTimer</p>
</div>
<div class="center">
  <form on:submit|preventDefault={onStart}>
    <input class="input" name="setNumber" placeholder="Number of sets" type="number">
    <input class="input"  name="setTime" placeholder="Time of each set" type="number" step="0.01">
    <input class="input"  name="setRest" placeholder="Rest between sets" type="number" step="0.01">
    <button class="button">Submit</button>
  </form>
</div>
<p class="text" style="font-size: 30px;">{isWorking} {output}</p> 
<button id="pause_btn" class="button hidden" on:click={function(){paused *= -1}}>Pause</button>

<audio bind:this={workAudio} src="alert2.wav"></audio>
<audio bind:this={restAudio} src="alert.wav"></audio>
<style>
  .hidden{
    display: none;
  }
</style>
