<template>
  <div class="w-full">
    <div class="flex bg-emerald-100  h-12  justify-between" id="header">
      <NuxtLink to="/">
        <IconsHammer />
      </NuxtLink>


      <div class="header-div">
        <ul class="flex items-center justify-end text-[#333333]  ">
          <NuxtLink v-if="loggedIn == false" to="/Account/Login/" class="button-div ">
            <a href="/" class="hover:text-emerald-700 text-[20px] py-2 px-4 cursor-pointer">Login</a>
          </NuxtLink>

          <div v-else>
            <a @click="openProfileModal = !openProfileModal" href="#"
              class="hover:text-emerald-700  text-[20px] py-2 px-4 cursor-pointer">Profile</a>
            <div class=" absolute w-[250px] shadow-xl border mt-3  top-10 right-48 bg-emerald-100 text-center"
              v-show="openProfileModal">
              <div class=" flex edit hover:bg-blue-500 hover:text-white justify-center">
                <div class="profile-icon my-auto">
                  <IconsAccount class="" />
                </div>
                <div>
                  <a href="/Account/Manage/">
                    <button class=" text-[20px] py-2 px-4 rounded-full border-emerald-700">
                      Edit Profile</button>
                  </a>
                </div>
              </div>
              <div class="flex logout  hover:bg-blue-400  hover:text-white  justify-center">
                <div class="logout-icon my-auto">
                  <IconsLogout />
                </div>
                <a href="/">
                  <button @click="logout()" class=" text-[20px] py-2 px-4 rounded-full border-emerald-700">
                    Logout</button>
                </a>
              </div>
            </div>
          </div>

          <NuxtLink to="/Account/Register/" class="button-div " v-if="loggedIn == false">
            <button class="hover:text-emerald-700 text-[20px] py-2 px-4 rounded-full border-emerald-700">Register</button>
          </NuxtLink>
          <a href="/" v-else>
            <button @click="logout()"
              class="hover:text-emerald-700 text-[20px] py-2 px-4 rounded-full border-emerald-700">
              Logout</button>
          </a>

          <div class="button-div relative w-full">
            <button
              class="hover:bg-emerald-700 text-[20px] transition-color delay-100 hover:text-white min-w-[180px] border border-emerald-700 rounded-full"
              @click="modal">Live Poll</button>
            <div class="absolute w-[340px] h-[120px] border mt-1 rounded-lg top-10 right-1 bg-emerald-100 text-center"
              v-show="openModal">
              <form @submit.prevent="createRoom">
                <label for="" class="">Room name</label>
                <input type="text" class="w-[90%] h-8 rounded-md my-2  border outline-none px-5 py-4 text-lg"
                  v-model="room_name">
                <button type="submit" class="rounded-lg bg-blue-500 w-24 h-8 text-white">Submit</button>

              </form>
            </div>
          </div>

        </ul>
      </div>
    </div>

  </div>
</template>
  
<script setup>



const openModal = ref(false)
const room_name = ref('')
const loggedIn = ref(false)
const openProfileModal = ref(false)
const logout = () => {
  console.log('logout')
  sessionStorage.removeItem('access')
  sessionStorage.removeItem('refresh')

}
onBeforeMount(() => {
  if (sessionStorage.getItem('access')) {
    loggedIn.value = true
  }
})
const createRoom = () => {

  const data = new FormData()
  data.append('name', room_name.value)
  data.append('url', location.host)

  const chatSocket = new WebSocket(`ws://localhost:8000/ws/socket-server/${room_name.value}/`)
  chatSocket.onmessage = (e) => {
    let data = JSON.parse(e.data)
    console.log('data:', data)
  }
  chatSocket.onclose = (e) => {
    console.log('connection closed')
  }
  chatSocket.onopen = (e) => {
    console.log("onopen", e)
  }
}

const modal = () => {
  // const event  = new Even
  openModal.value = !openModal.value


}
</script>

<style></style>