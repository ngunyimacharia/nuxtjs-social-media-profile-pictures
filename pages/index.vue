<template>
  <div class="bg-gray-100 p-10 h-full min-h-screen w-full min-w-screen">

        <div class="text-center">
          <h3 class="text-lg leading-6 font-medium text-gray-900">
            Social media picture
          </h3>
          <p class="mt-1 max-w-2xl text-sm text-gray-500">
            Fetch profile pictures from either FaceBook, Twitter or Gravatar
          </p>
        </div>

    <div class="py-6 grid grid-cols-2">
      <div>
        <div class="bg-white overflow-hidden sm:rounded-lg sm:shadow p-5">
          <form class="space-y-8" @submit.prevent="submit">
            <div class="space-y-8 sm:space-y-5">
              <div>

                <div class="mt-6 sm:mt-5 space-y-6 sm:space-y-5">
                  <div class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:pt-5">
                    <label for="network" class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2">
                      Social network
                    </label>
                    <div class="mt-1 sm:mt-0 sm:col-span-2">
                      <select required v-model="form.network" id="network" name="network" autocomplete="network" class="max-w-lg block focus:ring-indigo-500 focus:border-indigo-500 w-full shadow-sm sm:max-w-xs sm:text-sm border-gray-300 rounded-md">
                        <option value="facebook">Facebook</option>
                        <option value="twitter">Twitter</option>
                        <option value="gravatar">Gravatar</option>
                      </select>
                    </div>
                  </div>

                  <div class="sm:grid sm:grid-cols-3 sm:gap-4 sm:items-start sm:pt-5">
                      <label for="identifier" class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2">
                        Identifier (username)
                      </label>
                      <div class="mt-1 sm:mt-0 sm:col-span-2">
                        <input required v-model="form.identifier" type="text" name="identifier" id="identifier" autocomplete="given-name" class="max-w-lg block w-full shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:max-w-xs sm:text-sm border-gray-300 rounded-md">
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

      <div>
        image
      </div>
    </div>

</div>
</template>

<script>
export default {
  name: 'IndexPage',
  data(){
    return {
      form:{
        network:'facebook',
        identifier:''
      },
      networks:[
        {name:'Facebook', value:'facebook'},
        {name:'Twitter', value:'twitter'},
        {name:'Gravatar', value:'gravatar'},
      ],
      image:null
    }
  },
  methods:{
    submit(){
      this.image = `https://res.cloudinary.com/demo/image/${this.form.network}/${this.form.identifier}.jpg`;
      console.log(this.image);
    }
  }
}
</script>
