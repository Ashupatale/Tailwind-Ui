<template>
  <div class="grid grid-cols-3 h-screen">
    <div class="col-span-1">
      <img src="@/assets/bg-main-desktop.png" />
    </div>
    <div
      class="col-span-2 h-screen flex items-center p-30 pl-96"
      v-if="AnotherComp"
    >
      <Greetings />
    </div>
    <div class="col-span-2 h-screen flex items-center p-30 pl-96" v-else>
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
              maxlength="25"
              placeholder="e.g. Jane Appleased"
              v-model="cardholderName"
              @input="restrictSpecialChars"
            />
            <pre v-if="invalidInput" style="color: #ff0000">Invalid input</pre>
            <span class="text-start text-red-700" v-if="NameError"
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
              class="input shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              id="usernumber"
              type="text"
              pattern="[0-9]+"
              maxlength="19"
              placeholder="e.g. 1234 5678 9123 0000"
              v-model="cardholderNumber"
              @input="InvalidString"
            />
            <pre v-if="invalidInputString" style="color: #ff0000">
Invalid input</pre
            >

            <span class="text-start text-red-700" v-if="NumError"
              >Input feild cannot be empty</span
            >

            <span class="text-start text-red-700" v-if="WrongNum"
              >Card details are not valid</span
            >
          </div>

          <div class="grid grid-cols-4 gap-2">
            <label
              class="block text-gray-700 text-sm font-bold mb-2 text-start col-span-1 uppercase mr-2"
              for="username"
            >
              exp. date
            </label>
            <label
              class="block text-gray-700 text-sm font-bold mb-2 text-start col-span-1 uppercase"
              for="username"
            >
              (mm / yy)
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
              maxlength="2"
              class="col-span-1 shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              v-model="cardholderMonth"
              @input="InvalidDate"
            />

            <input
              type="text"
              id="password"
              placeholder="YY"
              maxlength="2"
              class="col-span-1 shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              v-model="cardholderYear"
              @input="InvalidYear"
            />

            <input
              type="text"
              id="password"
              placeholder="e.g. 123"
              maxlength="3"
              class="col-span-2 shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              v-model="cardholderCvv"
              @input="InvalidCvv"
            />
          </div>
          <span class="text-start text-red-700" v-if="DateErr"
            >Input feild cannot be empty</span
          >
        </form>
        <div>
          <button
            :disabled="disabled"
            class="w-full rounded-md bg-[#21092F] py-2 px-3 text-white"
            @click="SubmitForm"
          >
            <span v-if="!Loader">Confirm</span>
            <span v-if="Loader">
              <i class="fa-solid fa-spinner fa-spin-pulse fa-spin-reverse"></i>
            </span>
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
              class="outline-none w-full bg-transparent mt-10 text-start"
              style="font-family: 'Space Grotesk', sans-serif; font-size: 28px"
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
              <span v-if="cardholderName" class="uppercase text-start">{{
                cardholderName
              }}</span>
              <span v-else>JANE APPLESEED</span>
            </div>

            <div class="w-1/4 flex flex-col">
              <span
                v-if="cardholderMonth || cardholderYear"
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

    <div class="absolute top-2/4 left-80 mt-3">
      <div
        id="card"
        class="relative w-96 h-60 rounded-2xl font-mono text-white overflow-hidden cursor-pointer"
      >
        <div
          class="absolute top-0 left-0 w-full h-full justify-center bg-gradient-to-tr from-gray-100 to-gray-200"
        >
          <div class="w-full h-12 bg-slate-600 mt-4"></div>

          <div class="h-8 bg-slate-400 mt-5 w-80 mx-auto rounded">
            <span class="float-right pr-3 mt-1" v-if="cardholderCvv">{{
              cardholderCvv
            }}</span>
            <span
              v-else
              class="float-right pr-3 mt-1"
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
import Greetings from './Greetings.vue'
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  components:{
    Greetings
  },
  data(){
    return {
      disabled:false,
      cardholderName:"",
      cardholderNumber:"",
      cardholderMonth:"",
      cardholderYear:"",
      cardholderCvv:"",
      NameError:false,
      NumError:false,
      WrongNum:false,
      DateErr:false,
      invalidInput: false,
      invalidInputString:false,
      AnotherComp:false,
      Loader:false

    }
  },
  watch: {
    cardholderName(){
      if(this.cardholderName !== null){
        this.NameError=false
      }
    },
    cardholderCvv(){
      if(this.cardholderMonth || this.cardholderYear || this.cardholderCvv !== null ){
          this.DateErr=false
        }
    },
    cardholderNumber() {
      // Card number without dash (-)
        let realNumber = this.cardholderNumber?.replace(/-/gi, '')

        // Generate dashed number
        let dashedNumber = realNumber?.match(/.{1,4}/g,'')

        // Replace the dashed number with the real one
        this.cardholderNumber = dashedNumber?.join('-')


        if(this.cardholderNumber !== null ){
          this.NumError=false
          this.WrongNum=false
        }

        if(this.cardholderNumber?.length < 18 ){

          this.WrongNum=true
        }
        else if(this.cardholderNumber?.length == 19){
          this.disabled=false
        }
        else if(this.cardholderNumber == null){
          this.NumError =true
        }


    }


  },

  methods:{
    restrictSpecialChars() {
          if (/[^A-Za-z," "]/.test(this.cardholderName)) {
            this.invalidInput = true;
          } else {
            this.invalidInput = false;
          }

      },

      InvalidString(event){
        const input = event?.target?.value;
      const cleanedInput = input?.replace(/[^0-9-]/g, '-');
      this.cardholderNumber = cleanedInput;

      },
      InvalidDate(event){
        const input = event?.target?.value;
      const cleanedInput = input?.replace(/[^0-9-]/g, '0');
      this.cardholderMonth = cleanedInput;


      },
      InvalidYear(event){
        const input = event?.target?.value;
      const cleanedInput = input?.replace(/[^0-9-]/g, '0');
      this.cardholderYear = cleanedInput;


      },
      InvalidCvv(event){
        const input = event?.target?.value;
      const cleanedInput = input?.replace(/[^0-9-]/g, '0');
      this.cardholderCvv = cleanedInput;


      },
    SubmitForm(){
      if(this.cardholderNumber?.length < 19){
        this.disabled=true
      }
      else{
        this.disabled=false
      }

       if(this.cardholderMonth || this.cardholderYear || this.cardholderCvv || this.cardholderName||this.cardholderNumber){
        this.NumError=false;
        this.DateErr=false;
        this.NameError=false;
      }
       if(this.cardholderMonth &&  this.cardholderYear &&  this.cardholderCvv &&  this.cardholderName && this.cardholderNumber !== ""){
        this.Loader=true
        setTimeout(()=>{
        this.AnotherComp=true
       },3000)


      }
      else if(this.cardholderName||this.cardholderNumber || this.cardholderMonth || this.cardholderYear || this.cardholderCvv == "" || this.cardholderNumber?.length < 19 ){
        this.NumError=true
        this.DateErr=true
        this.NameError=true
      }
    }
  }


};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
