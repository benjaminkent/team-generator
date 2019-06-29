<template lang="pug">
  .home-container
    header
      h1 Foosball Team Generator
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
      .team(v-if="teamOne.players.length")
        h2 team {{ normalizeTeamOneName }}
        p.players {{ teamOne.players[0] }} & {{ teamOne.players[1] }}
        .team-setup
          p.serve(v-if="servesFirst === 0") {{ normalizeTeamOneName }} gets first serve
          p.serve(v-else) Get ready to defend!
          p.foosmen Playing the {{ teamOne.foosmen }} foosmen
      .team(v-if="teamTwo.players.length")
        h2 team {{ teamTwo.adjective }} {{ teamTwo.noun }}
        p.players {{ teamTwo.players[0] }} & {{ teamTwo.players[1] }}
        .team-setup
          p.serve(v-if="servesFirst === 1") {{ normalizeTeamTwoName }} gets first serve
          p.serve(v-else) Get ready to defend!
          p.foosmen Playing the {{ teamTwo.foosmen }} foosmen
      .buttons-container
        button(@click="regenerate") Regenerate Teams
        button.update-players(@click="updatePlayers") Update Players
</template>

<script>
import { adjectives } from '../data/adjectives.js'
import { nouns } from '../data/nouns.js'

export default {
  name: 'home',
  data () {
    return {
      players: [],
      enterNames: true,
      teamOne: {
        players: [],
        adjective: '',
        noun: '',
        foosmen: ''
      },
      teamTwo: {
        players: [],
        adjective: '',
        noun: '',
        foosmen: ''
      },
      foosmen: [
        'black',
        'yellow'
      ],
      servesFirst: null
    }
  },
  mounted () {
    this.adjectives = adjectives
    this.nouns = nouns
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
      this.shuffle(this.foosmen)
      this.setPlayers()
      this.setAdjectives()
      this.setNouns()
      this.setFoosmen()
      this.setServesFirst()
      this.enterNames = false
      this.players = []
    },
    setPlayers () {
      this.teamOne.players = this.players.slice(0,2)
      this.teamTwo.players = this.players.slice(2,4)
    },
    setAdjectives () {
      this.teamOne.adjective = this.adjectives[Math.floor(Math.random() * this.adjectives.length)]
      this.teamTwo.adjective = this.adjectives[Math.floor(Math.random() * this.adjectives.length)]
    },
    setNouns () {
      this.teamOne.noun = this.nouns[Math.floor(Math.random() * this.nouns.length)]
      this.teamTwo.noun = this.nouns[Math.floor(Math.random() * this.nouns.length)]
    },
    setFoosmen () {
      this.teamOne.foosmen= this.foosmen[0]
      this.teamTwo.foosmen= this.foosmen[1]
    },
    setServesFirst () {
      this.servesFirst = Math.floor(Math.random() * 2)
    },
    updatePlayers () {
      this.enterNames === true ? this.enterNames = false : this.enterNames = true
    },
    regenerate () {
      this.players = this.teamOne.players.concat(this.teamTwo.players)
      this.onSubmit()
    }
  },
  computed: {
    normalizeTeamOneName: function () {
      let noun = this.teamOne.noun
      let adj = this.teamOne.adjective
      let normalNoun = noun.charAt(0).toUpperCase() + noun.slice(1)
      let normalAdj = adj.charAt(0).toUpperCase() + adj.slice(1)
      return `${normalNoun} ${normalAdj}`
    },
    normalizeTeamTwoName: function () {
      let noun = this.teamTwo.noun
      let adj = this.teamTwo.adjective
      let normalNoun = noun.charAt(0).toUpperCase() + noun.slice(1)
      let normalAdj = adj.charAt(0).toUpperCase() + adj.slice(1)
      return `${normalNoun} ${normalAdj}`
    }
  }
}
</script>

<style lang="scss" scoped>
.home-container {
  display: flex;
  align-items: center;
  flex-direction: column;
  min-width: 100vw;
  color: #222;
}

header {
  margin: 0;
  padding: 20px 0;
  width: 100%;
  text-align: center;
  border-bottom: 1px solid #b200ff20;
  background: repeating-linear-gradient(
    45deg,
    #333,
    #333 5px,
    #222 5px,
    #222 15px
  );

  h1 {
    margin: 0;
    font-size: 24px;
    color: #f2f2f2;
  }
}

form {
  min-width: 310px;
  max-width: 375px;
  margin: 10px;
}

.team-container {
  min-width: 310px;
  max-width: 375px;
  margin-top: 15px;

  .team {
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 1px solid #b200ff30;
    border-radius: 5px;
    margin: 20px 0;
    padding: 10px;
    box-shadow: 0px 0px 5px 2px #22222270;

    h2 {
      margin: 0;
      text-transform: capitalize;
      font-size: 18px;
    }

    p {
      margin: 0;
    }

    .players {
      margin: 20px 0;
      font-size: 18px;
    }

    .team-setup {
      display: flex;
      flex-direction: column;
      align-items: center;

      .serve {
        font-size: 14px;
        margin-bottom: 5px;
      }

      .foosmen {
        font-size: 14px;
      }
    }
  }

  .team:hover {
    box-shadow: 0px 0px 5px 2px #b200ff;
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

