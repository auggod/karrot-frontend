<template>
  <QCard
    v-if="entry"
    class="no-margin"
  >
    <QList
      class="full-width"
      v-if="entry"
    >
      <QItem
        class="bg-tertiary"
      >
        <QItemSide
          color="white"
          icon="fas fa-fw fa-info"
        />
      </QItem>

      <QItem dense>
        <QItemSide icon="far fa-fw fa-clock" />
        <QItemMain>
          <QItemTile label>
            {{ $d(new Date(entry.date), 'long') }},
            <DateAsWords
              :date="entry.date"
              style="display: inline"
            />
          </QItemTile>
        </QItemMain>
      </QItem>

      <QItem dense>
        <QItemSide icon="fas fa-fw fa-user" />
        <QItemMain>
          <QItemTile>
            <ProfilePicture
              v-for="user in entry.users"
              :key="user.id"
              :user="user"
            />
          </QItemTile>
        </QItemMain>
      </QItem>

      <QItem dense>
        <QItemSide icon="far fa-fw fa-comment" />
        <QItemMain>
          <QItemTile label>
            {{ entry.message }}
          </QItemTile>
        </QItemMain>
      </QItem>

      <QItem
        v-if="entry.group && entry.group.name"
        dense
      >
        <QItemSide icon="fas fa-fw fa-home" />
        <QItemMain>
          <QItemTile label>
            <RouterLink :to="{name: 'group', params: { groupId: entry.group.id }}">
              {{ entry.group.name }}
            </RouterLink>
          </QItemTile>
        </QItemMain>
      </QItem>

      <QItem
        v-if="entry.store && entry.store.name"
        dense
      >
        <QItemSide icon="fas fa-fw fa-shopping-cart" />
        <QItemMain>
          <QItemTile label>
            <RouterLink :to="{name: 'store', params: { groupId: entry.store.group.id, storeId: entry.store.id }}">
              {{ entry.store.name }}
            </RouterLink>
          </QItemTile>
        </QItemMain>
      </QItem>
    </QList>
    <QList
      v-if="entry.payload"
      striped
    >
      <QItem
        class="bg-tertiary"
      >
        <QItemSide
          color="white"
          icon="far fa-fw fa-file-alt"
        />
      </QItem>
      <HistoryPayloadDetail
        v-for="(value, key) in entry.payload"
        :key="key"
        :label="key"
        :value="value"
      />
    </QList>
    <QList>
      <QItem class="text-white">
        <QBtn
          @click="toggleRaw()"
          color="secondary">Raw data
        </QBtn>
      </QItem>
      <QItem v-if="raw">
        <pre style="white-space: pre-wrap">
          {{ entry }}
        </pre>
      </QItem>
    </QList>
  </QCard>
</template>

<script>
import {
  QBtn,
  QCard,
  QList,
  QItem,
  QItemMain,
  QItemTile,
  QItemSide,
} from 'quasar'
import ProfilePicture from '@/users/components/ProfilePicture'
import DateAsWords from '@/utils/components/DateAsWords'
import HistoryPayloadDetail from '@/history/components/HistoryPayloadDetail'

export default {
  props: {
    entry: {
      type: Object,
      default: null,
    },
  },
  components: {
    QBtn,
    QCard,
    QList,
    QItem,
    QItemMain,
    QItemTile,
    QItemSide,
    ProfilePicture,
    DateAsWords,
    HistoryPayloadDetail,
  },
  methods: {
    toggleRaw () {
      this.raw = !this.raw
    },
  },
  data () {
    return {
      raw: false,
    }
  },
}
</script>

<style scoped lang="stylus">
</style>
