<template>
  <div class="grid grid-cols-3 h-screen">
    <div class="col-span-1 ">
      <img src="@/assets/bg-main-desktop.png"/>
    </div>
    <div class="col-span-2 h-screen flex items-center p-30 pl-96">
      <div class="w-full max-w-xs">
        <form class="bg-white rounded pt-6 pb-8 mb-4">
          <div class="mb-4">
            <label
              class="block text-gray-700 text-sm font-bold mb-2 text-start uppercase"
              for="username"
            >
              cardholder name
            </label>
            <input
              class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              id="username"
              type="text"
              placeholder="e.g. Jane Appleased"
              v-model="cardholderName"
            />
            <span class="text-start text-red-700" v-if="NumError"
              >Input feild cannot be empty</span
            >
          </div>
          <div class="mb-4">
            <label
              class="block text-gray-700 text-sm font-bold mb-2 text-start uppercase"
              for="username"
            >
              card number
            </label>
            <input
              class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              id="usernumber"
              type="text"
              placeholder="e.g. 1234 5678 9123 0000"
              v-model="cardholderNumber"
            />
            <span class="text-start text-red-700" v-if="NumError"
              >Input feild cannot be empty</span
            >
          </div>

          <div class="grid grid-cols-4 gap-2">
            <label
              class="block text-gray-700 text-sm font-bold mb-2 text-start col-span-1 uppercase mr-2"
              for="username"
            >
              exp.date
            </label>
            <label
              class="block text-gray-700 text-sm font-bold mb-2 text-start col-span-1 uppercase"
              for="username"
            >
              (mm/yy)
            </label>

            <label
              class="block text-gray-700 text-sm font-bold mb-2 text-start col-span-2 uppercase"
              for="username"
            >
              CVC
            </label>
          </div>

          <div class="grid grid-cols-4 gap-2">
            <input
              type="text"
              id="password"
              placeholder="MM"
              class="col-span-1 shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              v-model="cardholderMonth"
            />

            <input
              type="text"
              id="password"
              placeholder="YY"
              class="col-span-1 shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              v-model="cardholderYear"
            />

            <input
              type="text"
              id="password"
              placeholder="e.g. 123"
              class="col-span-2 shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              v-model="cardholderCvv"
            />
          </div>
          <span class="text-start text-red-700" v-if="NumError"
            >Input feild cannot be empty</span
          >
        </form>
        <div>
          <button
            class="w-full rounded-md bg-[#21092F] py-2 px-3 text-white"
            @click="SubmitForm"
          >
            Confirm
          </button>
        </div>
      </div>
    </div>

    <!-- Front content -->

    <div class="absolute top-20 left-52">
      <div
        id="card"
        class="relative w-96 h-60 rounded-2xl font-mono text-white overflow-hidden cursor-pointer transition-all duration-500"
      >
        <!-- Front content -->
        <div
          class="absolute top-0 left-0 bottom-0 w-full h-full flex flex-col justify-center gap-6 p-6 bg-gradient-to-r from-indigo-700 via-purple-800 to-pink-900 transition-all duration-100 delay-200 z-20"
        >
          <div class="flex items-center">
            <div class="w-12 h-12 bg-white rounded-full mr-5"></div>

            <div class="w-8 h-8 border-2 rounded-full"></div>
          </div>

          <!-- CardNumber -->
          <div class="">
            <label for="" class="hidden">Card Number</label>
            <span
              v-if="cardholderNumber"
              class="outline-none w-full bg-transparent text-3xl mt-10"
              >{{ cardholderNumber }}</span
            >
            <span
              v-else
              class="outline-none w-full bg-transparent text-3xl mt-10"
              style="font-family: 'Space Grotesk', sans-serif"
              >0000 0000 0000 0000</span
            >
          </div>

          <div class="w-full flex">
            <div class="w-full text-start">
              <span v-if="cardholderName.length" class="uppercase text-start">{{
                cardholderName
              }}</span>
              <span v-else>JANE APPLESEED</span>
            </div>

            <div class="w-1/4 flex flex-col">
              <span
                v-if="cardholderMonth.length || cardholderYear.length"
                class="uppercase text-start"
                >{{ cardholderMonth }}/{{ cardholderYear }}</span
              >
              <span v-else style="font-family: 'Space Grotesk', sans-serif"
                >00/00</span
              >
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Back content -->

    <div class="absolute top-2/4 left-80 mt-5">
      <div
        id="card"
        class="relative w-96 h-60 rounded-2xl font-mono text-white overflow-hidden cursor-pointer"
      >
        <div
          class="absolute top-0 left-0 w-full h-full justify-center bg-gradient-to-tr from-gray-100 to-gray-200"
        >
          <div class="w-full h-12 bg-slate-600 mt-5"></div>

          <div class="h-8 bg-slate-400 mt-5 w-80 mx-auto rounded">
            <span class="float-right pr-5 mt-1" v-if="cardholderCvv.length">{{
              cardholderCvv
            }}</span>
            <span
              v-else
              class="float-right pr-5 mt-1"
              style="font-family: 'Space Grotesk', sans-serif"
              >000</span
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="js">
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data(){
    return {

      cardholderName:"",
      cardholderNumber:"",
      cardholderMonth:"",
      cardholderYear:"",
      cardholderCvv:"",
      NumError:false
    }
  },
  watch: {
    cardholderNumber() {
      // Card number without dash (-)
        let realNumber = this.cardholderNumber?.replace(/-/gi, '')

        // Generate dashed number
        let dashedNumber = realNumber?.match(/.{1,4}/g)

        // Replace the dashed number with the real one
        this.cardholderNumber = dashedNumber?.join('-')

        if(this.cardholderNumber || this.cardholderName !== null ){
          this.NumError=false
        }
    }


  },

  methods:{
    SubmitForm(){
      if(this.cardholderNumber == null | " " ){
        this.NumError=true
      }
      else if(this.cardholderNumber.length){
        this.NumError=false
      }
    }
  }


};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
