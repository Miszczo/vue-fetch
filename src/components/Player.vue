<script setup lang="ts">

const props = defineProps({
  playerData: {
    type: Object,
    required: true
  }
})


const { first_name, last_name } = props.playerData
const { full_name: teamName } = props.playerData.team
</script>

<template>
  <div class="player">
    <h3 class="player-name">
      {{ first_name }} {{ last_name }}
    </h3>
    <h5 class="team-name">{{ teamName }}</h5>
  </div>
</template>

<style scoped lang="scss">
.player {
  margin-bottom: 15px;
  background-color: var(--main-orange);
  border: 1px solid black;
  padding: 10px 20px;
  border-radius: 8px;


  .player-name {
    margin-bottom: 5px;
  }

  .team-name {}
}
</style>
