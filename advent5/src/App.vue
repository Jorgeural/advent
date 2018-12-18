<template>
  <div id="app" class="app">
    <!-- <img src="./assets/logo.png"> -->
    <h1>Advent of code exercise 5</h1>
    <h2>Day 5: Alchemical Reduction</h2>
    <div class="app__text-container">
      <p>
        You've managed to sneak in to the prototype suit manufacturing lab. The Elves are making decent progress, but are still struggling with the suit's size reduction capabilities.
        While the very latest in 1518 alchemical technology might have solved their problem eventually, you can do better. You scan the chemical composition of the suit's material and discover that it is formed by extremely long polymers (one of which is available as your puzzle input).
        The polymer is formed by smaller units which, when triggered, react with each other such that two adjacent units of the same type and opposite polarity are destroyed. Units' types are represented by letters; units' polarity is represented by capitalization. For instance, r and R are units with the same type but opposite polarity, whereas r and s are entirely different types and do not react.
        For example:
        In aA, a and A react, leaving nothing behind.
        In abBA, bB destroys itself, leaving aA. As above, this then destroys itself, leaving nothing.
        In abAB, no two adjacent units are of the same type, and so nothing happens.
        In aabAAB, even though aa and AA are of the same type, their polarities match, and so nothing happens.
        Now, consider a larger example, dabAcCaCBAcCcaDA:
        <pre class="app__pre">
          dabAcCaCBAcCcaDA  The first 'cC' is removed.
          dabAaCBAcCcaDA    This creates 'Aa', which is removed.
          dabCBAcCcaDA      Either 'cC' or 'Cc' are removed (the result is the same).
          dabCBAcaDA        No further actions can be taken.
        </pre>
        After all possible reactions, the resulting polymer contains 10 units.
        How many units remain after fully reacting the polymer you scanned? (Note: in this puzzle and others, the input is large; if you copy/paste your input, make sure you get the whole thing.)
      </p>
    </div>
    <button @click="reduce()">Reduce</button>
    <button @click="polymerize()">Polymerize</button>

    <div class="app__results">
      <p>The provided polymer contains {{exampleLength}} chemical composites</p>
      <p v-if="reducedPolymer"> After reduction you ended up with a {{reducedPolymer}} composites polymer, YAY!</p>
    </div>
    
  </div>
</template>

<script>
import exampleJson from './example.json'
export default {
  name: "app",
  data() {
    return {      
      prueba: "dabAcCaCBAcCcaDA",
      exampleLength: exampleJson.example.length,
      reducedPolymer: 0
    };
  },
  methods: {
    reduce() {
      // let string = this.prueba
      let string = exampleJson.example
      let i = 0
      let changed = true

     while(changed) {
       let current = string[i]
       let next = string[i + 1]
       var isCurrentUpper = current === current.toUpperCase()

       if((isCurrentUpper && current.toLowerCase() === next) || (!isCurrentUpper && current.toUpperCase() === next)) {
         string = string.substring(0, i) + string.substring(i + 2, string.length);
        if(i !== 0) {
          i--
        }
       } else {
        if (i < string.length - 2){
          i++
        } else {
          changed = false;
        }
       }
     }
     console.log('Reduce ' + string.length)
     this.reducedPolymer = string.length
    },
    polymerize() {
        // let str = this.prueba
        let str = exampleJson.example
        let i = 0;
        while (str[i + 1] != undefined) {
            let a = str.charCodeAt(i);
            let b = str.charCodeAt(i + 1);
            if (a == b + 32 || a == b - 32) {
                str = str.substring(0, i) + str.substring(i + 2, str.length);
                i--;
            } else {
                i++;
            }
        }
        console.log('Polymerize ' + str.length)
        this.reducedPolymer = str.length
    }     
  }  
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1,
h2 {
  font-weight: normal;
}

.app__text-container{
  max-width: 1000px;
  margin: 0 auto;
}

p {
  text-align: justify
}

.app__pre {
  max-width: 500px;
  margin: 15px auto;
}

.app__results {
  max-width: 500px;
  margin: 0 auto;
}

.app__results > p {
  text-align: center;
}

a {
  color: #42b983;
}
</style>
