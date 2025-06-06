<script lang="ts" setup>
interface Post {
  id: string
  slug: string
  body: string
  data: Record<string, any>
  collection: string
  render: any
}

defineProps<{
  list: Post[]
}>()

function getDate(date: string) {
  return new Date(date).toISOString()
}

function getHref(post: Post) {
  if (post.data.redirect)
    return post.data.redirect
  return `/posts/${post.slug}`
}

function getTarget(post: Post) {
  if (post.data.redirect)
    return '_blank'
  return '_self'
}

function isSameYear(a: Date | string | number, b: Date | string | number) {
  return a && b && getYear(a) === getYear(b)
}

function getYear(date: Date | string | number) {
  return new Date(date).getFullYear()
}
</script>

<template>
  <ul sm:min-h-38 min-h-28 mb-18>
    <template v-if="!list || list.length === 0">
      <div my-12 opacity-50>
        nothing here yet.
      </div>
    </template>
    <li v-for="(post, index) in list" :key="post.data.title" mb-8>
      <div v-if="!isSameYear(post.data.date, list[index - 1]?.data.date)" select-none relative h18 pointer-events-none>
        <span text-7em color-transparent font-bold text-stroke-2 text-stroke-hex-aaa op-30 absolute top--0.2em>
          {{ getYear(post.data.date) }}
        </span>
      </div>
      <div flex="~ text-wrap gap-2 justify-between w-full">
        <a text-lg lh-tight prose-link :aria-label="post.data.title" :target="getTarget(post)" :href="getHref(post)" block text-wrap break-words>
          <span lh-normal>
            <i v-if="post.data.draft" text-base vertical-mid i-ri-draft-line />
            {{ post.data.title }}
          </span>
        </a>
        <div text-sm ws-nowrap flex="~ gap-2 items-center">
          <i v-if="post.data.redirect" text-base i-ri-external-link-line />
          <i v-if="post.data.recording || post.data.video" text-base i-ri:film-line />
          <time v-if="post.data.date" :datetime="getDate(post.data.date)">{{ post.data.date.split(',')[0] }}</time>
          <span v-if="post.data.duration">· {{ post.data.duration }}</span>
        </div>
      </div>
    </li>
  </ul>
</template>
