<script setup lang="ts">
const { apiImgUrl } = useRuntimeConfig().public;

const { slideNext, slidePrev } = useSwiperHelpers();
const { formatRuntime, formatDate } = useUtils();

const { data: popularMovieAndSerie } = await useFetch(
  '/api/popular-movie-and-serie'
);

const swiperRef = ref();
</script>

<template>
  <section class="relative">
    <Swiper
      class="h-auto aspect-[9/8] md:aspect-[10/8] lg:aspect-[10/7] xl:aspect-[10/5] 2xl:aspect-[10/4]"
      ref="swiperRef"
      :modules="[SwiperPagination]"
      :pagination="{
        bulletClass: 'swiper-pagination-bullet',
        type: 'bullets'
      }"
    >
      <SwiperSlide v-for="media in popularMovieAndSerie" :key="media.id">
        <NuxtImg
          class="lg:w-[70%] h-[70%] lg:h-full object-cover object-left w-full lg:ml-auto"
          :src="`${apiImgUrl}/w1920_and_h800_multi_faces${media.backdrop_path}`"
          :format="'webp'"
          :quality="80"
        />
        <div
          class="absolute bottom-0 lg:top-0 left-0 lg:w-[70%] w-full lg:h-full flex flex-col lg:gap-y-4 lg:justify-center justify-end pb-14 lg:pb-0 px-4 lg:px-20 h-[70%] bg-gradient-to-t lg:bg-gradient-to-r from-just-black via-just-black to-transparent"
        >
          <h1
            class="text-2xl lg:text-5xl max-w-full min-h-fit overflow-hidden lg:overflow-visible whitespace-nowrap text-ellipsis"
          >
            {{ media.title || media.name }}
          </h1>
          <div
            class="flex flex-col lg:flex-row text-sm md:text-base 2xl:text-lg gap-y-2 lg:items-center text-gray"
          >
            <Rating :vote-average="media.vote_average" />
            <div class="hidden lg:block">
              <Icon name="ph:dot" />
            </div>
            <div class="flex lg:gap-x-0 items-center gap-y-1 flex-wrap">
              <span>{{ media.vote_count }} Reseñas</span>
              <div>
                <Icon name="ph:dot" />
              </div>
              <span>{{
                formatDate(media.release_date)?.split('/')[2] ||
                formatDate(media.first_air_date)?.split('/')[2]
              }}</span>
              <div>
                <Icon name="ph:dot" />
              </div>
              <span>{{
                formatRuntime(media.runtime) ||
                media.seasons?.length + ' Temporadas'
              }}</span>
            </div>
          </div>
          <p class="text-silver hidden lg:block 2xl:text-lg">{{ media.overview }}</p>
        </div>
      </SwiperSlide>
      <SwipersButtonPrev
        class="hidden lg:block"
        @slide-prev="slidePrev(swiperRef)"
      />
      <SwipersButtonNext
        class="hidden lg:block"
        @slide-next="slideNext(swiperRef)"
      />
    </Swiper>
  </section>
</template>

<style>
.checked {
  color: orange;
}
.swiper-pagination-bullet {
  background-color: #fff;
}
</style>
