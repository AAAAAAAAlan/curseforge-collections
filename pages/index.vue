<template lang="pug">
.container
  .input-field.flex.justify-between
    addonIdField(v-model="addonId" :value="addonId")
    button.add-addon(
      @click="getAddonInfo(addonId)"
      :disabled="isLoading"
    ) 
      span(v-if="!isLoading") Add
      loader(v-else)
  p.text-xs.text-gray-700.mt-1.ml-1 You can <span class="underline"><a target="_blank" href="https://i.imgur.com/uQ0Y1P9.png">find</a></span> project ID on curseforge addon page. For example 
    span.underline.cursor-pointer(@click="setAddonId(3358)")  DBM project id: 3358.
  transition-group(name="list" tag="p")
    addonCard(
      v-for="(addon, index) in addonList"
      :key="addon.id"
      :addon="addon"
      :index="index"
      @removeAddon="onRemoveAddon"
      )
  transition(name="list")
    button.create-collection(
      v-if="addonList.length >= 1"
      @click="getCollectionHash"
    ) Create Collection
  snackbar(
    ref="snackbar"
    baseSize="100px"
    :holdTime="3000"
    :multiple="true"
    position="bottom-center"
  )
</template>

<script>
import encode from 'object-encode'
import Snackbar from 'vuejs-snackbar'

import loader from '~/components/loader'
import addonIdField from '~/components/addonIdField'
import addonCard from '~/components/addonCard'

export default {
  components: {
    addonIdField,
    addonCard,
    loader,
    Snackbar,
  },
  data() {
    return {
      addonId: '',
      addonList: [],
      exportIds: [],
      isLoading: false,
    }
  },
  computed: {
    isDublicate() {
      return this.exportIds.includes(parseInt(this.addonId))
    },
  },
  methods: {
    async getAddonInfo(addonId) {
      if (addonId && !this.isDublicate) {
        try {
          this.isLoading = !this.isLoading
          const addonInfo = await this.$axios.get(
            `https://private-anon-2b7b944ff1-twitchappapi.apiary-proxy.com/api/v2/addon/${addonId}`
          )
          this.isLoading = !this.isLoading
          this.addonList.push(addonInfo.data)
          this.exportIds.push(addonInfo.data.id)
          this.addonId = ''
        } catch (error) {
          this.$refs.snackbar.error(error)
          this.isLoading = false
        }
      } else
        this.$refs.snackbar.error('Field is empty or addon already in list.')
    },
    getCollectionHash() {
      if (this.addonList.length >= 1) {
        const salt = 123
        const object = this.exportIds
        const encodedString = encode.encode_object(object, 'base64', salt)
        window.open(`http://localhost:3000/${encodedString}`)
      } else {
        this.$refs.snackbar.error('No mods in collection.')
      }
    },
    onRemoveAddon(index) {
      this.addonList.splice(index, 1)
      this.exportIds.splice(index, 1)
    },
    setAddonId(id) {
      this.addonId = id
    },
  },
}
</script>

<style lang="stylus">
.container
  button
    height 50px
    background-color #f16436
    box-shadow 0 4px 6px -1px rgba(0,0,0,.1), 0 2px 4px -1px rgba(0,0,0,.06)
    &:hover
      opacity 0.8

  .input-field
    .add-addon
      margin-left 5%
      padding 5px 5%

  .create-collection
    width 100%


.list-item
  display: inline-block
  margin-right: 10px

.list-enter-active, .list-leave-active
  transition: all 0.5s

.list-enter, .list-leave-to
  opacity: 0
  transform: translateY(30px)
</style>
