<template>
  <section class="container">
    <h1>Teams</h1>
    <div v-if="loading">Loading...</div>
    <div v-if="error">Error loading teams</div>
    <div v-if="teams.length">
      <TeamCard
        v-for="team in teams"
        :key="team.teamId"
        :team="team"
        :teamDetails="teamDetails[team.teamId]"
      />
    </div>
  </section>
</template>

<script>
import TeamCard from '../../components/TeamCard'
const BASE_URL = 'https://test.services.nbl.com.au/api_cache/nbl/genius'

export default {
  components: {
    TeamCard
  },
  data() {
    return {
      teams: [],
      teamDetails: {},
      loading: false,
      error: null
    }
  },
  async asyncData({ error }) {
    try {
      const res = await fetch(`${BASE_URL}?route=competitions/35847/teams&fields=teamName,teamId`)
      if (!res.ok) {
        throw new Error('Error fetching teams')
      }
      const data = await res.json()
      const teamDetails = {}
      await Promise.all(
        data.map(async team => {
          const teamRes = await fetch(`${BASE_URL}?route=teams&filter[teamId]=${team.teamId}`)
          if (!teamRes.ok) {
            throw new Error('Error fetching team details')
          }
          const details = await teamRes.json()
          teamDetails[team.teamId] = details[0]
        })
      )
      return { teams: data, teamDetails, loading: false, error: null }
    } catch (err) {
      error({ statusCode: 500, message: err.message })
      return { teams: [], teamDetails: {}, loading: false, error: err.message }
    }
  },
  head() {
    return {
      title: 'TeamCard',
      meta: [
        { name: 'teams', content: 'This is a list of the teams in the NBL.' },
        { name: 'keywords', content: 'NBL, NBL info, national basketball league' }
      ]
    }
  }
}
</script>