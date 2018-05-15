<template>
  <div>
    <h1>Results for: {{ $route.params.id }}</h1>
    <div v-if="albumExist">
      <div v-for="(album, index) in albumData">
        <Card
          :title="album.collectionCensoredName"
          :image="album.artworkUrl100"
          :artistName="album.artistName"
          :url="album.artistViewUrl"
          :color="picker(index)"
        />
      </div>
    </div>
    <div v-else>
      No matches found
    </div>

  </div>
</template>

<script>
  import axios from 'axios'
  import Card from '~/components/Card.vue';

  export default {
    asyncData({params}) {
      return axios.get(`https://itunes.apple.com/search?term=${params.id}&entity=album`)
        .then((response) => {
          return {albumData: response.data.results}
        });
    },
    methods: {
      picker(index) {
        return index % 2 == 0 ? 'red' : 'blue';
      }
    },
    components: {
      Card
    },
    middleware: 'search',
    computed: {
      albumExist() {
        return this.albumData.length > 0;
      }
    }
  }
</script>