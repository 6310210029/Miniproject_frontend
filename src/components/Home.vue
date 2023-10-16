<template>
  <div>
    <header>
      
      <h1 class="header-title">คำนวณแคลอรี่ในอาหาร</h1>
      <img src="public/food.jpg" alt="จำนวนแคลอรี่ที่ควรได้รับต่อวันเป็นค่าประมาณ(US RDI) เพื่อคำนวณค่าที่แม่นยำยิ่งขึ้นของตัวคุณเอง ">
    </header>
    <nav>
  <ul>
    <li @click="filterMenu('all')">ทั้งหมด</li>
    <li @click="filterMenu('แป้ง')">แป้ง</li>
    <li @click="filterMenu('โปรตีน/ถั่ว')">โปรตีน/ถั่ว</li>
    <li @click="filterMenu('ผัก/ผลไม้')">ผัก/ผลไม้</li>
    <li @click="filterMenu('อาหาร')">อาหาร</li>
    <li @click="filterMenu('เครื่องดื่ม<')">เครื่องดื่ม</li>
    <!-- เพิ่มประเภทอาหารอื่น ๆ ที่คุณต้องการ -->
  </ul>
</nav>

 
    <main>
      <h2 class="menu-title">เมนูอาหาร</h2>
      <table class="menu-table">
        <thead>
          <tr>
            <th>ไอดี</th>
            <th>ชื่อเมนู</th>
            <th>จำนวน</th>
            <th>ประเภทอาหาร</th>
            <th>จำนวนแคลอรี่ในอาหาร</th>
            <th>เพิ่มลงรถเข็น</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in filteredMenu" :key="index">
          <td>{{ item.id }}</td>
          <td>{{ item.name }}</td>
          <td>
            <button @click="decreaseQuantity(item)">-</button>
            {{ item.quantity }}
            <button @click="increaseQuantity(item)">+</button>
          </td>
          <td>{{ item.type }}</td>
          <td>{{ item.calories }}</td>
          <td>
            <button @click="addToCart(item, item.quantity)">เพิ่มลงรถเข็น</button>
          </td>
        </tr>



</tbody>

      </table>
    </main>
    <aside>
      <h3>รถเข็น</h3>
      <ul class="cart-list">
        <li v-for="(item, index) in cart" :key="index" class="cart-item">
          <div class="item-details">
            <span class="item-name">{{ item.name }}</span> - {{ item.quantity }} จาน - {{ item.calories * item.quantity }} แคลอรี่ 
          </div>
          <button class="delete-button" @click="removeFromCart(index)">ลบ</button>
        </li>
      </ul>
      <h4>รวมแคลอรี่: {{ totalCaloriesInCart }} กิโลแคลอรี่</h4>
    </aside>
  </div>
</template>

<style>
/* กำหนดสีพื้นหลังให้กับ <main> เป็นสีเทา */
main {
  background-color: #b2dfdb;
  padding: 20px;
  
 
}

/* กำหนดสีตัวอักษรให้กับ <main> เป็นสีดำ */
main {
  color: #000;
  
}

header {
  background-color: #00897b;
  color: #fff;
  text-align: center;
  padding: 20px;
  align-items: center;
  width: 100%; /* ทำให้ header ยาวเต็มความกว้างขอบจอ */
}

nav {
  background-color: #004d40;
  color: #fff;
  padding: 20px;
  align-items: center;
}

nav ul {
  list-style: none;
  padding: 0;
  display: flex;
  justify-content: center;
}

nav li {
  margin: 0 20px;
}

aside {
  background-color: #ffffff;
  padding: 20px;
}
/* กำหนดสีพื้นหลังของตารางเป็นเทา */
.menu-table {
  background-color: #ffffff;
}
/* กำหนดสีของปุ่มให้เป็นสีฟ้า */
button {
  background-color: #00695c;
  color: #fff;
  border: none;
  margin: 0; 
  padding: 5px 10px;
  cursor: pointer;
  box-shadow: 0 3px 6px rgba(81, 126, 82, 0.688); /* เพิ่มเงาให้ปุ่ม */
  transition: background-color 0.3s; /* เพิ่มการเปลี่ยนสีเมื่อเมาส์ไฮไลต์ */
  
}


button:hover {
  background-color: #478a5dd0;
  align-items: center;
}

.menu-table {
  width: 100%;
  border-collapse: collapse;
  align-items: center;
}

.menu-table th,
.menu-table td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: center;
}

.delete-button {
  width: 50px;
  text-align: center;
  background-color: #254f40eb; /* เพิ่มสีพื้นหลังแบบเส้นแดงสำหรับปุ่มลบ */
  color: #fff;
}
.cart-list {
  list-style: none;
  padding: 0;
}
.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.item-details {
  flex: 1;
}

.menu-image {
  max-width: 120px;
  max-height: 120px;
}
</style>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      menu: [
        { id: 52, name: 'ผัดไทย', type:'แป้ง', price: '1 จาน', calories: 300, quantity: 0 },
        { id: 102, name: 'ข้าวผัด', type:'แป้ง', price: '1 จาน', calories: 250, quantity: 0 },
        { id: 53, name: 'ทุเรียน', type:'ผัก/ผลไม้', price: '1 จาน', calories: 200, quantity: 0 },
        // เพิ่มรายการอาหารอื่น ๆ ที่คุณต้องการ
      ],
      filteredMenu: [], // เพิ่มตรงนี้
      cart: [],
      totalCaloriesInCart: 0,
    };
  },
  created() {
    this.filterMenu('all');
  },
  
  methods: {
    async initialize() {
      this.foodItem = [];
      try {
        const response = await axios.get('http://localhost:9000/food');
        this.foodItem = response.data;
        console.log('data food ====>', this.foodItem);
      } catch (error) {
        console.error('เกิดข้อผิดพลาดในการดึงข้อมูล: ', error);
      }
    },
    filterMenu(type) {
    if (type === 'all') {
      this.filteredMenu = this.menu;
    } else {
      this.filteredMenu = this.menu.filter(item => item.type === type);
    }
  },
    addToCart(item, quantity) {
    const index = this.cart.findIndex((cartItem) => cartItem.name === item.name);
    if (index !== -1) {
      this.cart[index].quantity += quantity;
    } else {
      item.quantity = quantity;
      this.cart.push({
        name: item.name,
        price: item.price,
        calories: item.calories,
        image: item.image,
        quantity: quantity,
      });
    }
    this.totalCaloriesInCart += item.calories * quantity;
  },
  increaseQuantity(item) {
    item.quantity++;
    this.totalCalories += item.calories;
  },
  decreaseQuantity(item) {
    if (item.quantity > 0) {
      item.quantity--;
      this.totalCalories -= item.calories;
    }
  },
  removeFromCart(index) {
    const removedItem = this.cart.splice(index, 1)[0];
    this.totalCalories -= removedItem.calories * removedItem.quantity;
  },
}



};

</script>
