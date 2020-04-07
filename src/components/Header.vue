<template>
  <div>
    <b-nav>
      <b-nav-item tabs disabled>
        <b>Vue Quiz App</b>
      </b-nav-item>
      <b-nav-item disabled>
        Counter
        <span class="fade-in">
          <b>{{correctCount}}/{{totalCount}}</b>
        </span>
      </b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: {
    correctCount: Number,
    totalCount: Number
  }
};
</script>

<style scoped>
.fade-in {
  animation: fadeIn ease 5s;
  -webkit-animation: fadeIn ease 5s;
  -moz-animation: fadeIn ease 5s;
  -o-animation: fadeIn ease 5s;
  -ms-animation: fadeIn ease 5s;
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@-moz-keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@-webkit-keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@-o-keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@-ms-keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>