# MongoDB
## 数据增加
>### 插入单个文档
>*       db.a.insert({x1:55 , x2:66 , x3:77});
>### 插入多个文档
>* 一次性的往集合里插入多个文档,多个文档以数组的方式插入（中括号）
>   * db.a.insert([
>   * {name:'jack',age:19},
>   * {name:"jim",age:20},
>   * {name:"rose",age:18}
>   * ]);
>* **当给同一个字段/键/属性赋多个值的时候，取最后一个值**
>    *       db.a.insert({m:1 , m:2 , m:3});
>* **允许重复插入两次相同的文档，因为系统每次给文档分配不同的_id,不同的_id代表不同的文档**。
>    *       db.a.insert({x:1});
>    *       db.a.insert({x:1});
>* **插入时可以自定义id，但是id不能重复**
>    *       db.a.insert({_id:2,x:1})
>    * insert 在插入集合时，如果集合不存在，会先创建集合

>## 数据删除
>### 删除一条文档
>*       db 集合名 deleteOne （ {键值对 } ）
>*       db.product.deleteOne({item:"电脑"});
>
>### 删除多条文档
>*       db 集合名 deleteMany ({条件} );
>*       db.product.deleteMany({item:"电影票"})
>
>### 删除所有文档
>*       db.product.deleteMany({});
>
>### 删除集合
>*       db.abc.drop();
>
>### 删除数据库
>* 例如删除 test3：
>* db;
>* use test3;
>* db.dropDatabase();
>
>### 删除字段（unset）
>* db.product.updateMany({},{$unset:{stocks:1}});

## 数据修改
>*  **文档的更新主要用到以下两个函数：**
>    *       update({参数1},{参数2})
>    *       updateMany({参数1},{参数2})
>      * 在使用时，通常跟两个参数：
>      * 第一个参数位置放的是更新的条件，
>      * 第二个参数位置放的是更新的内容
>
>### 更新一个文档
>*       db.product.update({name:"猫妖传"},{$set:{price:55}});
> 
>### 更新多个文档:
>*       db.product.updateMany({item:"电影"},{$set:{stocks:150}});
> 
>### 关键字
>* $inc 关键字：
>  * 将所有商品的价格都增加2块钱：
>  *       db.product.updateMany({},{$inc:{price: 2}});
>  * 将所有商品的价格都减少2块钱：
>    *       db.product.updateMany({},{$inc:{price: -2}})；
>* $unset 关键字（可以删除字段）：
>   * 将所有商品的stocks字段删除：
>   *       db.product.updateMany({},{$unset:{stocks:1}})；
>* $set 关键字（可以增加字段）：
>   * 字段追加stocks：
>   *       db.product.updateMany({ },{$set:{stocks:100}})

## 数据查询
>### 文档的基础查询方式
>* **文档格式：**
>    * 键1 : 值1
>* **查询格式：db.集合名.find（条件）** 
>* $eq (等于)：
>    *       db.product.find({director:"张艺谋"});
>    *       db.product.find({director:{$eq:"张艺谋"}});
>* $ne(不等于)
>    * 查询不包含 "英雄"
>    *       db.product.find({name:{$ne:"英雄" }})
>* $gt(大于)，$gte(大于等于)：
>    * 查询价格高于280的商品信息
>    *       db.product.find({price:{$gt:280}})
>* $lt(小于),$lte(小于等于)
>    * 查询价格小于于280的商品信息
>    *       db.product.find({price:{$lt:280}})
>* $and (和)
>    * 查询价格在40到280之间的商品信息（包含边界）
>    *       db.product.find({ $and:[{price:{$gte:40} } ,{price:{$lte:280 } }] });
>    *       db.product.find({ price :{$gte:40 , $lte: 280}});
>* $or:
>    * 查询商品类型是电影票或者价格高于280的商品
>    *       db.product.find({$or:[{item:"ticket"},{price:{$gt:280}}]});

