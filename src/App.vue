<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <h3>vee-validator 테스트</h3>
      </div>
    </v-app-bar>

    <v-main>
      <v-form>
        <v-container>
          <v-row>
            <v-col>
              <validation-observer>
                <validation-provider>
                  <v-text-field v-model="userId" label="ID"/>
                </validation-provider>
                <validation-provider>
                  <v-text-field v-model="userPw" label="비밀번호"/>
                </validation-provider>
                <validation-provider>
                  <v-text-field v-model="userPw2" label="비밀번호 재입력"/>
                </validation-provider>
                <validation-provider>
                  <v-text-field v-model="userEmail" label="이메일"/>
                </validation-provider>
                <validation-provider>
                  <v-text-field v-model="userPhone" label="핸드폰번호"/>
                </validation-provider>
                <v-btn color="primary" align="left">완료</v-btn>
              </validation-observer>
            </v-col>
          </v-row>
        </v-container>

      </v-form>
    </v-main>
  </v-app>
</template>

<script>
import { ValidationProvider, ValidationObserver } from 'vee-validate';
export default {
  name: 'App',
  components: {
    ValidationProvider, ValidationObserver
  }
};
</script>