<template>
  <div id="app">
    <header> 
      <h1> Bilge'nin Büyülü Müzik Çaları </h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{current.title}} -
          <span>{{current.artist}}</span>
        </h2>
        <div class="control">
          <button class="prev" @click="prev">önceki</button>
          <button class="play" v-if="!isPlaying" @click="play">oynat</button>
          <button class="pause" v-else @click="pause">duraklat</button>
          <button class="next" @click="next">sonraki</button>
        </div>
      </section>
      <section class="playlist">
        <h3>Çalma Listem</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
        {{song.title}} - {{song.artist}}
        </button>
      </section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data (){
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Grateful',
          artist: 'Neffex',
          src: require('./assets/neffex-grateful.mp3')
        },
        {
          title: 'Invincible',
          artist: 'Deaf Kev',
          src: require('./assets/deaf-kev-invincible.mp3')
        },
        {
          title: 'Son Durağın',
          artist: 'Kahraman Deniz',
          src: require('./assets/Kahraman-Deniz-Son-Durağın.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.current =song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function() {
        this.index++;
        if (this.index > this.songs.length - 1){
          this.index = 0;
        }

        this.current = this.songs[this.index];
        this.play(this.current);

      }.bind(this));
      this.isPlaying =true;

    },
    pause () {
      this.player.pause();
      this.isPlaying = false;

    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1){
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev () {
        this.index--;
      if (this.index < 0){
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);

    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    //this.player.play();
  }
  }
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}
header{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 90px;
  background-color: rgb(94, 77, 255);
  color: rgb(8, 187, 178);
  border-style: dashed;
  border-width: 6px;
  border-color: rgb(0, 59, 148);
}
main {
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
  padding: 45px;
}
.song-title {
  color: cadetblue;
  font-size: 45px;
  font-weight: 750;
  text-transform: uppercase;
  text-align: center;
  border-style: dotted;
  border-color: linear-gradient(to right,rgb(8, 187, 178),rgb(1, 28, 53)); 
  border-width: 9px;
}
.song-title span {
  font-weight: 4000;
  font-style: italic;
  
}

.control {
  display: flex;
  justify-content: center;
  align-items:center ;
  padding: 40px 70px;
}

button {
  appearance:none;
  background:rgb(12, 92, 88);
  border: 2px;
  outline: auto;
  cursor: pointer;
}
button:hover {
  opacity: 0.9;
}
.play {
  font-size: 30px;
  font-weight: 700px;
  padding: 15px 35px ;
  margin: 0px 15px;
  border-radius: 10px;
  color: rgb(5, 7, 7);
  background: rgb(19, 122, 218);
}
.next, .prev {
  font-size: 25px;
  font-weight: 700px;
  padding: 10px 25px ;
  margin: 0px 15px;
  border-radius: 8px;
  color: rgb(5, 7, 7);
  background: rgb(8, 187, 178);

} 
.playlist {
  padding: 20px 120px;

}
.playlist h3 {
  color: cadetblue;
  font-size: 45px;
  font-weight: 500;
  margin-bottom: 60px;
  text-align: center;
  font-style: italic;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 15px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover {
  color: aquamarine;

}
.playlist .song.playing {
  color: black;
  background-image: linear-gradient(to right,rgb(8, 187, 178),rgb(19, 122, 218));
}

</style>
