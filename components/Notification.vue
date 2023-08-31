<template>
    <div v-show="!auctions[0]"
        class="bg-[#dff0d8] my-3 w-full text-[#468847] text-center h-16 rounded-md border border-[#baccb3]">
        <h1>You are currently not subscribed to any auction. Please browse to an auction in the All Auctions section below
        </h1>
    </div>
    <div class="border mt-3 flex-wrap" v-for="auction in auctions[0]">
        <div class="text-center border hover:bg-gray-200 ">

            <NuxtLink :to="`/Auction/${auction.id}`">
                <div class="text-white text-sm w-0 h-0 border">
                    <div v-if="auction.status == 'ACTIVE'"
                        class="flex w-32 h-7 m-2 bg-[#73a839] rounded-md justify-center gap-2">
                        <div class="">
                            <IconsHammer class="w-5 h-7" />
                        </div>


                        <div class="my-1">

                            <p>{{ auction.status }}</p>
                        </div>
                    </div>
                    <div v-else="auction.status == 'PREPARING'"
                        class="flex w-32 h-7 m-2 bg-blue-500 rounded-md justify-center gap-2">
                        <div class="">
                            <IconsSettings class="w-5 h-7" />
                        </div>
                        <div class="my-1">

                            <p>{{ auction.status }}</p>
                        </div>

                    </div>
                </div>
                <div>
                    <h1 class=" text-blue-800 text-center text-3xl font-semibold">{{ auction.title }}</h1>
                    <p class="text-xs"><strong>SCHEDULED CLOSURE DATE:</strong>AUGUST 13,2023, 15:00</p>
                    <p class="text-[#73a839] text-sm">GMT+0300(EAST AFRICA TIME)</p>
                </div>
            </NuxtLink>
        </div>
        <div class="flex">
            <div class="w-[50%] text-center border">
                <h1 class="text-3xl font-semibold"> {{ auction.lot_count }} </h1>
                <div class="flex justify-center w-[50%] mx-auto gap-1 ">
                    <IconsProgress class="my-1" />

                    <p>Lots In Auction</p>
                </div>
                <div class="flex border justify-around h-8">
                    <div class="">winning</div>
                    <div class="">Losing</div>
                </div>
            </div>

            <div class="w-[50%] text-center border">
                <h1 class="text-3xl font-semibold">6</h1>
                <div class=" flex justify-center w-[50%] mx-auto gap-1 ">
                    <IconsCalendar class="my-1" />
                    <p>Days to Closure</p>
                </div>
                <div class="option border">
                    <div @click="areyousure()" v-show="unsubscribe" class="unsubscribe  flex  justify-center">
                        <button class="">unsubscribe</button>
                    </div>
                    <div v-show="!unsubscribe" class="  confirm-unsubscribe flex justify-around">
                        <div @click="unsubscribeAuction(auction.id)" class="confirm w-[50%]  ">
                            <button>confirm</button>
                        </div>
                        <div @click="unsubscribe = !unsubscribe" class="cancel w-[50%]">
                            <button>x Cancel</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios';
export default {
    data() {
        return {
            auctions: [],
            unsubscribe: true
        };
    },

    async created() {
        if (process.client) {
            const token = sessionStorage.getItem('access')
            const config = {
                headers: {
                    Authorization: `Bearer ${token}`
                }
            }
            if (token) {
                const response = await axios.get("http://localhost:8000/Auction/subscribed/", config)
                console.log(response.data.length)
                this.auctions.push(response.data)
            }
        }
    },
    methods: {
        areyousure() {
            this.unsubscribe = !this.unsubscribe
            console.log(this.unsubscribe)
        },
        async unsubscribeAuction(id) {
            console.log('unsubscribe from ', id)
            const token = sessionStorage.getItem("access")

            if (token) {
                axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;

                const url = `http://localhost:8000/Auction/subscribe/${id}/`
                const response = await axios.delete(url)
                console.log(response.data)
                const router = useRouter()
                router.go(0)
            }
        }


    }
}


</script>