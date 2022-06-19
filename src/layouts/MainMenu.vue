<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>

        <q-toolbar-title>
          Idle App
        </q-toolbar-title>

        <div>v{{ version }}</div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <div class="q-pa-md q-gutter-md">
        <q-space />
        <div class="row text-center">
          <div class="q-pa-xs col-auto text-black">
            <div flat class="q-pa-xs bg-grey-5">
              <i class="on-left"><q-icon name="paid" color="brown-7" size="1.5em"/></i>
              <span class="text-bold">{{ kFormatter(currency) }}</span>
            </div>
          </div>
          <div class="q-pa-xs col-auto text-black">
            <div flat class="q-pa-xs bg-grey-5">
              <i class="on-left"><q-icon name="forest" color="brown-7" size="1.5em"/></i>
              <span class="text-bold">{{ kFormatter(wood) }}</span>
            </div>
          </div>
          <div class="q-pa-xs col-auto text-black">
            <div flat class="q-pa-xs bg-grey-5">
              <i class="on-left"><q-icon name="circle" color="grey-7" size="1.5em"/></i>
              <span class="text-bold">{{ kFormatter(stone) }}</span>
            </div>
          </div>
          <div class="q-pa-xs col-auto text-black">
            <div flat class="q-pa-xs bg-grey-5">
              <i class="on-left"><q-icon name="circle" color="grey-9" size="1.5em"/></i>
              <span class="text-bold">{{ kFormatter(coal) }}</span>
            </div>
          </div>
          <div class="q-pa-xs col-auto text-black">
            <div flat class="q-pa-xs bg-green-5 market" @click="showMarket()">
              <i class="on-left"><q-icon name="store" color="grey-9" size="1.5em"/></i>
              <span class="text-bold">STORE</span>
            </div>
          </div>
        </div>
        <q-card class="my-card bg-brown-8 text-white">
          <q-card-section vertical align="center">
            <q-icon name="forest" color="white" size="2em"/>
            <div class="text-h6">Tree</div>
            <div class="text-subtitle2">Chop this tree to get wood</div>
          </q-card-section>

          <q-card-actions vertical align="center" class="bg-brown-9">
            <q-btn flat class="bg-brown-7" @click="onClick('wood')">Chop</q-btn>
          </q-card-actions>
        </q-card>

        <q-card class="my-card bg-grey-7 text-white" v-if="f1">
          <q-card-section vertical align="center">
            <q-icon name="circle" color="grey-6" size="2em"/>
            <div class="text-h6">Rock mining Floor 1</div>
            <div class="text-subtitle2">Mine to get cobblestone</div>
          </q-card-section>

          <q-card-actions vertical align="center" class="bg-grey-8">
            <q-btn flat class="bg-grey-6" @click="onClick('stone')">Mine</q-btn>
          </q-card-actions>
        </q-card>
        <q-card class="my-card bg-grey-5 text-white" v-else>
          <q-card-section vertical align="center">
            <q-icon name="circle" color="grey-6" size="2em"/>
            <div class="text-h6">Rock mining Floor 1</div>
            <div class="text-subtitle2">1k $ to open</div>
          </q-card-section>

          <q-card-actions vertical align="center" class="bg-grey-8">
            <q-btn flat class="bg-grey-6" @click="open('f1')">OPEN</q-btn>
          </q-card-actions>
        </q-card>

        <q-card class="my-card bg-grey-9 text-white" v-if="f2">
          <q-card-section vertical align="center">
            <q-icon name="circle" color="grey-10" size="2em"/>
            <div class="text-h6">Rock mining Floor 2</div>
            <div class="text-subtitle2">Mine to get coal</div>
          </q-card-section>

          <q-card-actions vertical align="center" class="bg-grey-10">
            <q-btn flat class="bg-grey-8" @click="onClick('coal')">Mine</q-btn>
          </q-card-actions>
        </q-card>
        <q-card class="my-card bg-grey-5 text-white" v-else>
          <q-card-section vertical align="center">
            <q-icon name="circle" color="grey-8" size="2em"/>
            <div class="text-h6">Rock mining Floor 2</div>
            <div class="text-subtitle2">10k $ to open</div>
          </q-card-section>

          <q-card-actions vertical align="center" class="bg-grey-8">
            <q-btn flat class="bg-grey-6" @click="open('f2')">OPEN</q-btn>
          </q-card-actions>
        </q-card>
      </div>
    </q-page-container>
  </q-layout>
  
  <q-dialog v-model="dialogMarket" full-width>
    <q-card class="my-card bg-grey-5">
      <q-card-section class="bg-green-7">
        <div class="row">
          <div class="text-h6">Store</div>
          <q-space />
          <q-btn flat icon="close" class="bg-red-5" @click="close()"></q-btn>
        </div>
      </q-card-section>
      <q-card-section vertical class="bg-brown-3">
        <div class="row">
          <div class="col-4">
            <q-icon name="forest" color="brown-8" size="5em"/>
          </div>
          <div class="col-auto">
            <div class="text-h6">Wood</div><br />
            <div class="text-h8">
              {{ swood }} from {{ wood }}
              <q-separator />
              To Get: {{ swood*1 }}$
            </div>
          </div>
        </div>
        <div class="row">
          <div><q-btn class="bg-blue-7" @click="minus('wood')">-10</q-btn>&nbsp;<q-btn class="bg-blue-7" @click="plus('wood')">+10</q-btn></div>
          <div class="el-right"><q-btn flat class="bg-blue-7" @click="onSell('wood')">sell</q-btn>&nbsp;<q-btn flat class="bg-blue-7" @click="onSell('wood', 1)">sell all</q-btn></div>
        </div>
      </q-card-section>
      <q-card-section vertical align="left" class="bg-grey-3">
        <div class="row">
          <div class="col-4">
            <q-icon name="circle" color="grey-7" size="5em"/>
          </div>
          <div class="col-auto">
            <div class="text-h6">Stone</div><br />
            <div class="text-h8">
              {{ sstone }} from {{ stone }}
              <q-separator />
              To Get: {{ sstone*2 }}$
            </div>
          </div>
        </div>
        <div class="row">
          <div><q-btn class="bg-blue-7" @click="minus('stone')">-10</q-btn>&nbsp;<q-btn class="bg-blue-7" @click="plus('stone')">+10</q-btn></div>
          <div class="el-right"><q-btn flat class="bg-blue-7" @click="onSell('stone')">sell</q-btn>&nbsp;<q-btn flat class="bg-blue-7" @click="onSell('stone', 1)">sell all</q-btn></div>
        </div>
      </q-card-section>
      <q-card-section vertical align="left" class="bg-grey-5">
        <div class="row">
          <div class="col-4">
            <q-icon name="circle" color="grey-9" size="5em"/>
          </div>
          <div class="col-auto">
            <div class="text-h6">Coal</div><br />
            <div class="text-h8">
              {{ scoal }} from {{ coal }}
              <q-separator />
              To Get: {{ scoal*2 }}$
            </div>
          </div>
        </div>
        <div class="row">
          <div><q-btn class="bg-blue-7" @click="minus('coal')">-10</q-btn>&nbsp;<q-btn class="bg-blue-7" @click="plus('coal')">+10</q-btn></div>
          <div class="el-right"><q-btn flat class="bg-blue-7" @click="onSell('coal')">sell</q-btn>&nbsp;<q-btn flat class="bg-blue-7" @click="onSell('coal', 1)">sell all</q-btn></div>
        </div>
      </q-card-section>
    </q-card>
  </q-dialog>
