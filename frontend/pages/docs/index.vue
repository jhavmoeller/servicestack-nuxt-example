<template>
  <div>
    <h1>Docs</h1>
    <p>
      This page is built on the server. It serves as an example of a server-rendered page using a node.js container, or as a statically generated page (in case of a statically generated site).
    </p>
    <p>
      You can use <a target="_blank" href="https://nuxtjs.org/guide/commands#production-deployment">nuxt generate</a>, to serve this page as a static page.
      It uses <a target="_blank" href="https://nuxtjs.org/guide/async-data">asyncData</a> to fetch data prior to rendering.
    </p>
    <p>
      In this demo, when running "npm run generate", the content from the dist folder generated by nuxt are merged
      with the asp.net site files to serve them out of the asp.net wwwroot static file folder.
    </p>
    <p>
      You could further extend this by using dynamically generated static pages only known at built-time,
      see the <a target="_blank" href="https://nuxtjs.org/api/configuration-generate">generate property</a> api.
      A good example of this is the <a target="_blank" href="https://github.com/nuxt/nuxtjs.org/blob/master/nuxt.config.js#L31">nuxtjs.org src on GitHub</a>,
      which builds it's pages at build-time from <a target="_blank" href="https://github.com/nuxt/docs">an api that serves a GitHub repo via webhooks</a>.
    </p>
    <h2>Server Plugins</h2>
    <ul>
      <li v-for="plugin in plugins">{{plugin}}</li>
    </ul>
    <h2>Server Routes</h2>
    <table cellpadding="0" cellmargin="0">
      <thead>
        <tr><th>Request Type</th><th>Path</th><th>Verbs</th></tr>
      </thead>
      <tbody>
        <tr v-for="route in routes"><td>{{route.requestType}}</td><td>{{route.path}}</td><td>{{route.verbs.join(', ')}}</td></tr>
      </tbody>
    </table>

  </div>
</template>


<script>
import { getServerInfo } from '~/utils/api'

export default {
  async asyncData () {
    let response = (await getServerInfo()).result
    return { ...response }
  },
  data () {
    return {
      plugins: null,
      routes: null
    }
  }
}
</script>

<style scoped>
h2 { margin: 20px 0; }
table { border-collapse: collapse; border: 1px solid #cdcdcd; }
table thead { background-color: #cdcdcd; }
table td, table th { margin: 0; padding: 5px; border-top: 1px solid #cdcdcd}
p { margin-bottom: 20px; }
</style>
