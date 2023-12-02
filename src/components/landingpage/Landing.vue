<template>
  <div class="parent">
    <div class="mainDiv">
      <span>Sort By</span>
      <div class="turbricSorts">
        <div class="headers">Twubric Score</div>
        <div class="headers">
          Friends
          <img
            @click="sortUsers('friends', 'desc')"
            @keydown.enter.prevent="sortUsers('friends', 'desc')"
            :src="imgurl"
            alt=""
            tabindex="0"
          />
        </div>
        <div class="headers">
          Influence
          <img
            @click="sortUsers('influence', 'desc')"
            @keydown.enter.prevent="sortUsers('influence', 'desc')"
            :src="imgurl"
            tabindex="0"
          />
        </div>
        <div class="headers">
          Chirpiness
          <img
            @click="sortUsers('chirpiness', 'desc')"
            @keydown.enter.prevent="sortUsers('chirpiness', 'desc')"
            :src="imgurl"
            tabindex="0"
          />
        </div>
      </div>
    </div>

    <div class="mainDiv">
      <span>Joined Twitter between</span>
      <div class="turbricSorts">
        <div class="headers" style="width: 50%">
          <div><input type="date" v-model="startDate" name="startDate" id="" />start Date</div>
          <div>
            <button :disabled="!startDate" class="btn" @click="clearDate('start')">
              Clear Start Date
            </button>
          </div>
        </div>
        <div class="headers" style="width: 50%">
          <div>
            <input
              type="date"
              v-model="endDate"
              :title="startDate ? 'S ' + endDate : 'Please select a start date first'"
              :disabled="!startDate"
              name="endDate"
              id=""
            />End Date
          </div>
          <div>
            <button :disabled="!endDate" class="btn" @click="clearDate('end')">
              Clear End Date
            </button>
          </div>
        </div>
      </div>

      <div class="msg" v-show="msg">"SORRY THERE IS NO DATA TO SHOW "</div>
    </div>
  </div>
  <div class="cardParentDiv">
    <shimmerCard v-if="shimmer" v-for="user in arr" :key="user" />
    <Card
      v-if="!shimmer"
      @remove-Ele="removeEle"
      v-for="user in userData"
      :key="user.uid"
      :cardData="user"
    />
  </div>
</template>

<!--===========================================================================================================================================-->
<!-------------------------------------------------- java script ----------------------- -->
<script setup>
import Card from '../card.vue'
import shimmerCard from '../shimmerCard.vue'
import { watch, ref, onMounted } from 'vue'
const apikey =
  'https://gist.githubusercontent.com/pandemonia/21703a6a303e0487a73b2610c8db41ab/raw/82e3ef99cde5b6e313922a5ccce7f38e17f790ac/twubric.json'
const imgurl =
  'https://firebasestorage.googleapis.com/v0/b/blog-dacc7.appspot.com/o/upandDown.svg?alt=media&token=199b40be-f25e-4e38-9c40-7a25421351ce'

const userData = ref(null)
const userDataCopy = ref(null)
let startDate = ref(null)
let endDate = ref(null)
let msg = ref(false)
let shimmer = ref(true)
const arr = [1, 2, 4, 5, 6, 7, 3]
const removeEle = (uid) => {
  ///remove the card
  userData.value = userData.value.filter((obj) => obj.uid !== uid)
}

onMounted(async () => {
  try {
    let response = await fetch(apikey)
    let data = await response.json()
    userData.value = data
    userDataCopy.value = JSON.parse(JSON.stringify(data))
    shimmer.value = false
    console.log('user Data', data)
  } catch (error) {
    console.error('Error fetching user data:', error)
  }
})
function clearDate(str) {
  if (str == 'start') {
    startDate.value = null
  } else {
    endDate.value = null
  }
}
function sortUsers(key, order) {
  ///sorting function for now i have only using desc order sorting
  this.userData.sort((a, b) => {
    const aValue = a.twubric[key]
    const bValue = b.twubric[key]

    if (order === 'asc') {
      return aValue - bValue
    } else if (order === 'desc') {
      return bValue - aValue
    }

  })
}

watch(userData, (newVal, oldVal) => {
  //if the userdata empty then show the msg
  if (newVal.length == 0) {
    msg.value = true
  } else {
    msg.value = false
  }
})
watch(endDate, (newVal, oldVal) => {
  //CHECKING FOR END DATE VALUE
  if (newVal) {
    filterUsersByDate1()
  }

  if (!newVal && !startDate.value) {
    userData.value = JSON.parse(JSON.stringify(userDataCopy.value))
  }
})

watch(startDate, (newVal, oldVal) => {
  //CHECKING FOR clearing DATE VALUE
  if (!newVal && !endDate.value) {
    userData.value = JSON.parse(JSON.stringify(userDataCopy.value))
  }
})

// ==========filter function for date ===============================
function filterUsersByDate1() {
  if (startDate.value && endDate.value) {
    const startTimestamp = new Date(startDate.value).getTime() / 1000 // Convert to seconds
    const endTimestamp = new Date(endDate.value).getTime() / 1000 // Convert to seconds
    let data = JSON.parse(JSON.stringify(userDataCopy.value))

    userData.value = data.filter((user) => {
      const joinDate = user.join_date
      return joinDate >= startTimestamp && joinDate <= endTimestamp
    })
    console.log('user data', userData.value)
  }
}
</script>
<!----==========================================================================================================================================--->
<!-- ------------------------------------------css-------------------------------------------------------------------------------- -->
<style scoped>
.parent {
  display: flex;
  flex-direction: column;
  gap: 3vh;
}
.mainDiv {
  display: flex;
  flex-direction: column;
  /* background: yellow; */
  color: #fff;
  gap: 7px;
}
.mainDiv span {
  padding: 2px;
}
.turbricSorts {
  display: flex;
  /* background: antiquewhite; */
  min-height: 4vh;
  align-items: center;
  color: aliceblue;
}
.headers {
  /* background: aquamarine; */
  width: 25%;
  min-height: 4vh;
  display: flex;
  align-items: center;
  gap: 4px;
  border: 2px sold;
  border: 1px solid;
  padding: 8px;
  border-radius: 5px;
  justify-content: space-between;
}
.headers img {
  cursor: pointer;
}
.cardParentDiv {
  width: 97%;
  display: flex;
  flex-wrap: wrap;
  gap: 17px;
  align-content: center;
  align-items: center;
  overflow: auto;
  padding: 20px;
}
.msg {
  background: #09101057;
  height: 49vh;
  font-size: xx-large;
  display: flex;
  justify-content: center;
  align-items: center;
}
.btn {
  padding: 7px;
  font-weight: 800;
  color: #c71f47;
  filter: drop-shadow(-4px 0px 2px gray);
  cursor: pointer;
}
.btn:disabled {
  cursor: auto;
}
</style>