</template>

<script>
import { LocalStorage } from 'quasar'
import { version } from '../../package.json'

export default ({
  name: 'MainLayout',

  meta () {
    return {
      title: 'Idle Mining'
    }
  },
  data () {
    return {
      wood: 0,
      stone: 0,
      coal: 0,
      currency: 0,
      dialogMarket: false,
      scoal: 0,
      swood: 0,
      sstone: 0,
      f1: false,
      f2: false,
      version: version
    }
  },
  mounted () {
    if (!LocalStorage.getItem('wood')) LocalStorage.set('wood', 0) 
    else this.wood = LocalStorage.getItem('wood')
    if (!LocalStorage.getItem('stone')) LocalStorage.set('stone', 0)
    else this.stone = LocalStorage.getItem('stone')
    if (!LocalStorage.getItem('coal')) LocalStorage.set('coal', 0)
    else this.coal = LocalStorage.getItem('coal')
    if (!LocalStorage.getItem('currency')) LocalStorage.set('currency', 0)
    else this.currency = LocalStorage.getItem('currency')
    if (!LocalStorage.getItem('f1')) LocalStorage.set('f1', false)
    else this.f1 = LocalStorage.getItem('f1')
    if (!LocalStorage.getItem('f2')) LocalStorage.set('f2', false)
    else this.f1 = LocalStorage.getItem('f2')
  },
  methods: {
    onClick (source) {
      if (source === 'wood') {
        this.wood = LocalStorage.getItem('wood') + 1
        LocalStorage.set('wood', this.wood)
      } else if (source === 'stone') {
        this.stone = LocalStorage.getItem('stone') + 1
        LocalStorage.set('stone', this.stone)
      } else if (source === 'coal') {
        this.coal = LocalStorage.getItem('coal') + 1
        LocalStorage.set('coal', this.coal)
      }
    },
    onSell (source, type = 0) {
      let count = 0
      if (source === 'wood') {
        count = (!type) ? this.swood : this.wood
        this.wood = LocalStorage.getItem('wood') - count
        count = count * 1
        this.currency = this.currency + count
        LocalStorage.set('wood', this.wood)
        LocalStorage.set('currency', this.currency)
        this.swood = 0
      } else if (source === 'stone') {
        count = (!type) ? this.sstone : this.stone
        this.stone = LocalStorage.getItem('stone') - count
        count = count * 2
        this.currency = this.currency + count
        LocalStorage.set('stone', this.stone)
        LocalStorage.set('currency', this.currency)
        this.sstone = 0
      } else if (source === 'coal') {
        count = (!type) ? this.scoal : this.coal
        this.coal = LocalStorage.getItem('coal') - count
        count = count * 3
        this.currency = this.currency + count
        LocalStorage.set('coal', this.coal)
        LocalStorage.set('currency', this.currency)
        this.scoal = 0
      }
      if (count > 0) {
        this.$q.notify({
          type: 'positive',
          message: 'Sell success.'
        })
      } else {
        this.$q.notify({
          type: 'negative',
          message: 'Input amount.'
        })
      }
    },
    showMarket () {
      this.dialogMarket = true
    },
    close () {
      this.dialogMarket = false
    },
    plus (source) {
      if (source === 'wood') {
        const swood = this.swood + 10
        this.swood = (LocalStorage.getItem('wood') > swood) ? swood : LocalStorage.getItem('wood')
      } else if (source === 'stone') {
        const sstone = this.sstone + 10
        this.sstone = (LocalStorage.getItem('stone') > sstone) ? sstone : LocalStorage.getItem('stone')
      } else if (source === 'coal') {
        const scoal = this.scoal + 10
        this.scoal = (LocalStorage.getItem('coal') > scoal) ? scoal : LocalStorage.getItem('coal')
      }
    },
    minus (source) {
      if (source === 'wood') {
        const swood = this.swood - 10
        this.swood = (swood > 0) ? swood : 0
      } else if (source === 'stone') {
        const sstone = this.sstone - 10
        this.sstone = (sstone > 0) ? sstone : 0
      } else if (source === 'coal') {
        const scoal = this.scoal - 10
        this.scoal = (scoal > 0) ? scoal : 0
      }
    },
    kFormatter(num) {
        return Math.abs(num) > 999 ? Math.sign(num)*((Math.abs(num)/1000).toFixed(1)) + 'k' : Math.sign(num)*Math.abs(num)
    },
    open(floor) {
      if (floor === 'f1') {
        if (this.currency >= 1000) {
          this.currency = this.currency - 1000
          this.f1 = true
          LocalStorage.set('f1', true)
          LocalStorage.set('currency', this.currency)
        } else {
          this.$q.notify({
            type: 'negative',
            message: 'Not enough money.'
          })
        }
      } else if (floor === 'f2') {
        if (this.currency >= 10000) {
          this.currency = this.currency - 10000
          this.f2 = true
          LocalStorage.set('f2', true)
          LocalStorage.set('currency', this.currency)
        } else {
          this.$q.notify({
            type: 'negative',
            message: 'Not enough money.'
          })
        }
      }
    }
  }
})
</script>
<style>
.market:active {
  background: #979797!important;
}

.el-right {
  float: right;
  margin-left: auto;
}
</style>