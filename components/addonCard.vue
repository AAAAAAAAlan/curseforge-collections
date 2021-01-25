<template lang="pug">
  .addon-card.flex.justify-start.items-center.my-6.bg-white.p-3
    img.w-16(:src="addonImage")
    .info-section.ml-8
      h1.name {{ addon.name }}
      .links.flex.text-gray-700
        a.text-xs.mt-2.mr-8 {{ `Game: ${addon.gameName}` }}
        a.text-xs.mt-2.mr-8.underline(
          :href="addon.websiteUrl"
          target="_blank"
          rel="noopener noreferrer"
        ) Curseforge Link
        p.text-xs.mt-2.underline.text-red-500.cursor-pointer(
          v-if="!$route.params.sharedCollection"
          @click="$emit('removeAddon', index)"
        ) Remove from collection
      p.summary.text-xs.mt-2 {{ addon.summary }}
      

    
</template>

<script>
export default {
  name: 'AddonCard',
  props: {
    addon: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
  },
  computed: {
    addonImage() {
      return this.addon.attachments.length === 0
        ? this.addon.categories[0].avatarUrl
        : this.addon.attachments[0].thumbnailUrl
    },
  },
}
</script>

<style lang="stylus" scoped>
.addon-card
  box-shadow 0 4px 6px -1px rgba(0,0,0,.1), 0 2px 4px -1px rgba(0,0,0,.06)
</style>
