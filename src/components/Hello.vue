<template>
  <div>
    <router-link to="/update">
      更新
    </router-link>
    <ul class="box" id="dragbox">
        <li class="dragbox" v-for="(item,index) in dragList" :key="item.id" @dragstart="drag($event)">
            <a href="#" draggable="true" class="draglist" :data-id="item.id" :data-text="item.text" :data-index="index" data-origin="0">{{item.text}}</a>
        </li>
    </ul>
    <ul class="box">
      <li class="dragedbox" v-for="(item,index) in dragedList" @dragstart="drag($event)" @dragover="dragover($event)" @drop="drop($event)">
        <a href="#" draggable="true" class="dragedlist draglist"  :key="item.sortNo" :data-id="item.id" :data-text="item.text" :data-index="index" data-origin="1">{{item.text}}</a>
      </li>
    </ul>
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  name: 'hello',
  data () {
    return {
      dragList: [{
        text: '拖我0号',
        id: '0'
      }, {
        text: '拖我1号',
        id: '1'
      }, {
        text: '拖我2号',
        id: '2'
      }, {
        text: '拖我3号',
        id: '3'
      }, {
        text: '拖我4号',
        id: '4'
      }, {
        text: '拖我5号',
        id: '5'
      }, {
        text: '拖我6号',
        id: '6'
      }, {
        text: '拖我7号',
        id: '7'
      }, {
        text: '拖我8号',
        id: '8'
      }, {
        text: '拖我9号',
        id: '9'
      }],
      dragedList: [{
        text: '拖我5号',
        id: '5',
        sortNo: 0
      }, {
        text: '拖我6号',
        id: '6',
        sortNo: 1
      }, {
        text: '拖我7号',
        id: '7',
        sortNo: 2
      }, {
        text: '拖我8号',
        id: '8',
        sortNo: 3
      }, {
        text: '拖我9号',
        id: '9',
        sortNo: 4
      }, {
        text: null,
        id: null,
        sortNo: 5
      }, {
        text: null,
        id: null,
        sortNo: 6
      }, {
        text: null,
        id: null,
        sortNo: 7
      }]
    }
  },
  methods: {
    drag ($event) {
      var e = $event,
        _this = $(e.target),
        id = _this.data('id')
      if (id === undefined || id === null || id === '') {
        return
      }
      e.dataTransfer.effectAllowed = 'move'
      e.dataTransfer.setData('Origin', _this.data('origin'))
      e.dataTransfer.setData('Text', _this.data('text'))
      e.dataTransfer.setData('Id', id)
      e.dataTransfer.setData('Index', _this.data('index'))
    },
    dragover ($event) {
      $event.preventDefault()
    },
    drop ($event) {
      $event.preventDefault()
      var id = $event.dataTransfer.getData('Id')
      if (id === undefined || id === null || id === '') {
        return
      }
      var index = $($($event.target).parent()).index(),
        origin = $event.dataTransfer.getData('origin'),
        self = this,
        toTarget = {
          text: self.dragedList[index].text,
          id: self.dragedList[index].id,
          sortNo: self.dragedList[index].sortNo
        },
        ids = self.dragedList.map(item => item.id),
        text = $event.dataTransfer.getData('Text'),
        originIndx = $event.dataTransfer.getData('Index'),
        fromTarget = {
          text: text,
          id: id
        }
      if (origin === '0') {
        if (ids.indexOf(id) > -1) {
          return
        } else {
          fromTarget.sortNo = self.dragedList[index].sortNo
          self.dragedList.splice(index, 1, fromTarget)
        }
      } else {
        var temp = $.extend(true, [], self.dragedList)
        fromTarget.sortNo = temp[originIndx].sortNo
        temp[index] = fromTarget
        temp[originIndx] = toTarget
        self.dragedList = temp
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped >
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
.box{
  display: flex;
  
}
.box li{
  width: 100px;
  height: 100px;
  border: solid 1px #ccc; 
}
.box li a{
  display: block;
  width: 100px;
  height: 100px;
  line-height: 100px;
  text-align: center;
}
</style>
