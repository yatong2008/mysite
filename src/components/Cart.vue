<template>
    <div>
        <h2>{{title}}</h2>
        <table border="1">
            <tr>
                <th>#</th>
                <th>Course</th>
                <th>Item price</th>
                <th>Quantity</th>
                <th>Total Price</th>
            </tr>
            <tr  v-for='(c,index) in cart' :key='c.id'>
                <td>
                    <input type="checkbox" v-model='c.active' />
                </td>
                <td>{{c.title}}</td>
                <td>{{c.price}}</td>
                <td>
                    <button @click="subsctract(index)">-</button>
                    {{c.count}}
                    <button @click="add(index)">+</button>
                </td>                
                <td>{{c.price * c.count}}</td>

            </tr>
            <tr>
                <td></td>
                <td colspan="2">{{activeCount}}/{{count}}</td>
                <td colspan="2">{{total}}</td>
            </tr>            
        </table>
    </div>
</template>

<script>
    export default {
        name: "cart",
        props:['title', 'cart'],
        methods: {
            remove(i) {
                if(window.confirm('Are you sure?')) {
                    this.cart.splice(i,1);
                }
            },
            add(i) {
                this.cart[i].count++;
            },
            subsctract(i) {
                let count = this.cart[i].count;
                count > 1 ? this.cart[i].count -= 1 : this.remove(i);
            }
        },
        computed: {
            count() {
                return this.cart.length
            },
            activeCount() {
                return this.cart.filter(v => v.active).length
            },
            total() {
                return this.cart.reduce((sum, c) => {
                    if(c.active)
                        sum += c.price * c.count;
                    return sum;
                }, 0)
            }
        }
    }
</script>

<style scoped>

</style>