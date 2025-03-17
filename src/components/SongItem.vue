<template>
    <li 
      @click="playSong(song.songLink, song.artist, song.title)"
      class="flex items-center p-4 border-b border-gray-300 hover:bg-gray-100 cursor-pointer transition rounded-lg"
    >
      <img 
        :src="song.singerPictureLink" 
        :alt="song.artist" 
        class="w-16 h-16 rounded-full shadow-lg"
      />
      <p class="ml-4 text-lg font-medium text-gray-800">
        <strong>{{ song.artist }}</strong> - {{ song.title }}
      </p>
    </li>
  </template>
  
  <script>
  export default {
    props: {
      song: Object
    },
    methods: {
      playSong(url, artist, title) {
       this.$emit('play', url);
        this.$emit('selectedArtist', artist);
        this.$emit('selectedTitle', title);
       
      }
    }
  };
  </script>
  
  <style scoped>
  /* Custom Styles for SongItem */
  </style>
  