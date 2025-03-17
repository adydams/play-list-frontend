<template>
    <div class="max-w-lg mx-auto text-center p-6">
      <h1 class="text-3xl font-bold text-blue-600">AI Music Playlist Generator</h1>
      
      <ErrorMessage v-if="errorMessage" :message="errorMessage" />
  
      <!-- Genre Input -->
      <div class="mt-4 flex justify-center">
      
        <input 
          v-model="genre" 
          class="border border-gray-300 rounded-lg p-2 w-3/4 focus:outline-none focus:ring-2 focus:ring-blue-400"
          placeholder="Enter genre (e.g. rock, pop)" 
        />
        <button 
          @click="fetchPlaylist"
          class="ml-2 bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition"
        >
          Generate
        </button>
      </div>
  
      <!-- Loader and Error Handling -->
      <Spinner v-if="loading" />
        <div v-if="selectedArtist && selectedTitle" class="mt-6">
  <p class="ml-4 text-lg font-medium text-blue-800">
    <strong>{{ selectedArtist }}</strong> - {{ selectedTitle }}
  </p>
      </div>

         <!-- Video Player -->
         <div v-if="selectedSong" class="mt-6">
          
        <iframe 
          width="100%" 
          height="315" 
          :src="selectedSong" 
          class="rounded-lg shadow-md"
          frameborder="0" 
          allowfullscreen
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        ></iframe>
      </div>

      <!-- Display Playlist -->
      <div v-if="playlist.length" class="mt-6">
        <h2 class="text-2xl font-semibold text-gray-700">Playlist for {{ genre }}</h2>
        <ul class="mt-4">
          <SongItem 
            v-for="song in playlist" 
            :key="song.songLink" 
            :song="song" 
            @play="playSong"            
            @selectedArtist="selectedArtist = $event"
            @selectedTitle="selectedTitle = $event"

          />
        </ul>
      </div>
  
   
    </div>
  </template>
  
  <script>
  import Spinner from './LoadingSpinner.vue';
  import ErrorMessage from './ErrorMessage.vue';
  import SongItem from './SongItem.vue';
  
  export default {
    components: {
      Spinner,
      ErrorMessage,
      SongItem
    },
    data() {
      return {
        genre: "",          // User input for genre
        playlist: [],       // API response (playlist)
        selectedSong: null, // Currently playing video
        loading: false,     // Loading state
        errorMessage: "" ,   // Error message state
        selectedArtist: null, // Currently playing video artist
        selectedTitle: null, // Currently playing video title
     
      };
    },
    methods: {
      async fetchPlaylist() {
        this.loading = true;
        this.errorMessage = "";
        if (!this.genre.trim()) {
          this.errorMessage = "You have not selected any genre. Defaulting to Gospel.";
          this.genre = "gospel"; // Set genre to "gospel" if no input
        }
  
        this.selectedSong = null;
        this.selectedArtist = null;
        this.selectedTitle = null;
        this.playlist = [];
  
        try {
          const response = await fetch(`http://127.0.0.1:5000/generate_playlist?genre=${this.genre}`);
          const data = await response.json();
          this.playlist = data.playlist;
        } catch (error) {
          this.errorMessage = "Error fetching playlist.";
        } finally {
          this.loading = false;
        }
      },
      playSong(url, artist, title) {
        const videoId = url.split("v=")[1]; // Extract YouTube video ID
        this.selectedSong = `https://www.youtube.com/embed/${videoId}`;
        this.selectedArtist = artist;
        this.selectedTitle = title;
       
      }
    }
  };
  </script>
  
  <style scoped>
  /* Custom Styles */
  </style>
  