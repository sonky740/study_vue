<template>
  <AppLoading v-if="loading" />
  <AppError v-else-if="error" :message="error.message" />
  <div v-else>
    <AppError v-if="removeError" :message="removeError.message" />
    <h2>{{ post.title }}</h2>
    <p>id: {{ props.id }}, isOdd: {{ isOdd }}</p>
    <p>{{ post.content }}</p>
    <p class="text-muted">{{ post.createdAt }}</p>
    <hr class="my-4" />
    <div class="row">
      <div class="col-auto">
        <button
          class="btn btn-outline-dark"
          @click="$router.push(`/posts/${+props.id - 1}`)"
        >
          이전글
        </button>
      </div>
      <div class="col-auto">
        <button
          class="btn btn-outline-dark"
          @click="$router.push(`/posts/${+props.id + 1}`)"
        >
          다음글
        </button>
      </div>
      <div class="col-auto">
        <button class="btn btn-outline-dark" @click="goListPage">목록</button>
      </div>
      <div class="col-auto">
        <button class="btn btn-outline-primary" @click="goEditPage">
          수정
        </button>
      </div>
      <div class="col-auto">
        <button
          class="btn btn-outline-danger"
          @click="remove"
          :disabled="removeLoading"
        >
          <template v-if="removeLoading">
            <span
              class="spinner-grow spinner-grow-sm"
              role="status"
              aria-hidden="true"
            ></span>
            <span class="visually-hidden">Loading...</span>
          </template>
          <template v-else>삭제</template>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, toRef } from 'vue';
import { onBeforeRouteLeave, onBeforeRouteUpdate, useRouter } from 'vue-router';
import { useAxios } from '@/hooks/useAxios';
import { useAlert } from '@/hooks/useAlert';
import { useNumber } from '@/hooks/useNumber';

const props = defineProps({
  id: [String, Number],
});

const router = useRouter();
const idRef = toRef(props, 'id');
// const { id: idRef } = toRefs(props);
const { isOdd } = useNumber(idRef);
const url = computed(() => `/posts/${props.id}`);
const { error, loading, data: post } = useAxios(url);

const {
  error: removeError,
  loading: removeLoading,
  execute,
} = useAxios(
  `/posts/${props.id}`,
  { method: 'DELETE' },
  {
    immediate: false,
    onSuccess: () => {
      router.push({ name: 'PostList' });
      vSuccess('삭제가 완료되었습니다.');
    },
    onError: err => {
      vAlert(err.message);
    },
  },
);

const remove = async () => {
  if (confirm('삭제 하시겠습니까?') === false) return;
  execute();
};

const goListPage = () => {
  router.push({
    name: 'PostList',
  });
};

const goEditPage = () => {
  router.push({
    name: 'PostEdit',
    params: {
      id: props.id,
    },
  });
};

// alert
const { vAlert, vSuccess } = useAlert();

// router
onBeforeRouteUpdate(() => {
  console.log('onBeforeRouteUpdate');
});

onBeforeRouteLeave(() => {
  console.log('onBeforeRouteLeave');
});
</script>

<script>
export default {
  beforeRouteEnter() {
    console.log('beforeRouteEnter');
  },
};
</script>

<style lang="scss" scoped></style>
