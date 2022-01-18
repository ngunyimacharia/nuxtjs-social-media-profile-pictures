<template>
  <div class="bg-gray-100 p-10 h-full min-h-screen w-full min-w-screen">

        <div class="text-center">
          <h3 class="text-lg leading-6 font-medium text-gray-900">
            Social media picture
          </h3>
          <p class="mt-1 mx-auto text-sm text-gray-500">
            Fetch profile pictures from either FaceBook, Twitter or Gravatar
          </p>
        </div>

    <div class="py-6 w-2/3 mx-auto">
        <div class="bg-white overflow-hidden sm:rounded-lg sm:shadow p-5">
          <form class="space-y-8" @submit.prevent="submit" @reset.prevent="reset">
            <div class="space-y-8 sm:space-y-5">
              <div>

                <div class="mt-6 sm:mt-5 space-y-6 sm:space-y-5">
                  <div class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:pt-5">
                    <label for="network" class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2">
                      Social network
                    </label>
                    <div class="mt-1 sm:mt-0 sm:col-span-2">
                      <select @change="unsubmit" required v-model="form.network" id="network" name="network" autocomplete="network" class="max-w-lg block focus:ring-indigo-500 focus:border-indigo-500 w-full shadow-sm sm:max-w-xs sm:text-sm border-gray-300 rounded-md">
                        <option v-for="network in networks" :key="network.value" :value="network">{{network.name}}</option>
                      </select>
                    </div>
                  </div>

                  <div v-if="form.network" class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:pt-5">
                      <label for="identifier" class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2">
                        {{ form.network.name }}
                      </label>
                      <div class="mt-1 sm:mt-0 sm:col-span-2">
                        <input @keypress="unsubmit" required v-model="form.identifier" :type="form.network.type" name="identifier" id="identifier" class="max-w-lg block w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:max-w-xs sm:text-sm border-gray-300 rounded-md" />
                        <p class="text-red-800 text-sm my-5">
                          {{form.network.hint}}
                        </p>
                      </div>
                  </div>
                </div>
              </div>
            </div>

            <div class="pt-5">
              <div class="flex justify-end">
                <button type="reset" class="bg-white py-2 px-4 border border-gray-300 rounded-md shadow-sm text-sm font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                  Reset
                </button>
                <button type="submit" class="ml-3 inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                  Fetch
                </button>
              </div>
            </div>
          </form>
        </div>
    </div>

    <div class="text-center">
      <button 
        v-for="sample in samples"
        :key="sample.network"
        @click="showSample(sample)"
        class="m-2 bg-white py-2 px-4 border border-gray-300 rounded-md shadow-sm text-sm font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
      >
        {{sample.text}}
      </button>
    </div>

      <div class="text-center">
        <cld-image class="mx-auto w-96" v-if="form.submitted == 'true'" :public-id="`${form.identifier}.jpg`" :type="form.network.value" :alt="`${form.network.value} - ${form.identifier}`">
        </cld-image>
      </div>
</div>
</template>

<script>

import md5 from "crypto-js/md5";

export default {
  name: 'IndexPage',
  data(){
    return {
      form:{
        network:null,
        identifier:'',
        submitted:false
      },
      networks:[
        {
          name:'Facebook ID (Numeric)', 
          value:'facebook',
          type:'number',
          hint:'For privacy protection reasons, Facebook no longer supports accessing user images based on the user name; only the application-specific numeric ID obtained through authentication.'
        },
        {
          name:'Twitter ID (Numeric)', 
          value:'twitter',
          type:'number',
          hint:'The Twitter user ID is the unique numeric obtained once the user has authenticated into your application'
        },
        {
          name:'Twitter Username (Alphanumeric)', 
          value:'twitter_name',
          type:'text',
          hint:'The Twitter username is the alphanumeric unique screen name for each twitter user account.'
        },
        {
          name:'Gravatar Email Address (Alphanumeric)', 
          value:'gravatar',
          type:'text',
          hint: 'The email address of the Gravatar user account'
        },
      ],
      samples:[
        { text:'Bill Clinton (Facebook)', network: 0, identifier: '65646572251'},
        { text:'Bill Clinton (Twitter ID)', network: 1, identifier: '1330457336'},
        { text:'Bill Clinton (Twitter Username)', network: 2, identifier: 'BillClinton'},
        { text:'Cloudinary (Gravatar)', network: 3, identifier: "info@cloudinary.com"},
      ]
    }
  },
  mounted(){
    this.form.network = this.networks[0];
  },
  methods:{
    unsubmit(){
      this.form.submitted='false';
    },
    submit(){
      if(this.form.network.value === 'gravatar'){
        this.form.identifier = md5(this.form.identifier);
      }
      this.form.submitted='true';
    },
    showSample(sample){
      this.unsubmit();
      this.form.network = this.networks[sample.network];
      this.form.identifier = sample.identifier;
      this.submit();
    },
    reset(){
      this.form = {
        network:null,
        identifier:'',
        submitted:false
      };
      this.form.network = this.networks[0];
    }
  }
}
</script>
