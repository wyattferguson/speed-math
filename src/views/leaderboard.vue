<template>
  <div id="leaderboard" class="container">
    <sidebar page="leaderboard"></sidebar>
    <div class="center">
      <h1>HIGHSCORES</h1>
      <h4>Top 50 times for {{ cat }} questions</h4>
      <ul>
        <li>Categories</li>
        <li v-for="i in (maxQuestions/minStep)" :key="i"><router-link :to="{name:'leaderboard', params:{cat:i*minStep}}" class="btn">{{ i*minStep }}</router-link></li>
      </ul>
      <table>
        <tr>
          <th class="rank">Rank</th>
          <th class="name">Name</th>
          <th class="time">Time</th>
          <th class="date">Date</th>
        </tr>
        <tr v-for="(user, rank) in users" :key="rank">
				  <td class="rank">{{ rank + 1}}</td>
          <td class="name">{{ user['name']}}</td>
          <td class="time">{{ user['time']}}</td>
          <td class="date">{{ date_added(user['date_added']) }}</td>
			  </tr>
      </table>
    </div>
  </div>
</template>

<script>
import sidebar from '../components/sidebar'

export default {
  name: 'leaderboard',
  components: {
    sidebar
  },
  props: {
    cat: Number
  },
  data(){
    return{
      users: null,
      maxQuestions: this.$maxQuestions,
      minQuestions: this.$minQuestions,
      minStep: this.$minStep,
    }
  },
  methods: {
    getCategory(){
      this.$http.get("?c="+this.cat,{crossdomain: true})
        .then(response => {
          this.users = response.data
      });
    },
    
    // Clean up date format
    date_added(user_date) {
      return user_date.split(' ')[0];
    }
  },
  mounted() {
    this.getCategory();
  },
  watch: {
    $route(to, from) {
      this.getCategory();
    }
  }
}
</script>
