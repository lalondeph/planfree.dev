<template>
  <div>
    <h1 class="screen-reader-only">Planfree.dev game lobby</h1>

    <Modal
      v-if="modal"
      title="Choose your display name"
      @completed="enteredName"
    ></Modal>

    <Settings
      v-if="settings"
      title="Settings"
      @saveSettings="saveSettings"
    ></Settings>

    <div v-if="!modal && !settings" class="home">
      <div class="top-buttons">
      

        <button class="edit-name-button" @click="modal = true">
          <div>{{ name }}</div>
          <div>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              height="24px"
              viewBox="0 0 24 24"
              width="24px"
              fill="#000000"
            >
              <path d="M0 0h24v24H0V0z" fill="none" />
              <path
                d="M14.06 9.02l.92.92L5.92 19H5v-.92l9.06-9.06M17.66 3c-.25 0-.51.1-.7.29l-1.83 1.83 3.75 3.75 1.83-1.83c.39-.39.39-1.02 0-1.41l-2.34-2.34c-.2-.2-.45-.29-.71-.29zm-3.6 3.19L3 17.25V21h3.75L17.81 9.94l-3.75-3.75z"
              />
            </svg>
          </div>
        </button>
        <button
          v-if="!showCopiedToClipboard"
          class="button invite"
          @click="copyToClipboard()"
        >
          <div>Invite players</div>
          <div>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              height="24"
              viewBox="0 0 24 24"
              width="24"
            >
              <path d="M0 0h24v24H0V0z" fill="none" />
              <path
                d="M16 1H4c-1.1 0-2 .9-2 2v14h2V3h12V1zm3 4H8c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h11c1.1 0 2-.9 2-2V7c0-1.1-.9-2-2-2zm0 16H8V7h11v14z"
              />
            </svg>
          </div>
        </button>
        <button
          v-if="!modal && showCopiedToClipboard"
          class="button invite copied no-hover"
        >
          <div>Copied to clipboard</div>
          <div></div>
        </button>
      </div>

      <div class="top-left-buttons">
        <button class="round-button" @click="goToGithub()" aria-label="Go to the GitHub repository">
          <svg xmlns="http://www.w3.org/2000/svg" height="20" width="20"><path d="M7.333 14.896 10 13.312l2.688 1.584-.709-3 2.313-1.979-3.063-.271L10 6.792 8.771 9.646l-3.063.271 2.334 1.979ZM5.062 18l1.313-5.542L2 8.729l5.75-.5L10 3l2.25 5.25 5.75.479-4.375 3.729L14.938 18 10 15.062ZM10 11.062Z"/></svg>
        </button>

        <button class="round-button" @click="settings = true" aria-label="Settings">
          <svg xmlns="http://www.w3.org/2000/svg" height="20" width="20"><path d="m8.104 17.75-.292-2.458q-.083-.042-.468-.25-.386-.209-.844-.48l-2.25.938-1.896-3.292 1.938-1.479q-.021-.146-.032-.333-.01-.188-.01-.396 0-.125.021-.333.021-.209.021-.438L2.354 7.771 4.25 4.5l2.271.958q.271-.187.614-.395.344-.209.677-.355l.292-2.479h3.792l.292 2.459q.333.166.645.354.313.187.605.416L15.75 4.5l1.896 3.271-2 1.521q.021.187.031.354.011.166.011.354 0 .167-.011.354-.01.188-.01.375l1.979 1.479-1.917 3.292-2.291-.958q-.25.187-.563.375-.313.187-.687.375l-.292 2.458Zm1.834-5.271q1.02 0 1.739-.729.719-.729.719-1.75t-.719-1.75q-.719-.729-1.739-.729-1.021 0-1.75.729-.73.729-.73 1.75t.73 1.75q.729.729 1.75.729Zm0-1.25q-.5 0-.865-.364-.365-.365-.365-.865t.365-.865q.365-.364.865-.364.479 0 .854.364.375.365.375.865t-.375.865q-.375.364-.854.364Zm.083-1.25ZM9.167 16.5h1.645l.271-2.208q.584-.167 1.094-.469.511-.302.99-.781l2.041.896.834-1.376-1.813-1.354q.104-.333.136-.614.031-.282.031-.594 0-.271-.031-.531-.032-.261-.115-.636l1.812-1.416-.812-1.355-2.083.896q-.396-.416-.979-.76-.584-.344-1.105-.49l-.25-2.229H9.167l-.25 2.229q-.625.167-1.125.438t-1 .792l-2.021-.876-.833 1.355 1.75 1.354q-.084.333-.126.646-.041.312-.041.583 0 .271.031.562.031.292.115.646l-1.729 1.354.833 1.376 2-.855q.5.459 1.021.761.52.302 1.104.448Z"/></svg>
        </button>

        <button v-if="showInstallPwa" class="round-button" @click="installPWA()" aria-label="Add to homescreen">
          <svg xmlns="http://www.w3.org/2000/svg" height="20" width="20"><path d="M7 17v-2H3.5q-.625 0-1.062-.438Q2 14.125 2 13.5v-9q0-.625.438-1.062Q2.875 3 3.5 3H11v1.5H3.5v9h13V11H18v2.5q0 .625-.438 1.062Q17.125 15 16.5 15H13v2Zm7.479-6L11 7.521l1.062-1.063 1.688 1.688V3h1.5v5.104l1.688-1.687L18 7.479Z"/></svg>
        </button>
      </div>

      <button v-if="!playerHasVoted() && !showVotes" class="button no-hover">
        <span>Cast your votes</span>
      </button>
      <button
        v-if="playerHasVoted() && !showVotes"
        class="button"
        @click="showVotesClicked()"
      >
        <span>Show votes!</span>
      </button>
      <button
        v-if="showVotes && countdown === 0"
        class="button start"
        @click="startNewGame()"
      >
        <span>Start new game!</span>
      </button>
      <button v-if="showVotes && countdown > 0" class="button no-hover">
        <span>{{ countdown }}</span>
      </button>

      <div class="players" v-for="player in getPlayers()" :key="player.id">
        <div class="player" :class="{ voted: player.vote }">
          <span v-if="showVotes && countdown === 0">{{ player.vote }}</span>
        </div>
        <div class="name">
          <span>{{ player.name }}</span>
        </div>
      </div>

      <!-- TODO make this an array that is set by a setting -->
      <!-- TODO add a 'unit of work' description box -->
      <div class="options" v-if="!showVotes || (showVotes && countdown !== 0)">
        <button
          v-for="vote in tshirt"
          :key="`vote-${vote}`"
          class="fib-button"
          :class="{ current: currentVote === vote }"
          @click="performVote(vote)"
          :disabled="currentVote === vote"
        >
          <span>{{ vote }}</span>
        </button>
      </div>

      <div class="results-container" v-if="showVotes && countdown === 0">
        <div class="results">
          <div class="average">Average: {{ getAverage() }}</div>
          <div class="popular">Popular: {{ getMode() }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import store from "../store";
import Modal from "../components/Modal.vue";
import Settings from "../components/SettingsModal.vue";
import Player from "../view-models/player";
import { io } from "socket.io-client";
import { onMounted, ref } from "vue";
import { useRoute } from "vue-router";

let showInstallPwa = ref(false);
const modal = ref(true);
const settings = ref(false)
const showVotes = ref(false);
const showCopiedToClipboard = ref(false);
const countdown = ref(0);
const interval: any = ref(null);
const name = ref("");
const currentVote: any = ref(null);

const fibonacci = ['0','1','2','3','5','8','13','21','34','55','89','?']
const modFibonacci = ['0','1/2','1','2','3','5','8','13','20','40','100','?']
const tshirt = ['XXS','XS','S','M','L','XL','XL','?']
const powers2 = ['0','1','2','4','8','16','32','64','?']
const five = ['1','2','4','5','?']


let deferredPrompt: any;
window.addEventListener('beforeinstallprompt', (e) => {
  e.preventDefault();
  deferredPrompt = e;
  showInstallPwa.value = true;
});

function installPWA() {
  deferredPrompt.prompt();
}


onMounted(() => {
  if (joiningAGame()) {
    const route = useRoute();
    const socket = io(process.env.VUE_APP_SERVER, {
      query: {
        roomId: route.params.id,
      },
    });
    store.commit("setSocket", socket);
  }

  const storedName = localStorage.getItem("name");
  if (storedName) {
    enteredName(storedName);
  }

  setupSocketHandlers();
});

function showVotesClicked() {
  store.state.socket.emit("show");
}

function getPlayers() {
  return store.state.players;
}

function performVote(vote: string) {
  store.state.socket.emit("vote", vote);
  currentVote.value = vote;
}

function startNewGame() {
  store.state.socket.emit("restart");
}

function emitName(name: string) {
  store.state.socket.emit("name", name);
}

function enteredName(updatedName: string) {
  name.value = updatedName;
  emitName(updatedName);
  localStorage.setItem("name", updatedName);
  modal.value = false;
}

function saveSettings() {
  settings.value = false;
}

function playerHasVoted() {
  const players = store.state.players;
  return (
    players.filter((p) => p.vote !== null && p.vote !== undefined).length > 0
  );
}

function copyToClipboard() {
  const tempInput = document.createElement("input");
  tempInput.value = window.location.href;
  document.body.appendChild(tempInput);
  tempInput.select();
  document.execCommand("copy");
  document.body.removeChild(tempInput);
  showCopiedToClipboard.value = true;
  setTimeout(() => (showCopiedToClipboard.value = false), 3000);
}

function getAverage() {
  const players = store.state.players;
  let count = 0;
  let total = 0;
  for (const player of players) {
    if (player.vote && player.vote !== "?") {
      total += parseInt(player.vote);
      count++;
    }
  }
  return (total / count).toFixed(1).replace(/\.0+$/, "");
}

function getMode(): string {
  const players = store.state.players;
  const scores: Record<string, number> = {};
  for (const player of players) {
    if (player.vote) {
      if (scores[player.vote]) {
        scores[player.vote] = scores[player.vote] + 1;
      } else {
        scores[player.vote] = 1;
      }
    }
  }

  let mostSeen = 1;
  let mostSeenCard = "";
  for (const key in scores) {
    const value = scores[key];
    if (value > mostSeen) {
      mostSeen = value;
      mostSeenCard = key;
    }
  }

  if (mostSeen == 1) {
    return "NA";
  } else {
    return mostSeenCard;
  }
}

function goToGithub() {
  open("https://github.com/LukeGarrigan/planfree.dev");
}

function joiningAGame() {
  const currentState = store.state.socket;
  return (
    currentState &&
    Object.keys(currentState).length === 0 &&
    currentState.constructor === Object
  );
}

function setupSocketHandlers() {
  store.state.socket.on("update", (players: Player[]) => {
    store.state.players = players;
  });

  store.state.socket.on("show", () => {
    showVotes.value = true;
    clearInterval(interval.value);
    countdown.value = 3;
    interval.value = setInterval(() => {
      countdown.value -= 1;
      if (countdown.value == 0) {
        clearInterval(interval.value);
      }
    }, 1000);
  });

  store.state.socket.on("restart", () => {
    showVotes.value = false;
    currentVote.value = null;
  });

  store.state.socket.on("ping", () => {
    store.state.socket.emit("pong");
  });
}
</script>

<style scoped lang="scss">
.players {
  user-select: none;
  position: relative;
  top: 5em;
  width: 320px;
  height: 320px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  .player {
    border-radius: 26px;
    border: none;
    cursor: default;
    width: 64px;
    height: 80px;
    background: #f3f0f1;
    box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
      6px 6px 10px rgba(0, 0, 0, 0.2);
    color: #161b1f;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .name {
    margin-top: 1em;
    text-align: center;
    font-size: 26px;
  }

  .voted {
    background: #54e8dd;
  }
}

.home {
  display: flex;
  justify-content: center;
  height: 100%;
  width: 100%;
  box-sizing: border-box;
}
.button {
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  z-index: 9999;
  top: 45%;
  width: 320px;
  height: 80px;
  background: #f3f0f1;
  border-radius: 32px;
  text-align: center;
  border: none;
  cursor: pointer;
  transition: all 0.1s ease-in-out;
  box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
    6px 6px 10px rgba(0, 0, 0, 0.2);
  color: #161b1f;
  &:hover {
    opacity: 0.3;
    box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
      6px 6px 10px rgba(0, 0, 0, 0.2);
  }
  &:active {
    opacity: 1;
    box-shadow: inset -4px -4px 8px rgba(255, 255, 255, 0.5),
      inset 8px 8px 16px rgba(0, 0, 0, 0.1);
  }

}

.top-left-buttons {
  display: flex;
  flex-direction: row;
  left: 20px;
  top: 20px;
  position: absolute;
  gap: 5px;

  .round-button {
    background: #f3f0f1;
    border-radius: 32px;
    text-align: center;
    border: none;
    cursor: pointer;
    width: 30px;
    height: 30px;
    box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
      6px 6px 10px rgba(0, 0, 0, 0.2);
    color: #161b1f;

    &:hover {
      opacity: 0.3;
      box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
        6px 6px 10px rgba(0, 0, 0, 0.2);
    }
  }
}

@media only screen and (max-width: 700px) {
  .top-left-buttons {
    visibility: hidden;
  }
}

.top-buttons {
  display: flex;
  flex-direction: row;
  width: 100%;
  position: absolute;
  justify-content: flex-end;
  top: 2%;
  right: 2%;
  gap: 20px;
  .invite {
    position: relative;
    user-select: none;
    gap: 10px;
    width: 300px;
    height: 70px;
    font-size: 26px;
    svg {
      position: relative;
      left: 2px;
      top: 3px;
      fill: #161b1f;
    }
  }

  .edit-name-button {
    position: relative;
    user-select: none;
    height: 70px;
    font-size: 26px;
    background: #f3f0f1;
    border-radius: 32px;
    border: none;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    opacity: 0.5;
    cursor: pointer;
    &:hover {
      opacity: 1;
    }
    &:hover::before {
      transform: scaleX(1);
    }

    &:before {
      content: "";
      position: absolute;
      display: block;
      width: 100%;
      height: 2px;
      bottom: 10px;
      right: 10px;
      background-color: #000;
      transform: scaleX(0);
      transition: transform 0.3s ease;
    }
    svg {
      position: relative;
      left: 2px;
      top: 3px;
    }
  }
}

.screen-reader-only {
  position: absolute;
  width: 0px;
  overflow:hidden;
}

.copied {
  background: #54e8dd;
}

.no-hover {
  pointer-events: none;
}

span {
  font-family: "Montserrat", sans-serif;
  font-size: 26px;
  font-weight: semibold;
  color: #161b1f;
  user-select: none;
}

.results-container {
  display: flex;
  justify-content: center;
  position: absolute;
  flex-wrap: wrap;
  height: 200px;
  gap: 30px;
  width: 90%;
  bottom: 5%;
  font-size: 20px;
  color: #54e8dd;

  .results {
    display: flex;
    flex-direction: column;
    justify-content: center;
    background: #161b1f;
    border-radius: 26px;
    border: none;
    width: 250px;
    height: 100px;
    transition: all 0.1s ease-in-out;
    box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
      6px 6px 10px rgba(0, 0, 0, 0.2);

    user-select: none;
    font-family: "Montserrat", sans-serif;
    font-weight: semibold;
    &:focus {
      outline: none;
    }

    .average {
      padding: 4px;
    }
  }
}

.options {
  display: flex;
  justify-content: center;
  position: absolute;
  flex-wrap: wrap;
  height: 200px;
  gap: 30px;
  width: 90%;
  bottom: 5%;
  user-select: none;

  .fib-button {
    display: flex;
    align-items: center;
    justify-content: center;
    background: #f3f0f1;
    border-radius: 26px;
    text-align: center;
    border: none;
    cursor: pointer;
    width: 64px;
    height: 80px;
    transition: all 0.1s ease-in-out;
    box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
      6px 6px 10px rgba(0, 0, 0, 0.2);
    color: #161b1f;
    &:not(.current) {
      &:hover {
        opacity: 0.3;
        box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
          6px 6px 10px rgba(0, 0, 0, 0.2);
      }
      &:active {
        opacity: 1;
        box-shadow: inset -4px -4px 8px rgba(255, 255, 255, 0.5),
          inset 8px 8px 16px rgba(0, 0, 0, 0.1);
      }
    }
    &.current {
      background: #54e8dd;
    }
  }
}
</style>
