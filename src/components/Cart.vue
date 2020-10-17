<template>
  <div>
    <h2>{{ title }}</h2>

    <el-table
        ref="multipleTable"
        :data="cart"
        tooltip-effect="dark"
        style="width: 100%"
        border
        @selection-change="handleSelectionChange">
      <el-table-column
          type="selection"
          width="55">
      </el-table-column>

      <el-table-column
          prop="title"
          label="Course"
          width="120">
      </el-table-column>
      <el-table-column
          prop="price"
          label="Price"
          width="120">
      </el-table-column>

      <el-table-column
          label="Title"
          width="200">
        <template slot-scope="scope">
          <el-input-number v-model="scope.row.count" :min="1" :max="100"></el-input-number>
        </template>
      </el-table-column>

      <el-table-column
          label="Total"
          width="120">
        <template slot-scope="scope">
          {{scope.row.count * scope.row.price}}
        </template>
      </el-table-column>

    </el-table>


    <table border="1">
      <tr>
        <th>#</th>
        <th>Course</th>
        <th>Item price</th>
        <th>Quantity</th>
        <th>Total Price</th>
      </tr>
      <tr v-for='(c,index) in cart' :key='c.id'>
        <td>
          <input type="checkbox" v-model='c.active'/>
        </td>
        <td>{{ c.title }}</td>
        <td>{{ c.price }}</td>
        <td>
          <el-button @click="subtract(index)">-</el-button>
          {{ c.count }}
          <el-button @click="add(index)">+</el-button>
        </td>
        <td>{{ c.price * c.count }}</td>

      </tr>
      <tr>
        <td></td>
        <td colspan="2">{{ activeCount }}/{{ count }}</td>
        <td colspan="2">{{ total }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "cart",
  props: ['title'],
  data() {
    return {
      cart: JSON.parse(localStorage.getItem('cart')) || [],
      multipleSelection:[]
    }
  },
  watch: {
    cart: {
      // eslint-disable-next-line no-unused-vars
      handler(n, o) {
        this.setLocalData(n);
      },
      deep: true
    }
  },
  created() {
    this.$bus.$on('addCart', good => {
      const ret = this.cart.find(v => v.id === good.id);
      if (!ret) {
        //cart does not contain the data
        this.cart.push(good);
      } else {

        ret.count++;
      }
    });
  },
  methods: {
    setLocalData(n) {
      localStorage.setItem('cart', JSON.stringify(n));
    },
    remove(i) {
      if (window.confirm('Are you sure?')) {
        this.cart.splice(i, 1);
      }
    },
    add(i) {
      this.cart[i].count++;
    },
    subtract(i) {
      let count = this.cart[i].count;
      count > 1 ? this.cart[i].count -= 1 : this.remove(i);
    },
    toggleSelection(rows) {
      if (rows) {
        rows.forEach(row => {
          this.$refs.multipleTable.toggleRowSelection(row);
        });
      } else {
        this.$refs.multipleTable.clearSelection();
      }
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
      console.log(this.multipleSelection);
    }
  },
  computed: {
    count() {
      return this.cart.length
    },
    activeCount() {
      // eslint-disable-next-line no-debugger
      debugger
      return this.multipleSelection.filter(v => v.active).length
    },
    total() {
      return this.cart.reduce((sum, c) => {
        if (c.active)
          sum += c.price * c.count;
        return sum;
      }, 0)
    }
  }
}
</script>

<style scoped>

</style>