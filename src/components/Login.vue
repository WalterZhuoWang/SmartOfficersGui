<template>
  <div id="Login">

    <div class="content">
      <Form ref="user" :model="user" :rules="userValidate">
        <FormItem prop="username">
          <Input type="text" v-model="user.username" placeholder="Username" style="width: 200px">
            <Icon type="ios-person-outline" slot="prepend"></Icon>
          </Input>
        </FormItem>
        <FormItem prop="password">
          <Input type="password" v-model="user.password" placeholder="Password" style="width: 200px">
          <Icon type="ios-lock-outline" slot="prepend"></Icon>
          </Input>
        </FormItem>
        <FormItem style="text-align: left;">
          <Button type="primary" @click="toNextPage('user')">Login</Button>
          <Button @click="userRegisterFlag=true" >Register</Button>
        </FormItem>
      </Form>
    </div>
    <Modal v-model="userRegisterFlag" title="User Register" @on-ok="Register('registerUser')" :loading="loading">
      <Form ref="registerUser" :model="registerUser" :rules="registerUserValidate" :label-width="100">
        <FormItem label="username" prop="username">
          <Input v-model="registerUser.username" placeholder="username"></Input>
        </FormItem>
        <FormItem label="password" prop="password">
          <Input v-model="registerUser.password" placeholder="password"></Input>
        </FormItem>
        <FormItem label="confirm password" prop="passwordCheck">
          <Input v-model="registerUser.passwordCheck" placeholder="confirm password"></Input>
        </FormItem>
      </Form>
    </Modal>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data: function () {
    const validatePasswordCheck = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('please enter the password again'))
      } else if (value !== this.registerUser.password) {
        callback(new Error('The two passwords do not match'))
      } else {
        callback()
      }
    }
    return {
      user: {
        username: '',
        password: ''
      },
      userValidate: {
        username: [{
          required: true,
          message: 'The username cannot be empty',
          trigger: 'blur'
        }],
        password: [{
          required: true,
          message: 'The password cannot be empty',
          trigger: 'blur'
        }]
      },
      userRegisterFlag: false,
      loading: true,
      registerUser: {
        username: '',
        password: '',
        passwordCheck: ''
      },
      registerUserValidate: {
        username: [{
          required: true,
          message: 'The username cannot be empty',
          trigger: 'blur'
        }],
        password: [{
          required: true,
          message: 'The password cannot be empty',
          trigger: 'blur'
        }],
        passwordCheck: [{
          required: true,
          message: 'please input the password again',
          trigger: 'blur'
        }, {
          validator: validatePasswordCheck,
          trigger: 'blur'
        }]
      }
    }
  },
  methods: {
    toNextPage (user) {
      this.$refs[user].validate((valid) => {
        if (valid) {
          console.log(this.user.username + ' ' + this.user.password)
          this.$router.replace('/')
          this.$Message.success('Success!')
        } else {
          this.$Message.error('The username or password is not correct!')
        }
      })
    },
    Register (registerUser) {
      console.log(this.registerUser.username + ' ' + this.registerUser.password)
      this.$refs[registerUser].validate((valid) => {
        if (valid) {
          console.log(this.registerUser.username + ' ' + this.registerUser.password)
          this.$Message.success('Success!')
          this.userRegisterFlag = false
        } else {
          this.$Message.error('The username or password is not correct!')
          setTimeout(() => {
            this.loading = false
            this.$nextTick(() => {
              this.loading = true
            })
          }, 500)
        }
      })
    }
  }
}
</script>

<style>
  .img-box img {
    width: 100%;
  }

  .content {
    margin: 25px auto;
    border: 0px solid #000;
    background-repeat: no-repeat;
    background-position: left top;
    width: 1000px;
    height: 400px;
    padding-top: 20px;
    text-align: center;
  }
</style>
