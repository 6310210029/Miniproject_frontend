<template>
  <v-app>
    <v-app-bar app color="#b2dfdc"> <!-- ตั้งสีที่คุณต้องการ -->
      <!-- เพิ่มปุ่มในแถบบาร์ด้านบน -->
      <v-btn text color="white" @click="goToHome" class="mr-2">หน้าแรก</v-btn>
      <v-btn text color="white" @click="goToManage" class="mr-2">หน้าจัดการข้อมูล</v-btn>
      <v-btn text color="white" @click="goToLogin" class="mr-2">เข้าสู่ระบบ</v-btn>
      <v-btn text color="white" @click="goToLogin" class="mr-2" >ออกจากระบบ</v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  data: () => ({}),
  methods: {
    goToLogin() {
      this.$router.push('/');
    },
    goToManage() {
      this.$router.push('/ManageTable');
    },
    goToHome() {
      this.$router.push('/home');
    },
  },
};
</script>

<style>
</style>
