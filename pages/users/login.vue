<template>
  <v-container>
     <v-card style="width: 50%" height="500" >
            <v-card-title>로그인</v-card-title>
      <v-form @submit.prevent="loginForm">
        <tbody>
          <tr>
            <th>아이디</th>
            <td>
              <v-text-field v-model="user.userId" placeholder="아이디"/>
            </td>
          </tr>
          <tr>
            <th>비밀번호</th>
            <td>
              <v-text-field v-model="user.userPw" placeholder="비밀번호"/>
            </td>
          </tr>
        </tbody>
      <v-row>
         <v-col cols="12" md="5">
          <v-btn class="mt-2 mr-2" type="button" color="primary" @click="loginForm" >로그인</v-btn>
        </v-col>
      </v-row>
      </v-form>
     </v-card>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      user: {
        userId: "",
        userPw: ""
      },
    };
  },

 methods: {
  async loginForm() {
    const formData = new FormData();
    formData.append('userId', this.user.userId);
    formData.append('userPw', this.user.userPw);

    try {
      const resp = await this.$axios.post('/api/v1/login', formData);

      window.console.log('resp:', resp.data);
      this.$router.push('/board/BoardList');
    } catch (error) {

      window.console.error('Error:', error);
    }
  }
 }}
</script>
