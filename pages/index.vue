<template lang="pug">
.container
  .input-field.flex.justify-between
    addonIdField(v-model="addonId" :value="addonId")
    button.add-addon(
      @click="getAddonInfo(addonId)"
    ) Add
  addonCard(
    v-for="(addon, index) in addonList"
    :key="index"
    :addon="addon"
    )
  button.border-gray-800(@click="getCollectionHash") DEBUG CREATE HASH
</template>

<script>
import encode from 'object-encode'

import addonIdField from '~/components/addonIdField'
import addonCard from '~/components/addonCard'

export default {
  components: {
    addonIdField,
    addonCard,
  },
  data() {
    return {
      addonId: '',
      addonList: [],
      exportIds: [],
    }
  },
  methods: {
    async getAddonInfo(addonId) {
      if (addonId) {
        try {
          const addonInfo = await this.$axios.get(
            `https://private-anon-2b7b944ff1-twitchappapi.apiary-proxy.com/api/v2/addon/${addonId}`
          )
          this.addonList.push(addonInfo.data)
          this.exportIds.push(addonInfo.data.id)
        } catch (error) {
          console.log(error)
        }
      } else {
        console.log('field empty')
      }
    },
    getCollectionHash() {
      if (this.addonList.length > 1) {
        const salt = 123
        const object = this.exportIds
        const encodedString = encode.encode_object(object, 'base64', salt)
        console.log(`localhost:3000/${encodedString}`)
      } else {
        console.log('no mods in collection')
      }
    },
  },
}
</script>

<style lang="stylus">
.container
  .input-field
    .add-addon
      height 50px
      background-color #f16436
      margin-left 5%
      padding 5px 5%
      box-shadow 0 4px 6px -1px rgba(0,0,0,.1), 0 2px 4px -1px rgba(0,0,0,.06)
</style>
