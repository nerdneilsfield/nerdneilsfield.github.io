<template>

<div class="test">
    <h1> {{ title  }} </h1>
    <p v-html="hama.name"></p>
    <button @click="greet">Hama</button>
    <input v-model="title" @keyup.enter="fuck">
</div>



</template>


<script>

export default {
name: 'test',
data() {
 return    {
    title: "Hello world",
    hama: {
       name: "<b>Jiangzeming!</b>"
       }
}
},
methods: {
greet: function() {
           this.title = "Fuck you!";
           alert("Hama Jiang zeming!");
       },
fuck: function(e) {
          console.log(e.target.value);
}
}
}

</script>

<style scoped>


</style>
