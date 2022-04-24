<script>
    
let playing = false;
let currentPlayer = 1;
let disableStart = true;
let timerValue = 0;
// Sound effects for project.
let selected;
let toggle = 0;
let timer = [
		{ id: 1, value: 60 },
		{ id: 2, value: 30 },
		{ id: 3, value: 15 },
		{ id: 3, value: 10 },
	];
// Add a leading zero to numbers less than 10.
let enableTimer =false;
const padZero = (number) => {
    if (number < 10) {
        return '0' + number;
    }
    return number;
}

let min1, sec1;
let min2, sec2;
// Create a class for the timer.
class Timer {
    constructor(player, minutes) {
        this.player = player;
        this.minutes = minutes;
    }
    getMinutes(timeId) {
        return timerValue;
    }
}

let p1time = new Timer('min1', timerValue);
let p2time = new Timer('min2', timerValue);


// Swap player's timer after a move (player1 = 1, player2 = 2).
const swapPlayer = () => {
   
        if (!playing) return;
    // Toggle the current player.
    currentPlayer = currentPlayer === 1 ? 2 : 1;
    // Play the click sound.
}


// Warn player if time drops below thirty seconds.
const timeWarning = (player, min, sec) => {
    // Change the numbers to red during the last 30 seconds.
    if (min < 1 && sec <= 30) {
        if (player === 1) {
            document.querySelector('.player-1 .player__digits').style.color = '#CC0000';
        } else {
            document.querySelector('.player-2 .player__digits').style.color = '#CC0000';
        }
    }
}


// Start timer countdown to zero.
const startTimer = () => {
    playing = true;
    let p1sec = 60;
    let p2sec = 60;
    p1time.minutes = timerValue;
    p2time.minutes = timerValue;
    let timerId = setInterval(function() {
        // Player 1.
        if (currentPlayer === 1) {
            if (playing) {
                // Disable start button.
                disableStart =  false;
              
                if (p1sec === 60) {
                    p1time.minutes = p1time.minutes - 1;
                }
                p1sec = p1sec - 1;
                timeWarning(currentPlayer, p1time.minutes, p1sec);
                sec1 = padZero(p1sec);
                min1 = padZero(p1time.minutes);
                if (p1sec === 0) {
                    // If minutes and seconds are zero stop timer with the clearInterval method.
                    if (p1sec === 0 && p1time.minutes === 0) {
                       
                        // Stop timer.
                        clearInterval(timerId);
                        playing = false;
                    }
                    p1sec = 60;
                }
            }
        } else {
        // Player 2.
            if (playing) {
            
                if (p2sec === 60) {
                    p2time.minutes = p2time.minutes - 1;
                }
                p2sec = p2sec - 1;
                timeWarning(currentPlayer, p2time.minutes, p2sec);
                sec2 = padZero(p2sec);
                min2 = padZero(p2time.minutes);
                if (p2sec === 0) {
                    // If minutes and seconds are zero stop timer with the clearInterval method.
                    if (p2sec === 0 && p2time.minutes === 0) {
                        // Play a sound effect.
                    
                        // Stop timer.
                        clearInterval(timerId);
                        playing = false;
                    }
                    p2sec = 60;
                }
            }
        }
    }, 1000);
}
const reload = () =>{
    location.reload();
}
const handleSubmit = ()=> {
    enableTimer = true
    timerValue = selected.value;
}

// Listen for a mouse click or tap on the screen to toggle between timers.


// Loop through the start and reset buttons.
// for (let i = 0; i < buttons.length; i++) {
//     buttons[i].addEventListener('click', () => {
//         if (buttons[i].textContent === 'START') {
//             // Turn the button a gray color to signify a disabled button.
//             buttons[i].style.color = '#EEEEEE';
//             buttons[i].style.backgroundColor = '#606060';
//             startTimer();
//         } else {
//             // Reset everything by reloading the page.
//             location.reload(true);
//         }
//     });
// }

// Listen for the press of the spacebar.
//document.addEventListener('keypress', event => {
    // if (event.keyCode === 32 || event.which === 32) {
    //     swapPlayer();
    // }
//});


