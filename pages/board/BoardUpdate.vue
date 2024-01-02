<template>

    <v-container class="d-flex align-center justify-center">
        <v-card style="width: 50%" height="500" >
            <v-card-title>수정</v-card-title>

        <v-sheet>
            <v-form @submit.prevent="submitForm">
            <table class="table">
                <tr>
                    <th>ID</th>
                    <td>{{boardId}}</td>

                   <th>구분</th>
                    <td><input v-model="board.boardCd" placeholder="구분" /></td>
                </tr>
                <tr>
                    <th>제목</th>
                    <td colspan="3"><input v-model="board.boardTitle" placeholder="제목" /></td>
                </tr>
                <tr>
                    <th>내용</th>
                    <td colspan="3"><input v-model="board.boardCnt" placeholder="내용" /></td>
                </tr>
            </table>
          <v-col cols="12" md="5">
                <v-btn class="mt-2 mr-2" color="primary" type="submit">저장</v-btn>
                <v-btn class="mt-2 mr-2" color="primary" @click="BoardList">목록</v-btn>
        </v-col>
        </v-form>
        </v-sheet>
        </v-card>
    </v-container>

</template>

<script>
export default {
    props: ['boardId'],
    data() {
        return {
            board: [],

        };
    },
    mounted() {
        this.$axios.get(`/api/v1/board/${this.boardId}`)
        .then((resp) => {
            this.board = resp.data[0];
            window.console.log(this.board);
        }).catch(() => {
            window.console.error(arguments);
        });
    },

    methods: {
        async BoardList() {
            await this.$router.push('/board/BoardList')
        } ,
        async submitForm() {
            const formData = new FormData();

    formData.append('boardCd', this.board.boardCd);
    formData.append('boardTitle', this.board.boardTitle);
    formData.append('boardCnt', this.board.boardCnt);
    try {

      await this.$axios.put(`/api/v1/board/${this.boardId}`, formData);

      this.$router.push('/board/BoardList');
    } catch (error) {
      window.console.error(error);
    }
  }
}
};
</script>
