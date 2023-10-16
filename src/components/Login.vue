<template>
    <v-form 
      ref="form"
      v-model="valid"
      lazy-validation
    >
    <br>
    <br>
    <div>
    <h1 align-center>  WELCOME </h1>
  </div>
    <br>
    <br>
      <v-text-field
        v-model="name"
        :counter="10"
        :rules="nameRules"
        label="Username:"
        required
      ></v-text-field>
  
      <v-text-field
        v-model="password"
        :rules="passwordRules"
        label="Password:"
        required
      ></v-text-field>
  
      <v-btn
        :disabled="!valid"
        color="Disabled"
        class="mr-4"
        @click="Login"
      >
        Login
      </v-btn>
    </v-form>
  </template>
  
  <script>
  export default {
    data: () => ({
      valid: true,
      name: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 10) || 'Name must be less than 10 characters'
      ],
      password: '',
      passwordRules: [
        v => !!v || 'Password is required',
      ],
      select: null,
      items: [
        'Item 1',
        'Item 2',
        'Item 3',
        'Item 4'
      ],
      checkbox: false
    }),
  
    methods: {
        Login(){
            if(this.$refs.form.validate()){
                alert('เข้าสู่ระบบ')
            }

        },
      validate () {
        this.$refs.form.validate()
      },
      reset () {
        this.$refs.form.reset()
      },
      resetValidation () {
        this.$refs.form.resetValidation()
      }
    }
  }
  </script>
  
  <style>
  
  </style>