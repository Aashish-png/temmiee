<template>
  
    <div class="card">
        <div class="first">
            <span class="firstschild">{{ cardData?.fullname }}</span> 
            <span class="firstschild" style="flex-grow: 1;">{{ cardData?.twubric?.total }}</span>
        </div>
        <div class="imgDiv">
            <img :src="cardData?.image" alt="">
        </div>
        <div class="first">
            <span class="firstschild">Friends  {{ cardData?.twubric?.friends}}</span>
            <span class="firstschild">Influence  {{ cardData?.twubric?.influence}}</span>
            <span class="firstschild">Chirpiness   {{ cardData?.twubric?.chirpiness}}</span>
        </div>
        <div class="first">
            <span class="firstschild">{{ formatJoinDate(cardData?.join_date) }}</span>
            <span class="firstschild removebtn"    @click="$emit('remove-Ele',cardData.uid)"> remove</span>
        </div>
    </div>


  </template>
  <!--js   -->
  <script setup>
 import { defineProps,defineEmits } from 'vue';

const props = defineProps({
  cardData: {
    type: Object,
    default: () => {},
  },
});
defineEmits(["remove-Ele"]);
const cardData=props.cardData
const formatJoinDate = (timestamp) => {

  const options = {
  timeZone: 'Asia/Kolkata', // Indian time zone
  month: 'short',
  day: 'numeric',
  year: 'numeric'
};
  const date = new Date(timestamp * 1000); // Multiply by 1000 to convert seconds to milliseconds
  return date.toLocaleDateString('en-US', options);
};
// console.log('Card Data: in card component', props.cardData);
  </script>

<!-- css start from here  -->
  <style scoped>

.card{
    background: #673AB7;
    width: 15rem;
    height: 14rem;
    display: flex;
    flex-direction: column;
    border-radius: 6px;
    margin-top: 2rem;
    margin-bottom: 2rem;
}
.first {
    background: brown;
    display: flex;
    justify-content: space-between;
    height: 4vh;
    align-items: center;
}
span.firstschild {
    background: aliceblue;
    border: 1px solid;
    height: inherit;
    display: flex;
  
    padding: 0 5px;
    align-items: center;
    justify-content: center;
}
.imgDiv {
    background: rgb(18, 20, 19);
    height: 12vh;
    border-radius: 11px;
    margin: 3px 0;
}
.imgDiv img{
   border-radius: 11px;
    width: 100%;
    height: 100%;
}
.removebtn{
    flex-grow: 1;
    cursor: pointer;
}

</style>
  