<script setup>
// const sayHello = () => {
//   console.log('Hi Index');
// };
// sayHello();

// Composables

const { sayHello } = utils();
sayHello();

const { $sayHello } = useNuxtApp();
$sayHello('Kanna');
</script>

<template>
  <div>
    <h1>
      Index Layout
      <img
        src="/image.png"
        alt="image"
      />
    </h1>
  </div>
</template>
