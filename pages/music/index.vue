<template>
  <div class="container">
    <div class="table-responsive">
      <table class="table table striped">
        <thead>
          <th></th>
          <th>Name</th>
          <th>Artist</th>
          <th>Album</th>
        </thead>
        <tbody v-if="ip">
          <tr v-for="(track, index) in ip.recenttracks.track" :key="index">
            <td><img v-bind:src="track.image[0].text" /></td>
            <td>{{ track.name }}</td>
            <td>{{track.artist.text}}</td>
            <td>{{track.album.text}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import axios from '@nuxtjs/axios'
export default {
  async asyncData({ $axios }: any) {
    let ip: any

    await $axios
      .$get(
        'https://ws.audioscrobbler.com/2.0/?method=user.getrecenttracks&user=mistry7777&api_key=855415e650a164fbb41c99104700d651&format=json'
      )
      .then((res: any) => {
        let ipStr = JSON.stringify(res).split('#').join('')
        ip = JSON.parse(ipStr)
      })
    return { ip }
  },
}
</script>

<style>
</style>