<template>
  <div id="app">
    <issueslist></issueslist>
    <jiao></jiao>
  </div>
</template>

<script>
import Jiao from './components/footer'
import Issueslist from './components/issues'
export default {
  name: 'app',
  components: {
    Issueslist,
    Jiao
  }
}
</script>

<style>
#app {
  max-width: 940px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*color: #2c3e50;*/
  margin-top: 0px;
  padding:1% 10%; position:relative;
}

pre, code {
white-space: pre-wrap; /* css-3 /
white-space: -moz-pre-wrap !important; / Mozilla, since 1999 /
white-space: -pre-wrap; / Opera 4-6 /
white-space: -o-pre-wrap; / Opera 7 /
word-wrap: break-word; / Internet Explorer 5.5+ */
}


html{ background:#eee; }

body{ width:90%; max-width: 960px; background:#fff; margin:3em auto 0; padding-top:2em;border:1px solid #ddd;border-width:0 1px;}

.img {
    text-align: center;
    background-color: white;
    background-image: url('/static/fire.svg');
    background-size: contain;
    background-position: 50% 0;
    background-repeat: no-repeat;
    height: 200px;
}

code {
  max-width: 900px;
}

div.shit {
  max-width: 256px;
  max-height: 256px;
  text-align: center;
}
</style>
