<script setup lang="ts"></script>

<template>
  <div class="step">
    <img src="/images/icon-thank-you.svg" alt="thank-you image" />

    <h1>Thank you!</h1>

    <p>Thanks for confirming your subscription! We hope you have fun using our platform. If you ever need support, please feel free to email us at support@loremgaming.com.</p>
  </div>
</template>

<style scoped>
h1 {
  margin: 1.75rem 0 0.5rem 0;
  color: #03295a;
  font-size: 2rem;
  font-weight: bold;
}

p {
  margin-bottom: 3rem;
  color: hsl(231, 11%, 63%);
}

.step {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  height: 100%;
}

img {
  width: 5rem;
}

@media only screen and (max-width: 750px) {
  h1 {
    font-size: 1.5rem;
  }

  p {
    margin-bottom: 2rem;
  }

  img {
    margin-top: 3rem;
    width: 4rem;
  }
}
</style>
