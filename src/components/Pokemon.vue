vue <template>
    <div class="container">
      <div>
        <img :src="imagen" alt="none" />
      </div>
      <div><label for="">{{ nombre }}</label></div>
     
    </div>
  </template>
  
  <script>
  
  export default {
  
  props:{
    imagen: String,
    nombre:{
      type:String,
      default: "XXXX"
    }
  
  }
  
    
  };
  </script>
  
  <style>
  
  img{
  width: 100%;
  max-width: 100vw;
  height: 100%;
  max-height: 100vh;
}

  
  
  </style>