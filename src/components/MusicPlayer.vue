<template>
    <v-container class="music-player">
      <v-card class="mx-auto" max-width="600" elevation="0">
        <v-card-title class="headline">Music Player</v-card-title>
        <v-card-text>
          <v-row class="track-info" justify="center">
            <v-col cols="12" class="text-center">
              <v-img
                :src="currentTrack.image"
                max-width="200"
                class="mx-auto mb-4"
              ></v-img>
              <h2>{{ currentTrack.title }}</h2>
              <p>{{ currentTrack.artist }}</p>
            </v-col>
          </v-row>
          <audio ref="audio" :src="currentTrack.url" @timeupdate="updateTime"></audio>
          <v-row justify="center" class="controls">
            <v-icon large class="ma-2" @click="prevTrack">mdi-skip-previous</v-icon>
            <v-icon large class="ma-2" @click="playPause">{{ isPlaying ? 'mdi-pause-circle' : 'mdi-play-circle' }}</v-icon>
            <v-icon large class="ma-2" @click="stop">mdi-stop-circle</v-icon>
            <v-icon large class="ma-2" @click="nextTrack">mdi-skip-next</v-icon>
          </v-row>
          <v-row class="progress" align="center" justify="center">
            <v-col cols="2" class="text-center">{{ formatTime(currentTime) }}</v-col>
            <v-col cols="8">
              <v-slider
                v-model="currentTime"
                :max="currentTrack.duration"
                @input="seek"
                class="mx-4"
                color="orange"
              ></v-slider>
            </v-col>
            <v-col cols="2" class="text-center">{{ formatTime(currentTrack.duration) }}</v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-container>
  </template>
  
  <script>
  export default {
    data() {
      return {
        isPlaying: false,
        currentTime: 0,
        currentTrackIndex: 0,
        tracks: [
          {
            title: 'Now You Know (UMENIKNOW)',
            artist: 'Sean Mmg ft. YBW Smith,Lilmaina & danski',
            url: 'public/music/NOW YOU KNOW (UMENIKNOW) (feat. YBW Smith, Lilmaina & danski).mp3',
            duration: 240, // Duration in seconds
            image:'public/music/sean-mmg-now-you-know-umeniknow.jpg',
          },
          {
            title: 'Kale ka Dance',
            artist: 'Sean Mmg ft. Mejja & YBW Smith',
            url: 'public/music/SEANMMG-KALE KA DANCE feat. MEJJA & YBWSmith (OFFICIAL MUSIC VIDEO).mp3',
            duration: 300,
            image: 'public/music/ybw.jpg'
          },

          {
            title: 'Dance Ya Kudonjo',
            artist: 'YBW Smith ft. Sean Mmg',
            url: 'public/music/SEANMMG-DANCE YA KUDONJO feat. YBWSmith(OFFICIAL MUSIC VIDEO)(2).mp3',
            duration: 300,
            image: 'public/music/kudonjo.jpg'
          },

          {
            title: 'Dimba',
            artist: 'Gody Tennor',
            url: 'public/music/Dimba.mp3',
            duration: 300,
            image: 'public/music/dimba.jpg'
          },

          {
            title: 'Maandamano',
            artist: 'Bien ft. Breeder LW',
            url: 'public/music/Bien ft. Breeder LW - Maandamano (Lyric Visualizer).mp3',
            duration: 300,
            image: 'public/music/lyricalvizz.jpg'
          },

           {
            title: 'Badman Ting',
            artist: 'Vybz Kartel',
            url: 'public/music/Vybz Kartel - Badman Ting (Official Audio).mp3',
            duration: 300,
            image: 'public/music/kyartel.jpg'
          },
           {
            title: 'BAND4BAND',
            artist: 'Central Cee ft. Lil Baby',
            url: 'public/music/CENTRAL CEE FT. LIL BABY - BAND4BAND (MUSIC VIDEO).mp3',
            duration: 300,
            image: 'public/music/CENTRAL-CEE-FT.-LIL-BABY-BAND4BAND-screenshot-billboard-1548.webp'
          },

          {
            title: 'Not Like Us',
            artist: 'Kendrick Lamar',
            url: 'public/music/Kendrick Lamar - Not Like Us.mp3',
            duration: 300,
            image: 'public/music/Lamar.jpg'
          },
          {
            title: 'Taki Nataki',
            artist: 'Tipsy Gee',
            url: 'public/music/Tipsy Gee - Taki Nakati (Official Video).mp3',
            duration: 300,
            image: 'public/music/tipsyyy.jpg'
          },
          {
            title: 'Tic Tac Remix',
            artist: 'Ranzscooby ft. Mejja & Scar Mkadinali',
            url: 'public/music/RanzScooby - TIC TAC REMIX ft Mejja & Scar Mkadinali (Official Music Video).mp3',
            duration: 300,
            image: 'public/music/tictac.jpg'
          },
          // Add more tracks as needed
        ]
      };
    },
    computed: {
      currentTrack() {
        return this.tracks[this.currentTrackIndex];
      }
    },
    methods: {
      playPause() {
        const audio = this.$refs.audio;
        if (this.isPlaying) {
          audio.pause();
        } else {
          audio.play();
        }
        this.isPlaying = !this.isPlaying;
      },
      stop() {
        const audio = this.$refs.audio;
        audio.pause();
        audio.currentTime = 0;
        this.isPlaying = false;
      },
      prevTrack() {
        this.currentTrackIndex =
          (this.currentTrackIndex - 1 + this.tracks.length) % this.tracks.length;
        this.resetTrack();
      },
      nextTrack() {
        this.currentTrackIndex = (this.currentTrackIndex + 1) % this.tracks.length;
        this.resetTrack();
      },
      resetTrack() {
        const audio = this.$refs.audio;
        audio.pause();
        audio.currentTime = 0;
        this.currentTime = 0;
        if (this.isPlaying) {
          audio.play();
        }
      },
      updateTime() {
        this.currentTime = this.$refs.audio.currentTime;
      },
      seek() {
        this.$refs.audio.currentTime = this.currentTime;
      },
      formatTime(seconds) {
        const minutes = Math.floor(seconds / 60);
        const secs = Math.floor(seconds % 60);
        return `${minutes}:${secs < 10 ? '0' : ''}${secs}`;
      }
    },
    watch: {
      currentTime(newTime) {
        const audio = this.$refs.audio;
        if (Math.abs(audio.currentTime - newTime) > 1) {
          audio.currentTime = newTime;
        }
      }
    }
  };
  </script>
  
  <style scoped>
 
  .track-info h2 {
    font-size: 20px;
    margin: 10px 0;
  }
  .track-info p {
    font-size: 16px;
    margin: 0;
    color: #ccc;
  }
  .controls {
    margin-bottom: 20px;
  }
  .controls .v-icon {
    cursor: pointer;
    color: #ff9100;
    transition: color 0.3s;
  }
  .controls .v-icon:hover {
    color: #e68000;
  }
  .progress {
    margin-top: 20px;
  }
  </style>