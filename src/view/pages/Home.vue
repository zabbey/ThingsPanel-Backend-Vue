<template>
  <div class="homes rounded">
    <!--begin::Home-->
    <!--最近访问-->
    <div class="card-custom card-stretch gutter-b mb-4.5">
      <MixedWidget3></MixedWidget3>
    </div>
    <div class="card-custom card-stretch gutter-b mb-4.5" style="margin-top:-22px">
      <MixedWidget4></MixedWidget4>
    </div>
    <!--运行状态-->
    <!--<div>
      <MixedWidget2></MixedWidget2>
    </div>-->
    <!--<div class="row">
      <div class="col-md-6">
      &lt;!&ndash;告警信息&ndash;&gt;
        <ListWidget3></ListWidget3>
      </div>
      <div class="col-md-6">
      &lt;!&ndash;操作日志&ndash;&gt;
        <ListWidget9></ListWidget9>
      </div>
      <div class="clear"></div>
    </div>-->
    <!--end::Home-->
  </div>
</template>
<style>
.clear {
  clear: both;
}
 
</style>
<style scoped>
/* #app {
    height: 100%;
    width: 100%;
    padding: 0px;
    margin: 0px;
  } */
.guidbtn {
  /*background: #5B92FF;padding: 15px 15px;*/
  color: #fff;
  border-radius: 4px; /*font-size: 15px;font-weight: bolder;*/
}
.font-des {
  font-size: 12px;
  font-weight: 100;
  color: #a8c5ff;
}
.arrowicon {
  margin-left: 20px;
  color: #2a45c5;
  font-size: 25px;
  position: relative;
  bottom: 3px;
}

</style>
<script>
import MixedWidget3 from "@/view/content/widgets/mixed/Widget3.vue";
import MixedWidget4 from "@/view/content/widgets/mixed/Widget4.vue";
import ListWidget10 from "@/view/content/widgets/list/Widget10.vue";

import { REFRESH } from "@/core/services/store/auth.module";
import { local_url } from "@/api/LocalUrl"
import ApiService from "@/core/services/api.service";
import axios from "axios"

export default {
  name: "home",
  data: () => ({
    isshowguide: false,
    guidlist: [],
  }),
  components: {
    ListWidget10,
    MixedWidget3,
    MixedWidget4,
  },
  created() {
    this.ajaxdata();

    // axios.get('http://localhost:3001/test')
    //   .then(res => {
    //     console.log("localhost:3001", res)
    //   })
    
  },
  methods: {
    ajaxdata() {
      ApiService.post(local_url + "api/asset/work_index", {
        work_name: "",
        page: 1,
      }).then(({ data }) => {
        if (data.code == 200) {
          if (data.data.data.length > 0) {
            this.isshowguide = true;
            this.getguidlist();
          }
        } else if (data.code == 401) {
          this.$store.dispatch(REFRESH).then(() => {});
        } else {
          //
        }
      });
    },

    getguidlist() {
      ApiService.post(local_url + "api/navigation/list", {
        work_name: "",
      }).then(({ data }) => {
        console.log("指南列表");
        console.log(data);
        if (data.code == 200) {
          this.guidlist = data.data;
        } else if (data.code == 401) {
          this.$store.dispatch(REFRESH).then(() => {});
        } else {
          //
        }
      });
    },
  },
};
</script>
