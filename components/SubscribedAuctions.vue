<template>
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
            </div>

            <div class="w-[50%] text-center border">
                <h1 class="text-3xl font-semibold">6</h1>
                <div class="flex justify-center w-[50%] mx-auto gap-1 ">
                    <IconsCalendar class="my-1" />
                    <p>Days to Closure</p>

                </div>
            </div>
        </div>
    </div>
    <!-- <div v-else>
        <div>
            <p>There are currently no auctions available,Please Come back latter</p>
        </div>
    </div> -->
</template>

<script>

import axios from 'axios';
export default {
    data() {
        return {
            auctions: []
        };
    },
    methods: {
        async getAuctions() {
            const response = await axios.get('http://localhost:8000/Auction/subscribed/');
            this.auctions.push(response.data);
            console.log('1', this.auctions);
        }
    },
    created() {
        this.getAuctions();
    },

}


</script>

