<template>
  <div @click="$emit('click')" class="block">
    <span v-if="value !== ''" :class="{ 'x' : value === 'x', 'o': value === 'o' }">{{ value }}</span>
  </div>
</template>

<script>
export default {
  props: ["value"]
};
</script>
<style>
.block {
  width: 84px;
  height: 84px;
  border: 2px solid rgb(233, 233, 233);
  font-size: 62px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}
.x {
  color: brown;
  position: relative;
  top: -6px;
  font-weight: bold;
  font-size: 48px;
}
.o {
  color: blue;
  position: relative;
  top: -6px;
  font-weight: bold;
  font-size: 48px;
}
</style>
