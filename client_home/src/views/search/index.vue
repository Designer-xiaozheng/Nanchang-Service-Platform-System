<template>
  <div class="page_search">
	<div class="warp">
	  <div class="container">
		<div class="row">
		  <div class="col-12">
			<div class="card_result_search">
			  <div class="title">搜索结果</div>
				<!-- 商品搜索结果 -->
			  <list_result_search
				:list="result_goods"
				title="手办商城"
				source_table="goods"
			  ></list_result_search>

				<!-- 文章搜索结果 -->
			  <list_result_search
				:list="result_article"
				title="热门资讯"
				source_table="article"
			  ></list_result_search>

				<!-- 论坛搜索结果 -->
			  <list_result_search
				:list="result_forum"
				title="交流论坛"
				source_table="forum"
			  ></list_result_search>

						  <list_result_search
				v-if="$check_action('/registered_users/list', 'get')"
				:list="result_registered_users_gender"
				title="注册用户性别"
				source_table="registered_users"
			  ></list_result_search>
												  <list_result_search
				v-if="$check_action('/hand_operated_shopping_mall/list', 'get')"
				:list="result_hand_operated_shopping_mall_specifications"
				title="手办商城规格"
				source_table="hand_operated_shopping_mall"
			  ></list_result_search>
						</div>
		  </div>
		</div>
	  </div>
	</div>
  </div>
</template>

<script>
import mixin from "../../mixins/page.js";
import list_result_search from "../../components/diy/list_result_search.vue";

export default {
  mixins: [mixin],
  data() {
	return {
	  "query": {
		word: "",
	  },
	  "result_goods": [],
	  "result_article": [],
	  "result_forum": [],
						"result_registered_users_gender":[],
												"result_hand_operated_shopping_mall_specifications":[],
				};
  },
  methods: {
	/**
	 * 获取${fmodel.filter.cart_name}
	 */
	get_goods() {
	  this.$get("~/api/goods/get_list?like=0", { page: 1, size: 10, title: this.query.word }, (json) => {
		if (json.result) {
		  this.result_goods = json.result.list;
		}
	  });
	},
	/**
	 * 获取文章
	 */
	get_article() {
	  this.$get("~/api/article/get_list?like=0", { page: 1, size: 10, title: this.query.word }, (json) => {
		if (json.result) {
		  this.result_article = json.result.list;
		}
	  });
	},
	/**
	 * 获取交流论坛
	 */
	get_forum() {
	  this.$get("~/api/forum/get_list?like=0", { page: 1, size: 10, title: this.query.word }, (json) => {
		if (json.result) {
		  this.result_forum = json.result.list;
		}
	  });
	},

				/**
	 * 获取gender
	 */
	get_registered_users_gender(){
		let url = "~/api/registered_users/get_list?like=0";
				this.$get(url, { page: 1, size: 10, "gender": this.query.word }, (json) => {
		  if (json.result) {
			var result_registered_users_gender = json.result.list;
			result_registered_users_gender.map(o => o.title = o['gender'])
	  			this.result_registered_users_gender = result_registered_users_gender
		 	}
		});
	},
										/**
	 * 获取specifications
	 */
	get_hand_operated_shopping_mall_specifications(){
		let url = "~/api/hand_operated_shopping_mall/get_list?like=0";
				this.$get(url, { page: 1, size: 10, "specifications": this.query.word }, (json) => {
		  if (json.result) {
			var result_hand_operated_shopping_mall_specifications = json.result.list;
			result_hand_operated_shopping_mall_specifications.map(o => o.title = o['specifications'])
	  			this.result_hand_operated_shopping_mall_specifications = result_hand_operated_shopping_mall_specifications
		 	}
		});
	},
			
  },
  components: { list_result_search },
	created(){
    this.query.word = this.$route.query.word || "";
  },
  mounted() {
	this.get_goods();
	this.get_article();
	this.get_forum();
					this.get_registered_users_gender();
											this.get_hand_operated_shopping_mall_specifications();
			  },
  watch: {
	$route() {
	  $.push(this.query, this.$route.query);
	  this.get_goods();
	  this.get_article();
	  this.get_forum();
				  this.get_registered_users_gender();
										  this.get_hand_operated_shopping_mall_specifications();
				},
  },
};
</script>

<style scoped>
.card_search {
  text-align: center;
}
.card_result_search>.title {
  text-align: center;
  padding: 10px 0;
}
</style>
