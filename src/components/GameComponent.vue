<template>
  <div>
    <section class="row">
      <div class="small-6 columns">
        <h1 class="text-center">Pokemon1</h1>
        <div class="healthbar">
          <div
            class="healthbar text-center"
            style="background-color: green; margin: 0; color: white;"
            :style="{'width':playerHealth+ '%'}"
          >{{playerHealth}}</div>
        </div>
      </div>
      <div class="small-6 columns">
        <h1 class="text-center">Pokemon2</h1>
        <div class="healthbar">
          <div
            class="healthbar text-center"
            style="background-color: green; margin: 0; color: white;"
            :style="{'width': monsterHealth+'%'}"
          >{{monsterHealth}}</div>
        </div>
      </div>
    </section>
    <section class="row controls" v-if="!gameIsRunning">
      <div class="small-12 columns">
        <button id="start-game" @click="startGame">START NEW GAME</button>
        <div class="small-12 columns">
          <div class="small-12 columns custom-align">
            <div
              class="pokemon"
              v-for="pokemon in pokemons"
              :key="pokemon.id"
              @click="getPokemon(pokemon.id, pokemon.name)"
            >
              <h1>
                <u>{{pokemon.name}}</u>
              </h1>
              <img :src="pokemon.img" alt />
            </div>
          </div>
        </div>
      </div>
    </section>
    <section class="row controls" v-if="gameIsRunning">
      <div class="small-12 columns">
        <button id="attack" @click="attack">ATTACK</button>
        <button id="special-attack" @click="specialAttack">SPECIAL ATTACK({{attackSp}})</button>
        <button id="heal" @click="heal">HEAL({{setHealth}})</button>
        <button id="give-up" @click="giveUp">GIVE UP</button>
      </div>
    </section>
    <section class="row log" v-if="turns.length > 0">
      <div class="small-12 columns">
        <ul>
          <li
            v-for="(turn, index) in turns"
            :key="index"
            :class="{'player-turn': turn.isPlayer, 'monster-turn': !turn.isPlayer}"
          >{{turn.text}}</li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "GameComponent",
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      gameIsRunning: false,
      setHealth: 0,
      attackSp: 0,
      turns: [],
      pokemons: [
        {
          id: 1,
          name: "rattata",
          img:
            "https://www.pngix.com/pngfile/middle/654-6547785_rattata-3d-png-pokemon-transparent-png.png",
          pv: 30,
          attack: 56
        },
        {
          id: 2,
          name: "pikachu",
          img:
            "https://www.pngix.com/pngfile/middle/654-6547511_log-in-register-pokemon-detective-pikachu-png-transparent.png",
          pv: 35,
          attack: 55
        },
        {
          id: 3,
          name: "zapdos",
          img:
            "https://www.clipartmax.com/png/middle/77-773492_zapdos-by-blui129-zapdos-pokemon-3d-png.png",
          pv: 90,
          attack: 90
        }
      ]
    };
  },
  methods: {
    startGame: function() {
      this.gameIsRunning = true;
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.turns = [];
      this.attackSp = 5;
      this.setHealth = 3;
    },
    attack: function() {
      if (this.attackSp === 0) {
        this.attackSp = this.attackSp;
      } else {
        this.attackSp--;
      }
      let damage = this.calculateDamage(3, 10);
      this.turns.unshift({
        isPlayer: true,
        text: "Player hits Monster for " + damage
      });
      this.monsterHealth -= damage;
      this.monsterAttacks();
      if (this.checkWin()) {
        return;
      }
    },
    specialAttack: function() {
      if (this.attackSp === 0) {
        let damageSp = this.calculateDamage(10, 20);
        this.monsterHealth -= damageSp;
        this.turns.unshift({
          isPlayer: true,
          text: "Player hits Monster hard for " + damageSp
        });
        if (this.checkWin()) {
          return;
        }
        this.monsterAttacks();
        this.attackSp = 5;
      } else {
        this.turns.unshift({
          isPlayer: true,
          text: "Cant active now Special attack"
        });
      }
    },
    heal: function() {
      let health = 10;
      this.setHealth -= 1;
      if (this.setHealth >= 0) {
        if (this.attackSp === 0) {
          this.attackSp = this.attackSp;
        } else {
          this.attackSp--;
        }
        this.monsterAttacks();
        if (this.playerHealth <= 90) {
          this.playerHealth += health;
        } else {
          this.playerHealth = 100;
        }
        this.turns.unshift({
          isPlayer: true,
          text: "Player heals for " + health
        });
      } else {
        this.setHealth = 0;
        this.turns.unshift({
          isPlayer: true,
          text: "You cant Health anymore !"
        });
      }
    },
    giveUp() {
      this.gameIsRunning = false;
    },
    monsterAttacks: function() {
      let damageMonster = this.calculateDamage(5, 12);
      this.playerHealth -= damageMonster;
      // this.checkWin();
      this.turns.unshift({
        isPlayer: false,
        text: "Monster hits Player for " + damageMonster
      });
    },
    calculateDamage: function(minDamage, maxDamage) {
      return Math.max(Math.floor(Math.random() * maxDamage) + 1, minDamage);
    },
    checkWin: function() {
      if (this.monsterHealth <= 0) {
        if (confirm("You won! New Game?")) {
          this.startGame();
          this.setHealth = 3;
        } else {
          this.gameIsRunning = false;
          this.monsterHealth = 0;
        }
        return true;
      } else if (this.playerHealth <= 0) {
        if (confirm("You lost! New Game?")) {
          this.startGame();
          this.setHealth = 3;
        } else {
          this.gameIsRunning = false;
          this.playerHealth = 0;
        }
        return true;
      }
      return false;
    },
    getPokemon(id, name) {
      console.log("id: " + id + " name: " + name);
    }
  }
};
</script>

<style scoped>
@import "../assets/foundation.min.css";
.text-center {
  text-align: center;
}

.healthbar {
  width: 80%;
  height: 40px;
  background-color: #eee;
  margin: auto;
  transition: width 500ms;
}

.controls,
.log {
  margin-top: 30px;
  text-align: center;
  padding: 10px;
  border: 1px solid #ccc;
  box-shadow: 0px 3px 6px #ccc;
}

.turn {
  margin-top: 20px;
  margin-bottom: 20px;
  font-weight: bold;
  font-size: 22px;
}

.log ul {
  list-style: none;
  font-weight: bold;
  text-transform: uppercase;
}

.log ul li {
  margin: 5px;
}

.log ul .player-turn {
  color: blue;
  background-color: #e4e8ff;
}

.log ul .monster-turn {
  color: red;
  background-color: #ffc0c1;
}

button {
  font-size: 20px;
  background-color: #eee;
  padding: 12px;
  box-shadow: 0 1px 1px black;
  margin: 10px;
}

#start-game {
  background-color: #aaffb0;
}

#start-game:hover {
  background-color: #76ff7e;
}

#attack {
  background-color: #ff7367;
}

#attack:hover {
  background-color: #ff3f43;
}

#special-attack {
  background-color: #ffaf4f;
}

#special-attack:hover {
  background-color: #ff9a2b;
}

#heal {
  background-color: #aaffb0;
}

#heal:hover {
  background-color: #76ff7e;
}

#give-up {
  background-color: #ffffff;
}

#give-up:hover {
  background-color: #c7c7c7;
}

.custom-align {
  display: flex;
}

.pokemon {
  padding: 10px;
  border: 1px solid blue;
}

.pokemon:focus {
  border: 5px solid yellow;
}
</style>
