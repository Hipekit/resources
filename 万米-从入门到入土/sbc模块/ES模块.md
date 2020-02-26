# 1、H5端商品搜索

# 1.1 接口调用

* goods/spuListFront：未登陆时，查询商品分页
* goods/allGoodsCates：从缓存中获取商品分类信息列表
* config/goodsDisplayDefault：前台商品列表默认展示维度
* isGoodsEvaluate：商品评价开关是否开启
* distribute/queryOpenFlag：查询平台端-社交分销总开关状态
* distribute/distributor-info：根据会员的Id查询该会员的分销员信息
* userSetting/isVisitWithLogin：查询访问是否需要登录

# 2、接口说明

# 2.1 goods/spuListFront

### 参数封装

keword：关键词

brandIds：品牌

sortFlag：排序字段

pageNum：第几页

pageSize：每页数量

cateId：分类编号

### 流程

封装参数：

根据营销编号封装SKU编号

分销商品状态

查询商品开关

上下架状态：YES

店铺状态：OPENING

审核状态：CHECKED

删除标识：NO

会员等级

会员等级折扣



分页查询SKU

* 拆分关键词
* 商品分类
* 店铺分类
* 设定排序
* 聚合参数（品牌、分类、规格）









