<script setup>
import { ref } from 'vue'
import { account } from '~/composables/account';
const { checkSession, table } = account();
const route = useRoute();
const isLoading = ref(false)
const list = ref([])
const created = async () => {
  if(!account().checkSession()){
    account().next('/')
    return
  }
  let teamList = await account().table.team.getList()
  if (teamList.length == 1) {
    pushTeam(0)
  } else {
    list.value = teamList
    isLoading.value = true
  }
}

/**
 * チーム選択y
 * @param {*} idx 
 */
const pushTeam = (idx) => {
  account().table.team.set(idx)
  account().next('mainBoard');
}
await created()
</script>
<template>
  <v-container class="pa-0 ma-0" v-if="isLoading">
    <v-row>
      <v-col cols=12 class="pa-0 ma-0">
        <v-container class="pa-0 ma-0">
          <v-row>
            <v-col class="pa-0 ma-0" cols=12>
              <h1 class="font-impact">SelectTeam</h1>
            </v-col>
            <v-col class="pa-0 ma-0" cols=12>
              <v-container>
                <v-row>
                  <v-col v-for="(item, idx) in list" cols="3">
                    <v-card class="pa-0 ma-0 act-card" @click="pushTeam(idx)">
                      <v-card-title>
                        <h4>{{ item.name }}</h4>
                      </v-card-title>
                      <v-card-text>
                        <v-content>
                          <v-row>
                            <v-col cols="5" class="pa-0 my-0">
                              <span class="bold">タスクAppID：</span>{{ item.taskAppId }}
                            </v-col>
                            <v-col cols="5" class="pa-0 my-0">
                              <span class="bold">スプリントAppId：</span>{{ item.sprintAppId }}
                            </v-col>
                          </v-row>
                        </v-content>
                      </v-card-text>
                    </v-card>
                  </v-col>
                </v-row>
              </v-container>
            </v-col>
          </v-row>
        </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>
<style scoped></style>