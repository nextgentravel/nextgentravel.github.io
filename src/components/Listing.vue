<template>
  <div>
    <div class="container pt-3 pb-6">
      <h3>NextGEN Travel GitHub Projects</h3>
      <p>Experiments and prototypes of tools created to test with users and help discover what makes a user-centric travel service.</p>
      <div class="card mb-3" v-for="(repository, index) in repositories" :key="index">
        <div class="card-header">
          <strong><a v-bind:href="repository.html_url" target="_blank">{{repository.name}}</a></strong>
        </div>
        <div class="card-body">
          <p class="card-text">{{repository.description}}</p>
          <p><strong>Demonstration: </strong><a v-bind:href="repository.homepage" target="_blank">{{repository.homepage}}</a></p>
        </div>
        <div class="card-footer">
          <div class="row">
            <div class="col"><small>{{repository.language}}</small></div>
            <div class="col"></div>
            <div class="col"><small><strong>Created:</strong> {{moment(repository.created_at).format("MMM D YYYY")}}</small></div>
            <div class="col"><small><strong>Updated:</strong> {{moment(repository.updated_at).format("MMM D YYYY")}}</small></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
// function b64_to_utf8( str ) {
//   return decodeURIComponent(escape(window.atob( str )));
// }

export default {
  name: 'Listing',
  created() {
    this.fetchData()
  },
  data() {
    return {
      loading: false,
      repositories: [],
      moment,
    }
  },
  methods: {
    fetchData() {
      this.loading = true
      fetch('https://api.github.com/orgs/nextgentravel/repos?sort=updated')
        .then(response => response.json())
        .then(data => {
          let urls = []
          data.forEach(item => {
            urls.push(item.html_url)
          })
          // Promise.all(data.map(url =>
          //     fetch(url.url + "/readme").then(resp => resp.json())
          // )).then(jsons => {
          //    jsons.forEach(json => {
          //      console.log(b64_to_utf8(json.content))
          //    })
          // }).catch(err => console.log(err))

          this.repositories = data
          this.loading = false
        })
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