>### 内嵌文档的查询
>* **文档格式：** 
>    * 键：{ 键值对1，键值对2…..键值对n}
>    * 为一个键对应多对 键 : 值
>* **查询格式1：**
>    *       db.product.find({size:{length:75,width:50,uom:'cm'}}
>    * 不按内嵌文档录入时的顺序写就会查询不到
>* **查询格式2：**
>    * 例子：查询商品尺寸单位是cm的商品信息
>    *       db.product.find({"size.uom":"cm"});
>    * 涉及到内嵌文档里的字段的引用，通过“.”的方式来引用，
>    * 并且需要将这个字段放在引号里
>* 注意：
>    * 使用格式2查询时，如果有多条件，是会把每一项单独作为条件查询，只要满足一个条件即可被查询到
>    * 如果想要查询满足所有条件的项，则使用 $elemMatch
>    * 例子：示例
>        *   db.test.insert({"id":1, "members":[
>        *   {"name":"BuleRiver1", "age":27, "gender":"M"}, {"name":"BuleRiver2", "age":23, "gender":"F"}, {"name":"BuleRiver3", "age":21, "gender":"M"}
>        *   ]});
>        *       db.test.find({"members":{"name":"BuleRiver1"}});
>            *   查询的结果是空集
>        *       db.test.find({"members":{"name":"BuleRiver1", "age":27, "gender":"M"}});
>            *   完全匹配一个的时候才能获取到结果
>        *      db.test.find({"members":{"age":27, "name":"BuleRiver1", "gender":"M"}});
>            *   把键值进行颠倒,也得不到结果
>        *       db.test.find({"members.name":"BuleRiver1"});
>            *   可以查询出结果的
>        *       db.test.find({"members.name":"BuleRiver1", "members.age":27});
>            *   可以查询出结果
>        *       db.test.find({"members.name":"BuleRiver1", "members.age":23});
>            *   BuleRiver1是数组中第一个元素的键值，而23是数组中第
>            *   二个元素的键值，这样也可以查询出结果
>        *    $elemMatch+同一个元素中的键值组合
>            *       db.test.find({"members":{"$elemMatch":{"name":"BuleRiver1", "age":27}}});
>            *   可以查询出结果
>        *   $elemMatch+不同元素的键值组合
>            *       db.test.find({"members":{"$elemMatch":{"name":"BuleRiver1", "age":23}}});
>            * 查询不出结果

>### 内嵌数组的查询
>* **文档格式：**
>        *   键：[值1，值2, …… 值n ] 
>        *   为一个键对应多个取值
>        *   数组里的元素是有编号的，编号从0开始，这就意味着，可以通过编
>        *   号去取元素。可以通过“数组名.编号”的方式去取值。
>        *   比如要取数组xx里面的第一个值，我们用：xx.0，取第二个值：xx.1......以此类推。
>* **查询格式：**
>    * 格式1：
>        *   db. 集合名 .find ({ 数组名：[值1，值2 …值n] });
>        *       db.inventory1.find({tags:["red","blank"]})；
>        *   注意：这种方式只会查询到包含条件中所有值且顺序相同的数据
>    * 使用 $all：
>        *       db.inventory1.find({tags:{$all:["blank","red"]}});
>        *   查询包含所有值的数据，不在意顺序
>    * 使用 $in :
>        *       db.inventory1.find({tags:{$in:["blank","red"]}});
>        *   查询包含有条件中的值的数据，只要有一个就可以匹配到
>    * 使用 $size :
>        *       db.inventory1.find({tags:{$size:1}});
>        *   可以通过查找数组里元素个数来查询数据
>    * 按数组元素的编号查询
>        *   例如：查询第二个元素大于25的记录
>        *   集合名.编号 用引号" "括起来
>        *       db.inventory1.find({"dim_cm.1":{$gt:25}})；
>    * 使用 $elemMatch
>        * 可用于数组的值范围，查询满足上下限的元素
>        * 找到数组dim_cm中某个元素大于15且小于20
>        *       db.inventory1.find({dim_cm:{$elemMatch:{$gt:15,$lt:20}}}）;
>        * and 就是判断两次,第一次 是把数组里的值循环一遍,有大于14的,通过,第二次,再循环一遍数组,有小于20的,通过,这样这个数组就被选出来了
>        * elemMatch呢,只循环一次数组的每个值 ,但判断的时候判断两次,是 14<值<20 这样判断
>        * 所以and 的时候 , 14 <20 ,20>14 就通过筛选了;而elemMatch呢,就是 14 !<14<20不通过,14<20 !<20 不通过,最终这个数据不通过筛选

>### 文档数组的查询
>* 文档格式：元素是文档的数组
>        *   db.集合名.insert（数组名：[  {文档1}，{文档2}… ] ）;
>        *       db.inventory2.insert([{item:"journal",instock:[{warehouse:"A",qty:5},{warehouse:"C",qty:15}]},{item:"notebook",instock:[{warehouse:"C",qty:5}]},{item:"paper",instock:[{warehouse:"A",qty:60},{warehose:"B",qty:15}]},{item:"planner",instock:[{warehouse:"A",qty:40},{warehouse:"B",qty:5}]},{item:"postcard",instock:[{warehouse:"B",qty:15},{warehouse:"C",qty:35}]}])；
>* 例1:查询在A仓库有5个数量的商品信息：
>            *       db.inventory2.find({instock:{warehouse:"A",qty:5}});
>            *   如果查询的元素顺序与内嵌文档内元素的顺序不匹配，则无结果
>* 使用$elemMatch:
>    * 查询在A仓库有5个数量的商品信息(顺序无所谓也可以查询到)
>    *      db.inventory2.find({instock:{$elemMatch:{qty:5,warehouse:"A"}}})
>* 可以使用序号定位数组内文档：
>    *   查询instock第一个数组元素的qty的值小于等于20的记录
>    *       db.inventory2.find({"instock.0.qty":{$lte:20}})
>* 在数组内所有文档中查询：
>    *   查询instock数组的任一元素的qty的值小于等于20的记录
>    *       db.inventory2.find({"instock.qty":{$lte:20}}

>### 查询特定字段
>* 格式：find( {查询条件} ，{显示条件}  )；
>* 显示条件：
>    * 要显示的 字段:1
>    * 不显示的 字段:0
>    * 对id 也适用
>    * 不可以混用（除了id字段）

>### 空值查询
>* 查询某字段为空或者没有某个字段的文档:
>    * 例子：
>    *     db.cc.find({item:null})
>* 只查询字段为空：
>    * 例子：
>    *     db.cc.find({item:{$type:10}});
>* 只查询字段不存在：
>    * 例子：
>    *     db.cc.find({item:{$exists:falue}});
>    * 思考：
>    * 查询某个字段存在可以这样：
>        * db.cc.find({item:{$exists:true}});
>
>### 模糊查询
>* 查询格式：
>    *   { < field （即键）>： { $ regex ： / pattern / ， $ options ： ‘’ } }
>    *   { < field >： { $ regex ： ‘pattern’ ， $ options ： ‘’ } }
>    *   { < field > ： { $ regex ： / pattern / < options > } }
>* 模糊查询可以使用 $regex 运算符通过正则表达式来进行匹配查询。
>    *   例：
>        *     db.member.find({"name":{ $regex:/XXX/ }})

>### 结果排序
>    * 格式：
>        *   **升序**（字段：1）：
>            *   例：
>            *       db.cc.find({item: "电影票"}).sort({price: 1});
>        *   **降序**（ 字段：-1）：
>            *   例：
>            *       db.cc.find({item: "电影票"}).sort({price: -1});
>        *   **多条件**：
>            *       db.cc.find({item: "电影票"}).sort({price: 1，stocks:-1});
				
		
				
		
