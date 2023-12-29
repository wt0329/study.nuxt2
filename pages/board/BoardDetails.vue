<template>
    <v-container class="d-flex align-center justify-center">
        <v-card style="width: 50%" height="500" >
            <v-card-title>상세보기</v-card-title>

        <v-sheet>

            <table v-if="board" class="table">
                <tr>
                    <th>ID</th>
                    <td>{{board.boardId}}</td>

                    <th>구분</th>
                    <td>{{board.boardCd}}</td>
                </tr>
                <tr>
                    <th>제목</th>
                    <td colspan="3">{{board.boardTitle}}</td>
                </tr>
                <tr>
                    <th>내용</th>
                    <td colspan="3">{{board.boardCnt}}</td>
                </tr>
                 <tr v-if="board.fileOgnm !== null">
                        <th>첨부파일</th>
                        <td colspan="3">
                            <a :href="`/api/v1/board/${board.fileId}/download`" download>
                                {{ board.fileOgnm }}
                            </a>
                        </td>
                    </tr>
                    <tr v-else>
                        <th>첨부파일</th>
                        <td colspan="3" >등록된 첨부파일이 없습니다.</td>
                    </tr>
            </table>
          <v-col cols="12" md="5">
                <v-btn class="mt-2 mr-2" color="primary" @click="updateBoard">수정</v-btn>
                <v-btn class="mt-2 mr-2" color="primary" @click="deleteBoard">삭제</v-btn>
                <v-btn class="mt-2 mr-2" color="primary" @click="BoardList">목록</v-btn>
        </v-col>
        </v-sheet>
        </v-card>
    </v-container>

</template>

<script>
export default {
    props: ['boardId'],
    data() {
        return {
            board: []
        };
    },

    mounted() {
        this.$axios.get(`/api/v1/board/${this.boardId}`)
        .then((resp) => {
            this.board = resp.data;
            window.console.log(this.board);
        }).catch(() => {
            window.console.error(arguments);
        });
    },

    methods: {
        async BoardList() {
            await this.$router.push('/board/BoardList')
        },
       deleteBoard() {
            if (confirm("삭제하시겠습니까?")) {
                try {
                    this.$axios.delete(`/api/v1/board/${this.boardId}`);
                    this.$router.push('/board/BoardList');
                } catch (error) {
                    window.console.error("삭제 실패:", error);
                }
            }
        },
        updateBoard() {
            this.$router.push(`/board/BoardUpdate/${this.board.boardId}`);
        }
    }
};
</script>
