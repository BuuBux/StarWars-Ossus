<template>
    <div class='search-text' >
      <h1>Ossus</h1>
      <p>Your place in web to get some knowledge about</p>
      <h3>🔥 Star Wars 🔥</h3>
    </div>
</template>

<script>

export default {
  name: 'Head',
  data(){
    return{

    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.search-text{
  color: #fff;
  text-align: center;
  width: 100%;
    h1 {
      font-size: 48px;
      line-height: 72px;
    }
    p {
      font-size: 20px;
      line-height: 26px;
      margin: 0 0 15px 0;
    }
    h3 {
      font-size: 24px;
      line-height: 32px;
    }
}
@media (min-width: 768px){
  .search-text{
    h1{
      font-size: 64px;
      line-height: 92px;
    }
    p{
      font-size: 24px;
      line-height: 32px;
    }
    h3{
      font-size: 32px;
      line-height: 36px;
    }
  }
}
@media (min-width: 768px){
  .search-text{
    h1{
      font-size: 72px;
      line-height: 92px;
    }
    p{
      font-size: 24px;
      line-height: 32px;
    }
    h3{
      font-size: 36px;
      line-height: 42px;
    }
  }
}
</style>
