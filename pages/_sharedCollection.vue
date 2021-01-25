<template lang="pug">
  .container
    h1.text-center.font-bold Copy this page's url and share it with your friends!
    p.text-center.mt-2 {{ `Unique collection id: ${$route.params.sharedCollection} `}}
    addonCard(
      v-for="(addon, index) in addons"
      :key="addon.id"
      :addon="addon"
      :index="index"
    )
    NuxtLink.create-new(
      to="/"
    ) Create new collection.
</template>

<script>
import encode from 'object-encode'

import addonCard from '~/components/addonCard'

export default {
  components: {
    addonCard,
  },
  async asyncData({ app, params }) {
    const salt = 123

    const decodedObject = encode.decode_object(
      params.sharedCollection,
      'base64',
      salt
    )

    const addons = []

    for (const id of decodedObject) {
      const x = await app.$axios.get(
        `https://private-anon-2b7b944ff1-twitchappapi.apiary-proxy.com/api/v2/addon/${id}`
      )
      addons.push(x.data)
    }
    return { addons }
  },
}
</script>

<style lang="stylus" scoped>
.create-new
  display block
  width 100%
  height 50px
  color white
  padding-top 18px
  text-align center
  background-color #f16436
  box-shadow 0 4px 6px -1px rgba(0,0,0,.1), 0 2px 4px -1px rgba(0,0,0,.06)
  &:hover
    opacity 0.8
</style>
