## 服务端返回消息类型的定义
> msg_type为服务端返回的消息类型
> 暂定4位
> 1000-1999
> 前2位表示逻辑模块
> 后2位表示具体操作

例如
### 10 - 农场模块
- 1001(10-01)表示农场全景
```
{
	msg_type = 1001,
	buildings = {

	},
	animal_farms = {

	},
	...
}
```

- 1002(10-02)表示邻居农场全景
```
{
	msg_type = 1002,
	buildings = {

	},
	animal_farms = {

	},
	...
}
```

- 1003(10-03)表示修改农场名称
```
{
	msg_type = 1003,
	name = "new farm name"
}
```

- 1004 农场升级
```
{
	msg_type = 1004,
	exp = 100,
	level = 1,
	rewards = [
		{
			item_id = 101111,
			count = 3,
		} * N
	]
}
```
> 升级解锁新物品由客户端判断

- 1005 农场地块扩建
```
{
	msg_type = 1005,
	block_id = 1,
}
```

### 11 - 建筑模块
- 1101(11-01)表示添加建筑
```
{
	msg_type = 1101,
	building_id = 11111,
	x = 30,
	y = 30,
}
```
- 1102(11-02)表示移动建筑
```
{
	msg_type = 1101,
	building_id = 11111,
	x = 30,
	y = 30,
	from_x = 10,
	from_y = 10,
}
```
### 12 - 农田模块
- 1201(12-01)种植作物
```
{
	msg_type = 1201,
	list = {
		{
			item_id = 1,
			fields = [1,2,3,4,],
		{
			item_id = 2,
			fields = [1,2,3,4,],
		},
	},
}
```
> 同一个种植操作可以在不同的农田上种植

- 1202(12-02)收割作物
```
{
	msg_type = 1202,
	fields = [1,2,3,4,],
}
```
> 一次操作可以收割多个农田

- 1203(12-03)使用彩虹币快速收割农田
```
{
	msg_type = 1203,
	fields = [1,2,3,4,], -- 彩虹比收割只能一次收割一块，也使用数组
	rainbow_coin = 1,
}
```
> 一次操作可以收割多个农田


### 13 - 工厂模块
- 1301(13-01)生产物品
```
{
	msg_type = 1301,
	factory_id = 1, -- 工厂id
	item_id = 1, -- 物品模板id
	id = 1, -- 物品id
}
```

- 1302(13-02)收获物品
```
{
	msg_type = 1302,
	factory_id = 1, -- 工厂id
	id = 1, -- 物品id
}
```

- 1303(13-03)工厂生产栏位扩展
```
{
	msg_type = 1303,
	factory_id = 1, -- 工厂id
	capacity = 3, -- 扩展后的栏位上限
}
```

### 14 - 任务模块
- 1401 任务列表
```
{
	msg_type = 1401,
	quests = [
		{
			quest_id = 1,
			index = 1,
			remain_time = 1000, -- 表示任务还剩多长时间显示（一般为0，默认显示，撕掉后更新，变成刷新时间）
			name = "name",
			desc = "desc",
			reward_gold = 10,
			reward_exp = 20,
			items = [
				{
					item_id = 101011,
					count = 1,
				},
				{
					item_id = 101011,
					count = 10,
				},
				{
					item_id = 1239,
					count = 1,
				}
			]
		} * N 
	]
}
```

- 1402 完成任务
```
{
	msg_type = 1402,
	quest_id = 1,
}
```
> TODO 任务奖励金币和经验前端计算

- 1403 撕掉/重置任务
```
{
	msg_type = 1403,
	quest = {
		quest_id = 1,
		index = 1,
		remain_time = 12000,
		name = "name",
		desc = "desc",
		reward_gold = 10,
		reward_exp = 20,
		items = [
			{
				item_id = 101011,
				count = 1,
			},
			{
				item_id = 101011,
				count = 10,
			},
			{
				item_id = 1239,
				count = 1,
			}
		]
	}
}
```
> 撕掉任务后直接返回一个新任务

- 1404 使用彩虹币刷新任务
```
{
	msg_type = 1404,
	quest = {
		quest_id = 1,
		index = 1,
		remain_time = 0,
		name = "name",
		desc = "desc",
		reward_gold = 10,
		reward_exp = 20,
		items = [
			{
				item_id = 101011,
				count = 1,
			},
			{
				item_id = 101011,
				count = 10,
			},
			{
				item_id = 1239,
				count = 1,
			}
		]
	}
}
```

