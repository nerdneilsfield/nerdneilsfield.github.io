<<template>
  <div class="jiao">
  <hr />
  <p id="footer"> <i class="fa fa-copyright" aria-hidden="true"></i>
 {{ author }} 2011 - {{ year}} </p>

  </div>
</template>


<script>
export default {
  name: 'jiao',
  data() { return {
      author : "Dengqi",
      year : (new Date()).getFullYear()
  }
  }
}
</script>

<style>
#footer {
  text-align: center;
}
</style>



