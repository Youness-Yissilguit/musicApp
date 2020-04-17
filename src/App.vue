<template>
  <div id="app">
    <header>
      <h1><i class="fas fa-music"></i> My Music App <i class="fas fa-music"></i></h1>
      <section class="play">
        <h3 class="actuel-m">{{ current.title }}</h3>
        <div class="dj-circle"></div>
        <div class="display">
          <span @click="repeat" class="repeat"><i class="fas fa-redo"></i></span>
          <span @click="prev" class="prev"><i class="fas fa-step-backward"></i></span>
          <span @click="play" v-if="!isPlay" class="play-btn">
            <i class="fas fa-play"></i>
          </span>
          <span @click="pause" v-else class="play-btn">
            <i class="fas fa-pause"></i>
          </span>
          <span @click="next" class="next"><i class="fas fa-step-forward"></i></span>
          <span v-if="isMute" @click="notmute" class="muted"><i class="fas fa-volume-up"></i></span>
          <span v-else @click="mute" class="muted"><i class="fas fa-volume-mute"></i></span>
        </div>
      </section>
      <section class="playlist">
        <h2>The Playlist</h2>
        <div v-for="song in songs" v-bind:key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'music active' : 'music'">
          <img :src="song.imgSrc"/>
          <p>{{ song.title }}</p>
        </div>
      </section>
    </header>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      current: {},
      index: 0,
      isPlay: false,
      isMute: false,
      songs: [
        {
          title: 'Bensound - Creativeminds',
          src: require('../src/assets/bensound-creativeminds.mp3'),
          imgSrc: require('../src/assets/1.jpg')
        },
        {
          title: 'Homer - Said',
          src: require('../src/assets/Homer_Said.mp3'),
          imgSrc: require('../src/assets/2.jpg')
        },
        {
          title: 'Jim Yosef - Firefly',
          src: require('../src/assets/Jim-Yosef.mp3'),
          imgSrc: require('../src/assets/3.jpg')
        },
        {
          title: 'Johnning - Dont Want U Back',
          src: require('../src/assets/Johnning.mp3'),
          imgSrc: require('../src/assets/4.jpg')
        },
        {
          title: 'Kisma - Fingertips',
          src: require('../src/assets/Kisma-Fingertips.mp3'),
          imgSrc: require('../src/assets/5.jpg')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song){
      if(typeof song.src != 'undefined'){
        this.current = song;
        this.player.src = this.current.src;
      }
      //play the music on the click
      this.player.play();
      this.isPlay = true;
    },
    pause(){
      this.player.pause();
      this.isPlay = false;
    },
    next(){
      this.index++;
      if(this.index > this.songs.length - 1){
        this.index = 0; 
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev(){
      this.index--;
      if(this.index < 0){
        this.index = this.songs.length - 1; 
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    repeat(){
      this.player.currentTime = 0
    },
    mute(){
      this.player.muted = true;
      this.isMute = true;
    },
    notmute(){
      this.player.muted = false;
      this.isMute = false;
    }
  },
  created (){
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    this.player.volume = .2
  }
}
</script>

<style>
body{
  margin: 0;
  padding: 0;
  font-family: cursive;
  background-color: #f0f0f0
}
*{
  margin: 0;
  padding: 0
}
header{
  width: 100%;
  min-height: 100%;
  background: #f857a6;
  background: -webkit-linear-gradient(to right, #ff5858, #f857a6);
  background: linear-gradient(to right, #ff5858, #f857a6); 
}
@media (min-width: 568px) {
  header{
    width: 40%;
    margin: 0 auto
  }
}
header h1{
  text-align: center;
  padding: 40px 0;
  font-weight: normal;
  color: #fff
}
header .play{
  background-color: #fff;
  border-radius: 60px 60px 0 0;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.actuel-m{
  margin: 15px 0 0;
  font-weight: normal;
  color: rgba(0, 0, 0, .8);
}
header .play .dj-circle{
  width: 170px;
  height: 170px;
  border-radius: 50%;
  background: url('../src/assets/bg.jpg') no-repeat;
  background-size: cover;
  margin: 30px 0;
}
header .play .display{
  width: 100%;
  margin: 0 auto 20px;
  display: flex; 
  justify-content: space-around;
  align-items: center;
  text-align: center;
}
header .play .display .play-btn{
  display: inline-block;
  width: 60px;
  height: 60px;
  cursor: pointer;
  background: linear-gradient(to top, #ff5858, #f857a6);
  color: #fff;
  text-align: center;
  line-height: 60px;
  border-radius: 50%;
  font-size: 22px;
  transition: all .4s;
}
header .play .display .play-btn:hover{
  background: linear-gradient(to bottom, #ff5858, #f857a6);
}
header .play .display .prev,
header .play .display .next,
header .play .display .repeat,
header .play .display .muted{
  font-size: 20px;
  color: rgba(0, 0, 0, .9);
  cursor: pointer
}
header .play .display .repeat,
header .play .display .muted{
  font-size: 18px
}
header .playlist{
  background-color: #f2f2f2;
  padding: 10px;
}
header .playlist h2{
  padding: 15px;
}
header .playlist .music{
  padding: 15px;
  background-color: #fff;
  display: flex;
  align-items: center;
  border-radius: 5px;
  margin-bottom: 10px;
  cursor: pointer;
  transition: all .4s ease-in-out;
}
header .playlist .music:hover,
header .playlist .music.active{
  color: #fff;
  background: linear-gradient(to left, #ff5858, #f857a6);
}
header .playlist .music img{
  width: 40px;
  height: 40px;
  display: inline-block;
  border-radius: 50%;
  margin-right: 15px
}
</style>
