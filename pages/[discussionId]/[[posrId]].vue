<template>
  <div class="ts-container has-vertically-spaced-large">
    <div class="ts-grid is-relaxed">
      <div class="column is-3-wide mobile:ts-app-drawer">
        <div style="position: sticky; top: 1rem">
          <div class="ts-menu is-start-icon is-separated">
            <a href="/" class="ts-button">
              <span class="ts-icon is-less-than-icon"></span> 返回首頁
            </a>
          </div>
          <div class="ts-divider is-section"></div>
          <div class="ts-wrap is-middle-aligned">
            <div class="ts-text is-heavy">{{ data.name }}</div>
          </div>
          <div class="ts-divider is-section"></div>
          <div class="ts-wrap is-middle-aligned">
            <div class="ts-avatar is-circular">
              <img :src="ownerProfileAvatar" />
            </div>
            <div class="column">
              <div class="ts-text">{{ data.user.displayName }}</div>
              <div class="ts-text is-small" :title="data.createdAt">
                {{ $dayjs(data.createdAt).fromNow() }}
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="column is-9-wide mobile:is-16-wide" v-if="!pending">
        <discussion-post v-for="(j, i) in data.posts" :key="i" v-bind="j" :users="data.users" />
      </div>
      <div class="column is-9-wide mobile:is-16-wide" v-else>
      </div>
      <div class="column is-4-wide mobile:ts-app-drawer">
        <div style="position: sticky; top: 1rem">
          <div class="ts-menu is-start-icon is-separated">
            <div class="ts-divider is-section"></div>
            <a href="#!" class="item"> <span class="ts-icon is-discord-icon"></span> 進入 Discord 發佈文章 </a>
            <a href="#!" class="item"> <span class="ts-icon is-github-icon"></span> 加入 GitHub 協助我們 </a>
            <div class="ts-divider is-section"></div>
            <a href="#!" class="item"> <span class="ts-icon is-user-group-icon"></span> 關於 Deter 迪特 </a>
            <a href="#!" class="item"> <span class="ts-icon is-flag-icon"></span> 全站版規 </a>
            <a href="#!" class="item"> <span class="ts-icon is-bookmark-icon"></span> 服務條款 </a>
            <a href="#!" class="item"> <span class="ts-icon is-check-to-slot-icon"></span> 隱私權政策 </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import DragonLightIcon from "../assets/DragonLightIcon.png"

import discussionPost from "../../components/DiscussionPost.vue";

const { apiInvokeBaseUrl, apiPublicBaseUrl } = useRuntimeConfig();
const route = useRoute();

const { discussionId } = route.params;
const { data, pending, error, refresh } = await useAsyncData(
  'discussion-data',
  () => $fetch(`${apiInvokeBaseUrl}/discussions/${discussionId}`)
);

if (error.value) {
  console.error(error.value)
}

useHead({
  title: data.value.name,
})

definePageMeta({
  layout: 'clear'
})

const ownerProfileAvatar = computed(() => {
  const { id, avatarHash } = data.value.user;
  if (!avatarHash) {
    return DragonLightIcon;
  }
    return `${apiPublicBaseUrl}/assets/images/avatar-${id}`;
});
</script>