### 15 - 路边摊（类型暂定）
- 1501 路边摊列表
```
{
	msg_type = 1501,
	free_ad_time = 12000,
	stalls = [
		{
			item_id = 111213,
			count = 12313,
			gold = 123,
			index = 1,
			is_ad = true,
		} * N 
	]
}
```

- 1502 路边摊上货
```
{
	msg_type = 1502,
	item_id = 101101,
	count = 19,
	gold = 1,
	is_ad = true,
	index = 3,
}
```

- 1503 路边摊撤下货物
```
{
	msg_type = 1503,
	index = 1,
}
```

- 1504 路边摊打广告
```
{
	msg_type = 1504,
	index = 1,
}
```

- 1505 路边摊售卖完成金币收获
```
{
	msg_type = 1505,
	index = 1,
}
```

- 1506 路边摊扩容
```
{
	msg_type = 1505,
	capacity = 8,
}
```

- 1507 路边摊购买广告
```
{
	msg_type = 1507,
	rainbow_coin = 8,
}
```

### 16 - 广告板
```
{
	msg_type = 1601,
	ads = [
		{
			item_id = 1010101,
			count = 2,
			gold = 10,
			farm_id = 1101,
			head1 = "1.jpg",
			head2 = "2.jpg",
			farm_name = "farm famous"
		} * N
	]
}
```

- 1602 点击广告查看具体农场
```
{
	msg_type = 1601,
	data = {},
}
```
> 具体数据同访问邻居返回的农场全景

### 17 - 轮船
- 1701 轮船任务列表

- 1702 完成轮船任务

- 1703 轮船任务请求帮助

- 1704 完成全部任务轮船出海

- 1705 轮船任务快速开启

### 18 - 果树
- 1801 种植果树

- 1802 收集果树

- 1803 使用彩虹币快速收集果树

- 1804 砍果树

### 19 - 采矿
- 1901 采矿
```
{
	msg_type = 1901,
	item_id = 1111212,
	count = 1,
}
```

### 20 - 蜜蜂
- 2001 放置蜂箱

- 2002 放置花丛

- 2003 放置蜜蜂

- 2004 收获蜂蜜

- 2005 砍花丛

### 21 - 每日任务
- 2101 每日任务列表
```
{
	msg_type = 2101,
	quests = [
		{
			id = 1,
			item_id = 10121212,
			count = 1,
		} * N
	],
}
```

- 2102 完成每日任务
```
{
	msg_type = 2102,
	quest_id = 1,
}
```

### 22 - 扭蛋
- 2201 扭蛋结果
```
{
	msg_type = 2201,
	item_id = 1,
	count = 1,
	rainbow_coin = 1,
}
```

### 23 - 转盘
- 2301 转盘列表
```
{
	msg_type = 2301,
	roulletes = [
		{
			index = 1,
			item_id = 123123,
			count = 1,
			rainbow_coin = 1,
		} * N
	]
}
```

- 2302 转盘结果
```
{
	msg_type = 2302,
	index = 1,
}
```

### 24 - 成就
- 2401 成就列表
```
{
	msg_type = 2401,
	acheviments = [
		{
			id = 1,
			level = 1,
			count = 2,
		} * N
	]
}
```

- 2402 领取成就奖励
```
{
	msg_type = 2402,
	ache_id = 1,
	level = 1,
}
```

- 2403 操作触发成就
```
{
	msg_type = 2403,
	ache_id = 1,
	level = 1,
}
```

### 25 - 好友帮助
- 2501 好友复活果树
```
{
	msg_type = 2501,
	id = 111,
}
```

- 2502 好友完成轮船任务
```
{
	msg_type = 2502,
	index = 2
}
```

### 26 - 农场收获/生产的随机掉落，随机宝箱开启
- 2601 农场收获/生产的随机掉落
```
{
	msg_type = 2601,
	item_id = 1,
	count = 1,
}
```

- 2602 随机宝箱开启
```
{
	msg_type = 2602,
	item_id = 1,
	count = 1,
	rainbow_coin = 0,
}
```
> 可能获得物品和彩虹币
> 获得物品是rainbow_coin为0



### 90 - 农田模块
- 9001 操作记录
```
{
	msg_type = 9001,
	opetations = {
		{
			msg_type = 1202,
			farm = {
				1,2,3,4,
			}
		},
		{
			msg_type = 1303,
			factory_id = 1, -- 工厂id
			capacity = 3, -- 扩展后的栏位上限
		},
	}
}
```
> operations 为具体操作记录的列表，按照时间排序，
> 每一条能够完整描述一个操作，客户端能够完整完整重播这个操作的逻辑

