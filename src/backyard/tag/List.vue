<template>
  <div class="animated fadeIn">
    <div class="row">
      <div class="col-md-12">
        <div class="pedia-navigation">
          <span class="item active">标签列表</span>
          <span class="tool">
            <router-link class="btn btn-primary btn-sm" to="/by/tag/create">
              <i class="fa fa-plus"></i>
              创建标签
            </router-link>
          </span>
        </div>
      </div>
    </div>

    <div class="row">
      <div class="col-xs-12">
        <NbPlainFilter :filters="pager.filters" @change="search"/>
      </div>
      <div class="col-xs-12">
        <div class="row">
          <div class="col-md-6" v-for="(tag,index) in pager.data">

            <TagCell :tag="tag" size="detail-lg" :operate="true" :delCallback="refresh"/>

          </div>
        </div>
      </div>


      <div class="col-xs-12 mt20">
        <NbPager :pager="pager" :callback="refresh"/>
      </div>


    </div>

  </div>
</template>

<script>
  import NbPlainFilter from '../../common/widget/filter/NbPlainFilter.vue'
  import NbPager from '../../common/widget/NbPager.vue'
  import Pager from '../../common/model/base/Pager'
  import Tag from '../../common/model/tag/Tag'
  import TagCell from './widget/TagCell'
  import NbTank from '../../common/widget/NbTank'
  import NbTanks from '../../common/widget/NbTanks'
  import Tank from "../../common/model/tank/Tank";
  import {UserRole} from "../../common/model/user/UserRole";

  export default {
    data() {
      return {
        pager: new Pager(Tag),
        user: this.$store.state.user,
        tank: new Tank("*", false, 1024 * 1024, "单个文件的测试上传"),
        tanks: []
      }
    },
    components: {
      NbPlainFilter,
      NbPager,
      TagCell,
      NbTank,
      NbTanks
    },
    methods: {
      search() {
        this.pager.page = 0
        this.refresh()
      },
      refresh() {
        this.pager.httpFastPage()
      }
    },
    created() {
      if (this.user.role === UserRole.ADMIN) {
        this.pager.getFilter('userUuid').visible = true
      } else {
        this.pager.setFilterValue('userUuid', this.user.uuid)
      }
    },
    mounted() {
      this.pager.enableHistory()
      if (!this.pager.getFilterValue('userUuid')) {
        this.pager.setFilterValue('userUuid', this.user.uuid)
      }

      this.refresh()
    }
  }
</script>

<style>

</style>
