<template>
  <div class="destination-list">
    <p v-if="isResult === false" class="no-result" >검색 결과가 없습니다.</p>
    <ul v-if="isResult === true">
      <li v-for="list in destinationList">
        <strong> {{ list.title }} </strong>
        <p> {{ list.addr1 }} </p>
      </li>
    </ul>
  </div>
</template>

<script>
    import axios from 'axios'
    import eventBus from '../main'
export default {
  name: 'destinationList',
  data () {
      return {
          serviceKey: 'QBK%2FlSyT0QTA%2F9qIlwhVlxwr%2FGep9McM%2B5c2y5DdJE4RuhiylG9vaYGiYIqz%2F4UamfDAYURZ84JjA%2FlIq6spwA%3D%3D',
          destinationList: [],
          isResult: false
      }
  },
  methods: {

  },
    created () {
        const self = this;
        eventBus.$on('keyword', function (data) {
            const mobileApp = 'vue_study';
            const numOfRows = '100';
            const contentTypeId = '12';
            const keyword = data;

            axios.get('http://api.visitkorea.or.kr/openapi/service/rest/KorService/searchKeyword?serviceKey=' + self.serviceKey +
                '&MobileApp='+mobileApp+
                '&MobileOS=ETC&pageNo=1&numOfRows='+numOfRows+
                '&listYN=Y&arrange=A&contentTypeId='+contentTypeId+
                '&keyword='+keyword+'')
                .then(function(result) {
                    self.destinationList = result.data.response.body.items.item;
                    self.isResult = true;
                })
                .catch(function(error) {
                    console.log(error)
                })

        });


    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .destination-list .no-result {padding-top:20px; font-size:12px; color:#999;}
  .destination-list ul {width:80%; margin:50px auto; background-color:rgba(255,255,255,0.7);}
  .destination-list ul li {list-style:none; text-align:left; padding-top:20px; border-bottom:1px solid #ddd; }
</style>
