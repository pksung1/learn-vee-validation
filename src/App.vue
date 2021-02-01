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
                <validation-provider
                  name="user-id"
                  rules="id-valid|required"
                  v-slot="{ errors, valid }"
                >
                  <v-text-field name="user-id" v-model="userId" label="ID"/>
                  <ul>
                    <li v-for="error in errors" :key="error">{{error}}</li>
                  </ul>
                  {{errors}}
                  {{valid}}
                </validation-provider>
                <validation-provider
                  name="passwd"
                  rules="pw-valid|required|password:@userpw2"
                  v-slot="{ errors }"
                >
                  <v-text-field v-model="userPw" label="비밀번호"/>
                  {{ errors[0] }}
                </validation-provider>
                <validation-provider name="userpw2">
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
import { ValidationProvider, ValidationObserver, extend } from 'vee-validate';
import { alpha_num, required } from 'vee-validate/dist/rules';
export default {
  name: 'App',
  components: {
    ValidationProvider, ValidationObserver
  },
  data () {
    return {
      kv: {
        'user-id': '아이디',
        'passwd': '비밀번호'
      },
      userId: '',
      userPw: '',
      userPw2: '',
      userPhone: '',
      userEmail: ''
    }
  },
  beforeMount () {
    extend('id-valid', {
      ...alpha_num,
      message: (name, schema) => {
        return `${this.kv[name]} 에서 [${schema._value_}]는 영문,숫자로 이루어져 있지 않습니다.`
      }
    }),
    extend ('pw-valid', {
      validate (value) {
        //eslint-disable-next-line
        const exp = new RegExp(/^([A-Za-z0-9_\!\@\#\$\%\^\&\*\(\)])\w+$/)
        return exp.test(value)
      },
      message: (name, schema) => {
        console.log(name, schema)
        return `${this.kv[name]} 에서 [${schema._value_}]는 올바르지 않는 문자가 들어있습니다.`
      }
    })
    extend ('required', {
      ...required,
      message: '반드시 존재해야 하는 값입니다.',
    })
    extend ('password', {
      params: ['target'],
      validate (value, {target}) {
        return value === target
      },
      message: '비밀번호가 일치하지 않습니다.'
    })
  }
};
</script>