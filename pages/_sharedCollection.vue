<template lang="pug">
  div
    div(v-for="addon in addons" :key="addon.id") {{ addon.websiteUrl }}
</template>

<script>
import encode from 'object-encode'

export default {
  async asyncData({ app, params }) {
    const salt = 123

    const decodedObject = await encode.decode_object(
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
