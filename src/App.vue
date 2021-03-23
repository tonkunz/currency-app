<template>
  <div class="container grid-lg my-2 py-2">

    <div class="card  mb-2" v-if="listenQuotes.length">
      <div class="card-header">
        <h4 class="h4">Acompanhando</h4>
        <div class="card-body">
          <WatchListQuotes
            :listen-quotes="listenQuotes"
            @unlisten="onUnlisten"
          />
        </div>
      </div>
    </div>

    <div class="card">
      <div class="card-header">
        <h4 class="h4">Todas as Moedas</h4>
      </div>
      <div class="card-body">
        <ListQuotes
          :quotes="quotes"
          :listen-quotes="listenQuotes"
          @listen="onListen"
          @unlisten="onUnlisten"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { onMounted, reactive, toRefs } from 'vue'
import api from '@/services/api'
import ListQuotes from '@/components/ListQuotes.vue'
import WatchListQuotes from './components/WatchListQuotes.vue'

export default {
  name: 'App',
  components: { ListQuotes, WatchListQuotes },
  setup() {
    // Reactive component state
    const data = reactive({
      quotes: {},
      listenQuotes: []
    })

    // LifeCycle
    onMounted(async () => {
      const response = await api.all()
      data.quotes = response.data
    })

    
    const onListen = (quote) => {
      data.listenQuotes.push(quote)
    }

    const onUnlisten = (quote) => {
      data.listenQuotes = data.listenQuotes.filter(q => quote != q);
    }

    return {
      ...toRefs(data),
      onListen,
      onUnlisten
    }
  }
}
</script>