</script>
<body oncontextmenu="return false;">

   

    <main on:mousedown={swapPlayer} >
        <form >
            <select class="timerOption" bind:value={selected} on:change="{() => timerValue = selected.value}">
                {#each timer as timer}
                    <option value={timer}>
                        {timer.value}
                    </option>
                {/each}
            </select>
            </form>
    {#if timerValue !== 0}
    <div>
      <div class="player" >
      
        <div class="player__tile player-1">
          <div class="player__digits">
              {#if min1}
            <span id="min1">{min1}</span>:<span id="sec1">{sec1}</span>
            {:else}
            <span id="min1">{timerValue}</span>:<span id="sec1">00</span>
            {/if}
          </div>
        </div>
        
        <div class="player__tile player-2">
          <div class="player__digits">
              {#if min2}
            <span id="min2">{min2}</span>:<span id="sec2">{sec2}</span>
            {:else}
            <span id="min2">{timerValue}</span>:<span id="sec2">00</span>
            {/if}
          </div>
        </div>
        
      </div>
      
      <div class="timer__buttons">
        {#if disableStart}
        <button class="timer__start-bttn bttn" on:click={startTimer}  type="button">START</button>
        {/if}
      
        <button class="timer__reset-bttn bttn" on:click={reload} type="button">RESET</button>
      </div>
    </div>
    {/if}
    </main>

  



  </body>
<style>
    main {
    width: 100%;
    padding: 0 10px;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
}


.timerOption{
    width: 100%;
    color: #020202;
    min-height: 50px;
    max-width: 600px;
    font-size: 4.5rem;
    font-weight: bold;
    border-radius: 8px;
    letter-spacing: 2px;
    margin: 0 auto 5px auto;
    border: 4px solid #000000;
    font-family: 'FiraMono-Regular', monospace;
    padding: 10px;
}
.player {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    margin: 1em 0 5px 0;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
}

.player__tile {
    width: 100%;
    height: 300px;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    margin: 0 auto;
    color: #000000;
    max-width: 400px;
    border-radius: 8px;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
    -webkit-box-pack: center;
        -ms-flex-pack: center;
            justify-content: center;
    background-color: #FFFFFF;
    -webkit-box-shadow: inset 4px 4px 0 #000, 
                inset -4px 4px 0 #000, 
                inset -4px -4px 0 #000, 
                inset 4px -4px 0 #000;
            box-shadow: inset 4px 4px 0 #000, 
                inset -4px 4px 0 #000, 
                inset -4px -4px 0 #000, 
                inset 4px -4px 0 #000;
}

.player-2 {
    color: #FFFFFF;
    margin-top: 5px;
    background-color: #2D2C2C;
}

.player__digits {
    font-size: 5.5rem;
    font-weight: bold;
}

.timer__buttons {
    margin-bottom: 1em;
}

.timer__start-bttn, 
.timer__reset-bttn {
    width: 100%;
    display: block;
    color: #020202;
    min-height: 50px;
    max-width: 400px;
    font-size: 1.5rem;
    font-weight: bold;
    border-radius: 8px;
    letter-spacing: 2px;
    margin: 0 auto 5px auto;
    border: 4px solid #000000;
    font-family: 'FiraMono-Regular', monospace;
}

.timer__start-bttn {
    color: #FFFFFF;
    background-color: #0071D5;
}

.timer__start-bttn:hover {
    color: #000000;
    background-color: #FFFFFF;
}

.timer__reset-bttn:hover {
    color: #FFFFFF;
    background-color: #0071D5;
}

@media only screen and (orientation: landscape) and (max-width: 850px) {
    .player {
        max-width: 610px;
        -webkit-box-orient: horizontal;
        -webkit-box-direction: normal;
            -ms-flex-direction: row;
                flex-direction: row;
        margin: 5px auto 0 auto;
    }
    .player__tile {
        max-width: 300px;
        max-height: 250px;
        margin: 0 3px 5px 3px;
    }
    .player__digits {
        font-size: 5rem;
    }
    .timer__buttons {
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        margin: 0 auto;
        max-width: 610px;
    }
    .timerOption{
        max-width: 300px;
        margin: 0 3px 5px 3px;
    }
     .timer__start-bttn, 
    .timer__reset-bttn {
        display: block;
        max-width: 300px;
        margin: 0 3px 5px 3px;
    }
}

@media only screen and (orientation: portrait) and (min-width: 400px) {
    .player__tile {
        height: 400px;
    }
    .player__digits {
        font-size: 6rem;
    }
}

@media only screen and (min-width: 850px) {
    .player {
        margin: 1em auto 10px auto;
        max-width: 810px;
        -webkit-box-orient: horizontal;
        -webkit-box-direction: normal;
            -ms-flex-direction: row;
                flex-direction: row;
    }
    .player__tile {
        height: 400px;
    }
    .player-2 {
        margin-top: 0;
    }
    .player__digits {
        font-size: 7rem;
    }
    .timer__buttons {
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        margin: 0 auto;
        max-width: 810px;
    }
    .timer__start-bttn, 
    .timer__reset-bttn {
        padding: .7em;
        font-size: 1.8rem;
    }
}

</style>