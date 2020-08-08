<template>
  <div id="app">
    <div class="mx-auto mt-5 " style="width: 600px;">
      <h5 class="display-4">DEVELOPED BY IOWND</h5>
    </div>
    <b-container class="mt-4">
      <div>
        <b-button
          class="mx-1 button"
          v-for="profesion of profesiones"
          @dragstart="dragStart"
          @dragover.stop
          :draggable="true"
          :id="profesion.id"
          :variant="profesion.type"
          :key="profesion.id"
          >{{ profesion.name }}
        </b-button>
      </div>
    </b-container>

    <div class="mx-auto" style="width: 500px;">
      <b-container class="mt-3">
        <b-form inline @submit.prevent="addPlayer">
          <b-input
            id="name"
            autofocus
            class="mb-2 mr-sm-2 mb-sm-0"
            placeholder="Jugador"
            v-model="player"
            @keyup.enter="addPlayer"
          ></b-input>
          <b-button variant="primary" @click="addPlayer">Agregar</b-button>
          <b-button variant="danger" class="ml-1" @click="clean"
            >Limpiar</b-button
          >
        </b-form>
      </b-container>
    </div>

    <b-container fluid class="mt-5">
      <b-row>
        <b-col v-for="player of players" :key="player.id">
          <div>
            <b-card
              :title="player.name"
              tag="article"
              style="max-width: 20rem;"
              class="mb-2 bg-light "
            >
              <hr />
              <div
                class="drop-area"
                @dragover.prevent
                @drop.prevent="drop"
                :id="player.id"
              >
                <b-button
                  class="m-1"
                  v-for="go of player.goTo"
                  :key="go.id"
                  :variant="go.type"
                >
                  {{ go.name }}
                </b-button>
              </div>
            </b-card>
          </div>
        </b-col>
      </b-row>
    </b-container>

    <main class="flexbox" v-if="false">
      <h1>HELLO</h1>
      <Board id="board-1">
        <Card
          :id="profesion.id"
          draggable="true"
          v-for="profesion of profesiones"
          :key="profesion.id"
        >
          <p>{{ profesion.name }}</p>
        </Card>
      </Board>
      <Board id="board-2">
        <!-- <Card id="card-2" draggable="true">
          <p>Card Two</p>
        </Card> -->
      </Board>
    </main>
  </div>
</template>

<script>
import Board from "./components/Board";
import Card from "./components/Card";

const [TANK, BUFFER, DEBUFFER, DD] = ["warning ", "success", "dark", "danger"];

export default {
  name: "App",
  components: {
    Board,
    Card
  },
  mounted() {
    (this.player = null), (this.onDrag = null), (this.players = []);
  },
  data() {
    return {
      player: null,
      onDrag: null,
      profesiones: [
        { id: 1, name: "TITAN", type: DD },
        { id: 2, name: "GK", type: DD },
        { id: 3, name: "CC", type: BUFFER },
        { id: 4, name: "COV", type: BUFFER },
        { id: 5, name: "APPETITE", type: BUFFER },
        { id: 6, name: "GATO", type: BUFFER },
        { id: 7, name: "SWM", type: BUFFER },
        { id: 8, name: "SD", type: BUFFER },
        { id: 9, name: "SEMILLA NEGRA", type: TANK },
        { id: 10, name: "SEMILLA BLANCA", type: TANK },
        { id: 11, name: "STIGMA", type: DEBUFFER },
        { id: 12, name: "NECRO", type: DEBUFFER }
      ],
      players: []
    };
  },
  methods: {
    addPlayer() {
      if (this.player === "") return;
      this.players.push({
        id: Date.now(),
        name: this.player.toUpperCase(),
        goTo: []
      });
      this.player = "";
    },
    clean() {
      this.players = [];
    },
    drop(e) {
      const dropId = e.target.id;
      const profesionId = this.onDrag;
      const profesion = this.profesiones.find(
        element => element.id == profesionId
      );
      this.players.find(element => element.id == dropId).goTo.push(profesion);
    },
    dragStart(e) {
      this.onDrag = e.target.id;
    }
  }
};
</script>

<style>
body {
  background: #2c3e50; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to right,
    #4ca1af,
    #2c3e50
  ); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to right,
    #4ca1af,
    #2c3e50
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

.drop-area {
  /* display: flex;
  flex-direction: column; */
  width: 100%;
  max-width: 900px;
  padding: 15px;
  height: 50vh;
}
/* * {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background-color: #f3f3f3;
}
.flexbox {
  display: flex;
  justify-content: space-between;
  width: 100%;
  max-width: 768px;
  height: 100vh;
  overflow: hidden;
  margin: 0 auto;
  padding: 15px;
}
.flexbox .board {
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: 900px;
  background-color: #313131;
  padding: 15px;
}

.flexbox .board .card {
  padding: 15px 25px;
  background-color: #f3f3f3;
  cursor: pointer;
  margin-bottom: 15px;
} */
</style>
