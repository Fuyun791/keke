## 刻课-教学小程序

一款面向智慧校园中的教学管理方向的小程序。结合当时的疫情背景下，我们主要关注网课，兴趣页，学生信息管理这两个模块的实现。

网课模块（**我负责**）：

1. 首页的课程展示（第一版是另一位组员写的，然后的代码页面布局是我精修的）
2. 课程详情页的展示
3. 课程内的评论区
4. 课程推荐（未加入）

兴趣：

1. 这个模块就是展示小组信息，文章，兴趣等，做的比较简单。

学生信息管理：

1. 个人信息
2. 课程信息
3. 学分展示

## 部分页面展示

图片需要能访问外网才能显示

<figure>
	<a href="static\image\home.jpg"><img height="600px" src="static\image\home.jpg"></a>
    <a href="static\image\my.jpg" style="margin-left: 60px"><img height="600px" src="static\image\my.jpg"></a>
    <a href="static\image\discuss.jpg"><img height="600px" src="static\image\discuss.jpg"></a>
    <a href="static\image\intrset.jpg" style="margin-left: 60px"><img height="600px" src="static\image\intrset.jpg"></a>
</figure>


## 项目运行

因为我们是直接使用 HBuilderX 创建的项目进行运行的，所以无法使用 npm 进行运行，所以最好的情况是安装 HBuilderX 把项目导入就可以运行了。

但如果你并不愿意下载 HBuilderX，我同时也上传了 uni-app 编译为微信小程序时的源码，路径为 unpackage\dist\dev\mp-weixin。导入微信开发者后加入自己的 appid 即可使用。（目前服务还在运行）（测试小程序账号：17920208，密码：123456，部分功能需要登录）

## 小程序技术选型

|        技术        |     说明     |
| :----------------: | :----------: |
|      uni-app       |   跨端技术   |
|        Vue         |   前端框架   |
| uni-app 官网的组件 |   简化开发   |
|       Nginx        | 轻量级服务器 |

## 后端服务选型

|        技术        |     说明     |
| :----------------: | :----------: |
|     Spring Boot    |   框架   |
|   Spring Security  |   安全框架   |
|        Redis       |   用于存token与保存评论   |
|        Mysql       | 数据库 |

项目使用了MVVM的模式。

## 开发中碰上比较细节的问题

1. 手机测试时点击输入框时，键盘的上移导致页面整体上移
2. 评论区某行想要点赞，同时触发这行的点击评论事件的，而且还触发了外层的点击效果。

#### 问题 1 的出现与解决

这个问题一开始在写 course.vue 也就是图一，图二页面时可能看不出来（我是开发到 course-discuss.vue 页面的时候才发现这个问题），因为我这个页面的布局是上面视频，下面评论区，所以页面被移动其实感受不大出来。

但在点击更多评论里的二层评论区时也就是图三图四（course-discuss.vue）的开发中，因为布局最上面是外层评论，页面的上移就会很明显会将外层评论直接顶上去。

<table>
    <tr>
        <td><center><div>[图一]</div><img src = static\image\discuss.jpg  width = 60% alt="图一" title="图一"></center></td>
        <td><center><div>[图二]</div><img src = static\image\discuss-shen.jpg  width = 60% alt="图二" title="图二"></center></td>
    </tr>
</table>

<table>
    <tr>
        <td><center><div>[图三]</div><img src = static\image\onther-discuss.jpg  width = 60% alt="图三" title="图三"></center></td>
        <td><center><div>[图四]</div><img src = static\image\other-discuss-shen.jpg  width = 60% alt="图四" title="图四"></center></td>
    </tr>
</table>

解决办法如下：

识别输入框出现时的高度，动态修改底部评论框的高度

```javascript
<view class="discuss-button" :style="{bottom: inputBottom + 'px'}">
	<view class="discuss-button-in">
		<view style="width: 100%;padding: 7px 15px;">
            <input class="inputDiscuss" cursor-spacing="15"
             :adjust-position="false" @focus="inputFocus"
                  @blur="inputBlur" v-model="discussText"
                   :placeholder="discussPersonToName" />
			</view>
		<view style="width: 50px;font-size: 11pt;color: #808080;" @click="discussSumbit">发布</view>
	</view>
</view>
        
inputFocus(e) {
	console.log(e)
	if(e.detail.height) {
		this.inputBottom = e.detail.height
	}
},
inputBlur() {
	this.inputBottom = 0
},
```

#### 问题 2 的出现与解决

问题 2 的问题就是点赞按钮本身就是至于评论行的div中的，所以会冒泡向上触发是很正常的事情。

这个click.stop是为了阻止事件的冒泡，hover-stop-propagation是为了阻止触发外层的点击样式。

```javascript
<view :hover-stop-propagation="true" @click.stop="clickGood(discuss.id,discuss.discussPerson,discuss)">
	<image :src="selectIsGood(discuss.id)" class="discuss-icon-small"></image>
</view>
```

## 其他

[Vue双向绑定模拟实现原理的代码](https://github.com/Fuyun791/vue-simulation) 另一个github项目，推荐看看

[谈Nginx部署两个vue项目](https://blog.csdn.net/weixin_43882435/article/details/104369127)

[vue使用element-ui动态生成表单高可复用性解决resetFields()带来的问题](https://blog.csdn.net/weixin_43882435/article/details/104247405)
