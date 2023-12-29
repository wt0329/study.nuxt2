<template>
    <v-container>
        <v-sheet elevation="1">
            <v-row>
                <v-col cols="12" md="5">
                    <v-text-field v-model="srchData.boardCd"  label="구분" density="compact" variant="outlined" ></v-text-field>
                </v-col>
               <v-col cols="12" md="5">
                    <v-text-field v-model="srchData.keyword"  label="키워드" density="compact" variant="outlined"></v-text-field>
                </v-col>
                <v-col cols="12" md="5">
                    <v-btn class="mt-2 mr-2" color="primary" @click="search">검색</v-btn>
                    <v-btn class="mt-2" color="primary" @click="write">등록</v-btn>
                </v-col>
            </v-row>
        </v-sheet>
        <v-sheet>
            <table>
                <thead>
                    <tr>
                        <th>#</th>
                        <th>ID</th>
                        <th>구분</th>
                        <th>제목</th>
                        <th>작성자</th>
                    </tr>
                </thead>
                <tbody>
                    <template v-if ="boards!=null && boards.length > 1">
                        <tr v-for="(board, idx) in boards" :key="board.boardId">
                            <td>{{ idx+1 }}</td>
                            <td>{{ board.boardId }}</td>
                            <td>{{ board.boardCd }}</td>
                            <!-- <td><router-link :to="{path: `/boardDetails/${board.boardId}` }">{{ board.boardTitle }}</router-link></td> -->
                            <td><nuxt-link :to="{path: `/board/BoardDetails/${board.boardId}` }">{{ board.boardTitle }}</nuxt-link></td>
                            <td>{{ board.userNm }}</td>
                        </tr>
                    </template>
                    <template v-else>
                        <tr>
                            <td colspan="4" class="text-center">등록된 게시글이 없습니다.</td>
                        </tr>
                    </template>
                </tbody>
            </table>
        </v-sheet>
    </v-container>
</template>

<script>
    export default {
        data() {
            return {
                boards: [],
                srchData : {
                   boardCd:'',
                   keyword: ''
                },

            }
        },
        mounted() {
            this.$axios.get('/api/v1/board')
            .then((resp) => {
                this.boards = resp.data;
                window.console.log(this.boards);
            }).catch(() => {
                window.console.error(arguments);
            })
        },

        methods: {
            async search() {

                try{
                    const resp = await this.$axios.get('/api/v1/board', {
                        params : this.srchData
                    });
                    this.boards = resp.data
                }catch(e) {
                   // console.log(e);
                }
            },
            write() {
                this.$router.push('/board/BoardInsert')
            }
        }
    }
</script>
