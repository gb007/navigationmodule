# NavigationModule模块使用

## 1.在工程的根目录build.gradle中添加

````

allprojects {
		repositories {
			...
			maven { url 'https://www.jitpack.io' }
		}
	}
	
````


## 2.在需要引用此类库模块的build.gradle中引入依赖

````

dependencies {

    //fragment主页module
    implementation 'com.daimajia.easing:library:2.0@aar'
    implementation 'com.daimajia.androidanimations:library:2.2@aar'
    implementation 'com.github.gb007:navigationmodule:1.0.1'
}

````


## 3.初始化配置信息

````

//------------------------------------NavigationBar初始化------------------------------

navigationBar.defaultSetting()  //恢复默认配置、可用于重绘导航栏
                .titleItems(tabText)      //  Tab文字集合  只传文字则只显示文字
                .normalIconItems(normalIcon)   //  Tab未选中图标集合
                .selectIconItems(selectIcon)   //  Tab选中图标集合
                .fragmentList(fragments)       //  fragment集合
                .fragmentManager(getSupportFragmentManager())
                .iconSize(20)     //Tab图标大小
                .tabTextSize(10)   //Tab文字大小
                .tabTextTop(2)     //Tab文字距Tab图标的距离
                .normalTextColor(Color.parseColor("#666666"))   //Tab未选中时字体颜色
                .selectTextColor(Color.parseColor("#333333"))   //Tab选中时字体颜色
                .scaleType(ImageView.ScaleType.CENTER_INSIDE)  //同 ImageView的ScaleType
                .navigationBackground(Color.parseColor("#80000000"))   //导航栏背景色
                .setOnTabClickListener(new EasyNavigationBar.OnTabClickListener() {
                    @Override
                    public boolean onTabSelectEvent(View view, int position) {
                        //Tab点击事件  return true 页面不会切换

                        return false;
                    }

                    @Override
                    public boolean onTabReSelectEvent(View view, int position) {
                        //Tab重复点击事件
                        return false;
                    }
                })
                .smoothScroll(false)  //点击Tab  Viewpager切换是否有动画
                .canScroll(true)    //Viewpager能否左右滑动
                .mode(EasyNavigationBar.NavigationMode.MODE_ADD)   //默认MODE_NORMAL 普通模式  //MODE_ADD 带加号模式
                .centerTextStr("发现")
                .centerImageRes(R.mipmap.add_image)
                .centerIconSize(36)    //中间加号图片的大小
                .centerLayoutHeight(100)   //包含加号的布局高度 背景透明  所以加号看起来突出一块
                .navigationHeight(60)  //导航栏高度
                .lineHeight(10)         //分割线高度  默认1px
                .lineColor(Color.parseColor("#ff0000"))
                .centerLayoutRule(EasyNavigationBar.RULE_BOTTOM) //RULE_CENTER 加号居中addLayoutHeight调节位置 EasyNavigationBar.RULE_BOTTOM 加号在导航栏靠下
                .centerLayoutBottomMargin(10)   //加号到底部的距离
                .hasPadding(true)    //true ViewPager布局在导航栏之上 false有重叠
                .hintPointLeft(-3)  //调节提示红点的位置hintPointLeft hintPointTop 
                .hintPointTop(-3)
                .hintPointSize(6)    //提示红点的大小
                .msgPointLeft(-10)  //调节数字消息的位置msgPointLeft msgPointTop 
                .msgPointTop(-10)
                .msgPointTextSize(9)  //数字消息中字体大小
                .msgPointSize(18)    //数字消息红色背景的大小
                .centerAlignBottom(true)  //加号是否同Tab文字底部对齐  RULE_BOTTOM时有效；
                .centerTextTopMargin(50)  //加号文字距离加号图片的距离
                .centerTextSize(15)      //加号文字大小
                .centerNormalTextColor(Color.parseColor("#ff0000"))    //加号文字未选中时字体颜色
                .centerSelectTextColor(Color.parseColor("#00ff00"))    //加号文字选中时字体颜色
                .setMsgPointColor(Color.BLUE) //数字消息、红点背景颜色
                .setMsgPointMoreRadius(5) //消息99+角度半径
                .setMsgPointMoreWidth(50)  //消息99+宽度
                .setMsgPointMoreHeight(40)  //消息99+高度
                .textSizeType(EasyNavigationBar.TextSizeType.TYPE_DP)  //字体单位 建议使用DP  可切换SP
                .setOnTabLoadListener(new EasyNavigationBar.OnTabLoadListener() { //Tab加载完毕回调
                    @Override
                    public void onTabLoadCompleteEvent() {
                        navigationBar.setMsgPointCount(0, 7);
                        navigationBar.setMsgPointCount(1, 109);
                        navigationBar.setHintPoint(4, true);
                    }
                })
                //.setupWithViewPager() ViewPager或ViewPager2
                .build();
                
