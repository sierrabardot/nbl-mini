<template>
  <div>
    <h3>{{ team.teamName }}</h3>
    <ul>
      <!-- error: 'Cannot read properties of undefined (reading 'suburb')' <li v-if="teamDetails.suburb"> -->
      <li v-if="teamDetails && teamDetails.suburb">
        Location: {{ teamDetails.suburb }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'TeamCard',
  props: {
    team: Object,
    teamDetails: Object
  }
};
</script>
