
<template>
    <div class="outer">
    <table class="table table-hover" v-cloak>
        <thead>
        <tr>
            <th class="text-right" @click="sortBy('id')">序号</th>
            <th class="text-right" @click="sortBy('name')">书名</th>
            <th class="text-right" @click="sortBy('author')">作者</th>
            <th class="text-right" @click="sortBy('price')">价格</th>
            <th class="text-right">操作</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="book in orderedParam">
            <td class="text-right">{{book.id}}</td>
            <td class="text-right">{{book.name}}</td>
            <td class="text-right">{{book.author}}</td>
            <td class="text-right">{{book.price}}</td>
            <div v-if="book.id%2==0">
                <td class="text-right">
                    <button type="button" class="btn btn-success" @click="delBook(book.id)">删除</button>
                </td>
            </div>
            <div v-else>
                <td class="text-right">
                    <button type="button" class="btn btn-danger" @click="delBook(book.id)">删除</button>
                </td>
            </div>
        </tr>
        <tr>
            <td class="text-right" colspan="5">
                <h4>总价:{{sum}}</h4></td>
        </tr>
        </tbody>
    </table>
    <div id="add-book">
        <legend>添加书籍</legend>
        <div class="form-group">
            <label for="">书名</label>
            <input type="text" class="form-control" v-model="book.name">
        </div>
        <div class="form-group">
            <label for="">作者</label>
            <input type="text" class="form-control" v-model="book.author" >
        </div>
        <div class="form-group">
            <label for="">价格</label>
            <input type="text" class="form-control" v-model="book.price">
        </div>
        <button class="btn btn-primary btn-block" v-on:click="addBook()">添加</button>
    </div>
    </div>
</template>
<script>
export default {
  name: 'book',
  mounted: function () {
    var self = this
    this.$http.get('https://api.myjson.com/bins/r8mm').then(function (data) {
      console.log('success:    ' + window.JSON.stringify(data))
//      self.$set('books', window.JSON.stringify(data.body))
      data.body.forEach(function (item, index) {
        self.books.push(item)
      })
      console.log(self.books)
    }, function (data, status, request) {
      console.log('fail' + status + ',' + request)
    })
  },
  data () {
    return {
      sortparam: '',
      books: [{
        id: 0,
        author: '11',
        name: '22',
        price: '33'
      }],
      book: {}
    }
  },
  computed: {
    sum: function () {
      var result = 0
      for (var i = 0; i < this.books.length; i++) {
        result = Number(this.books[i].price) + result
      };
      return result
    },
    orderedParam: function () {
      return window._.orderBy(this.books, this.sortparam)
    }
  },
  methods: {
    addBook: function () {
      this.book.id = this.books.length + 1
      this.books.push({
        id: this.book.id,
        author: this.book.author,
        price: this.book.price,
        name: this.book.name
      })
      this.book = {}
    },
    delBook: function (id) {
      var self = this
      this.books.forEach(function (item, index) {
        if (item.id === id) {
          console.log(self, item.id, id)
          self.books.splice(index, 1)
        }
      })
    },
    sortBy: function (sortparam) {
      this.sortparam = sortparam
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h1, h2 {
        font-weight: normal;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }
</style>