//------------------------------------NavigationBar初始化------------------------------



//------------------------------------设置底部消息数量，消息红点提示------------------------------

            navigationBar.setMsgPointCount(0, 20)//设置消息位置，消息数量
            navigationBar.setMsgPointCount(1, 105)//设置消息位置，消息数量大于99，则显示99+
            navigationBar.setHintPoint(3, true)//设置消息红点


            navigationBar.clearAllHintPoint()//清除所有消息红点
            navigationBar.clearAllMsgPoint()//清除所有消息数量
            navigationBar.clearHintPoint(3)//清除指定位置消息红点
            navigationBar.clearMsgPoint(0)//清除指定位置消息数量
            
//------------------------------------设置底部消息数量，消息红点提示------------------------------


````


## 3.1 实用范例

````
private fun initNavigation() {

        //底部导航名称
        val tabText = arrayOf("首页", "发现", "消息","我的")

        //未选中icon
        val normalIcon =
            intArrayOf(R.mipmap.index, R.mipmap.find,R.mipmap.message,R.mipmap.me)

        //选中时icon
        val selectIcon =
            intArrayOf(R.mipmap.index1, R.mipmap.find1, R.mipmap.message1,R.mipmap.me1)

        //底部fragment
        var fragments = mutableListOf<Fragment>()

        fragments.add(AFragment())
        fragments.add(BFragment())
        fragments.add(CFragment())
        fragments.add(DFragment())

        navigationBar.titleItems(tabText)
            .normalIconItems(normalIcon)
            .selectIconItems(selectIcon)
            .centerImageRes(R.mipmap.add_image)
            .fragmentList(fragments)
            .fragmentManager(supportFragmentManager)
            .centerLayoutRule(EasyNavigationBar.RULE_CENTER)
            .setOnTabClickListener(object : EasyNavigationBar.OnTabClickListener {
                override fun onTabSelectEvent(view: View?, position: Int): Boolean {
                    if (position == 3) {
                        Toast.makeText(this@MainActivity, "请先登录", Toast.LENGTH_SHORT).show()
                        //return true则拦截事件、不进行页面切换
                        return true
                    }
                    return false
                }

                override fun onTabReSelectEvent(view: View?, position: Int): Boolean {
                    return false
                }
            })
            .setOnCenterTabClickListener { //弹出菜单
                showMunu()
                false
            }
            .mode(EasyNavigationBar.NavigationMode.MODE_ADD) //默认MODE_NORMAL 普通模式  //MODE_ADD 带加号模式
            .anim(Anim.ZoomIn)
            .build()

        navigationBar.setAddViewLayout(createAddView())//自己实现点击中间加号弹出页面
       
        
        Handler().postDelayed(Runnable {
            navigationBar.setMsgPointCount(0, 20)//设置消息位置，消息数量
            navigationBar.setMsgPointCount(1, 105)//设置消息位置，消息数量大于99，则显示99+
            navigationBar.setHintPoint(3, true)//设置消息红点
        }, 1000)


        Handler().postDelayed(Runnable {
            //navigationBar.clearAllHintPoint()//清除所有消息红点
            //navigationBar.clearAllMsgPoint()//清除所有消息数量
            navigationBar.clearHintPoint(3)//清除指定位置消息红点
            navigationBar.clearMsgPoint(0)//清除指定位置消息数量
        }, 3000)
        

    }

````