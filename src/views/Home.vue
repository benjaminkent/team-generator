<template lang="pug">
  .home-container
    h1 Team Name Generator
    form(@submit.prevent="onSubmit" v-if="enterNames")
      .input-group
        label(for="player-one") Player 1
        input(type="text" id="player-one" v-model="players[0]")
      .input-group
        label(for="player-two") Player 2
        input(type="text" id="player-two" v-model="players[1]")
      .input-group
        label(for="player-three") Player 3
        input(type="text" id="player-three" v-model="players[2]")
      .input-group
        label(for="player-four") Player 4
        input(type="text" id="player-four" v-model="players[3]")
      button(type="submit") Generate Team Names
    .team-container(v-else)
      .team(v-if="teamOne.length")
        h2 Team One
        p {{ teamOne[0] }} & {{ teamOne[1] }}
      .team(v-if="teamTwo.length")
        h2 Team Two
        p {{ teamTwo[0] }} & {{ teamTwo[1] }}
      .buttons-container
        button(@click="regenerate") Regenerate Teams
        button.update-players(@click="toggle") Update Players
</template>

<script>
export default {
  name: 'home',
  data () {
    return {
      players: [],
      enterNames: true,
      teamOne: '',
      teamTwo: ''
    }
  },
  methods: {
    shuffle (array) {
      for (let i = array.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array
    },
    onSubmit () {
      this.shuffle(this.players)
      this.teamOne = this.players.slice(0,2)
      this.teamTwo = this.players.slice(2,4)
      this.enterNames = false
      this.players = []
    },
    toggle () {
      this.enterNames === true ? this.enterNames = false : this.enterNames = true
    },
    regenerate () {
      this.players = this.teamOne.concat(this.teamTwo)
      this.onSubmit()
    }
  }
}
</script>

<style lang="scss" scoped>
.home-container {
  display: flex;
  align-items: center;
  flex-direction: column;
}

h1 {
  margin: 10px 0;
}

form {
  min-width: 310px;
  max-width: 375px;
  margin: 10px;
}

.team-container {
  min-width: 310px;
  max-width: 375px;

  .team {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
}

.input-group {
  margin: 10px 0;
  display: flex;
  flex-direction: column;
}

input {
  border: 1px solid #ccc;
  border-radius: 3px;
  font-size: 12px;
  padding: 5px 0;
  text-indent: 5px;
}

button {
  margin-top: 20px;
  width: 100%;
  font-size: 14px;
  padding: 7px 10px;
  border: 1px solid #b200ff;
  background-color: #b200ff;
  color: #fff;
  border-radius: 3px;
  cursor: pointer;
}

button:hover {
  background-color: #490068;
  border: 1px solid #490068;
}

.buttons-container {
  margin-top: 10px;
}
</style>

