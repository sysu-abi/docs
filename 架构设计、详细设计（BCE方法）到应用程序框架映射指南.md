# 1.逻辑架构

逻辑架构由三层模型（表示层、业务层、持久化层）构成

## 1.1表示层

客户端使用安卓APP作为表示层，提供发布任务子系统、接取任务子系统、个人信息子系统、提现充值子系统。

## 1.2业务层

服务器充当业务层的角色，为表示层的各个子系统提供相应的服务模块。

## 1.3持久化层

MySQL提供数据的持久化服务。

# 架构目录设计

## 安卓app

```
├── src
     ├── androidTest                                                  //Android Test测试用例
     ├── main
     |    ├── java                                                    //java代码
     |    |    ├── com.sysu.ceres
     |    |    |    ├── activity
     |    |    |    |    |   CreateMessageActivity.java               //创建留言
     |    |    |    |    |   CreatSurveyActivity.java                 //创建调查问卷
     |    |    |    |    |   DoSurveyActivity.java                    //完成问卷
     |    |    |    |    |   EditTaskActivity.java                    //编辑任务
     |    |    |    |    |   LoginActivity.java                       //注册登录
     |    |    |    |    |   MainActivity.java                        //程序入口
     |    |    |    |    |   MyTaskActivity.java                      //我的任务
     |    |    |    |    |   StatisticListActivity.java               //获取问卷数据信息
     |    |    |    |    |   TaskDetailActivity.java                  //任务详情
     |    |    |    ├── adapter
     |    |    |    |    |   MyMessageRecyclerViewAdapter.java        //留言列表适配器
     |    |    |    |    |   MyStatisticsRecyclerViewAdapter.java     //问卷数据信息适配器
     |    |    |    |    |   MyTaskRecyclerViewAdapter.java           //任务列表适配器
     |    |    |    |    |   SectionsPagerAdapter.java                //ViewPaper控件适配器
     |    |    |    ├── fragment
     |    |    |    |    |   MessageFragment.java                     //留言碎片
     |    |    |    |    |   MineFragment.java                        //“我的”碎片
     |    |    |    |    |   StatisticsFragment.java                  //问卷数据信息碎片
     |    |    |    |    |   TaskDetailFragment.java                  //任务详情碎片
     |    |    |    |    |   TaskListFragment.java                    //任务列表碎片
     |    |    |    ├── http
     |    |    |    |    |   Api.java
     |    |    |    |    |   ApiMethods.java                          //API函数
     |    |    |    |    |   ApiService.java                          //API服务
     |    |    |    ├── model
     |    |    |    |    |   Message.java                             //留言实体类
     |    |    |    |    |   MessageList.java                         //留言列表类
     |    |    |    |    |   QuestionList.java                        //问题列表类
     |    |    |    |    |   Statistic.java                           //问卷数据信息类
     |    |    |    |    |   StatisticList.java                       //问卷问题选项列表
     |    |    |    |    |   Status.java                              //状态类
     |    |    |    |    |   Survey.java                              //问卷实体类
     |    |    |    |    |   SurveyFull.java                          //问题填充问卷类
     |    |    |    |    |   SurveyList.java                          //问卷列表类
     |    |    |    |    |   Task.java                                //任务实体类
     |    |    |    |    |   TaskList.java                            //任务列表类
     |    |    |    |    |   User.java                                //用户实体类
     |    |    |    |    |   UserList.java                            //用户列表类
     |    |    |    ├── observer
     |    |    |    |    |   MyObserver.java                          //定义观察者接口
     |    |    |    |    |   ObserverOnNextListener.java              //监听接口 
     |    |    |    ├── utils
     |    |    |    |    |   MyMD5Util.java                           //md5算法加密密码
     |    |    |    |    CeresConfig.java                             //配置文件
     |    ├── res                                                     //资源
     |    |    ├── drawable                                           //放置图片
     |    |    ├── drawable-v24                                       //放置图片
     |    |    ├── layout                                             //放置布局文件
     |    |    |    |   activity_creat_survey.xml                     //创建调查问卷界面
     |    |    |    |   activity_create_message.xml                   //创建留言界面
     |    |    |    |   activity_do_survey.xml                        //完成调查问卷界面
     |    |    |    |   activity_edit_task.xml                        //编辑任务界面
     |    |    |    |   activity_edit_user.xml                        //编辑用户信息界面
     |    |    |    |   activity_login.xml                            //注册登录界面
     |    |    |    |   activity_main.xml                             //主界面
     |    |    |    |   activity_my_task.xml                          //我的任务界面
     |    |    |    |   activity_statistic_list.xml                   //数据信息列表界面
     |    |    |    |   activity_task_detail.xml                      //任务详情界面
     |    |    |    |   fragment_message_list.xml                     //留言列表
     |    |    |    |   fragment_message_list_item.xml                //留言条目
     |    |    |    |   fragment_mine.xml                             //“我的”界面
     |    |    |    |   fragment_statistics_item.xml                  //问卷数据信息条目
     |    |    |    |   fragment_statistics_list.xml                  //问卷数据信息列表
     |    |    |    |   fragment_task_detail.xml                      //任务详情
     |    |    |    |   fragment_task_list.xml                        //任务列表
     |    |    |    |   fragment_task_list_item.xml                   //任务条目
     |    |    ├── menu                                               //菜单按钮文件
     |    |    |    |   bottom_nav_menu.xml                           //底部导航栏 
     |    |    ├── mipmap-anydpi-v26                                  //放置应用图标
     |    |    ├── mipmap-hdpi                                        //放置应用图标，高分辨率
     |    |    ├── mipmap-mdpi                                        //放置应用图标，
     |    |    ├── mipmap-xhdpi                                       //放置应用图标
     |    |    ├── mipmap-xxhdpi                                      //放置应用图标
     |    |    ├── mipmap-xxxhdpi                                     //放置应用图标
     |    |    ├── values                                             //放字符串、样式、颜色等配置
     |    |    |    |   colors.xml                                    //颜色
     |    |    |    |   dimens.xml                                    //自动生成Android屏幕适配
     |    |    |    |   strings.xml                                   //字符串
     |    |    |    |   styles.xml                                    //样式
     |    |    ├── values-w820dp 
     |    |    |    |   dimens.xml                                    //自动生成Android屏幕适配
     |    |    ├── xml 
     |    |    |    |   network_security_config.xml                   //网络安全配置文件
     |    | AndroidManifest.xml                                       //配置文件
     ├── test                                                         //Unit Test测试用例
     |   .gitignore                                                   //忽略指定文件
     |   build.gradle                                                 //app模块的gradle构建脚本
     |   proguard-rules.pro                                           //指定项目代码的混淆规则
   
```

## 服务器

```
├── src                                     				   
     ├── com.c72                    							
     |    |    AnswerStatistics.java         
     |    |    AnswerStatisticsMapper.java		
     |    |    Message.java
     |    |    MessageMapper.java
     |    |    Question.java
     |    |    QuestionMapper.java
     |    |    Survey.java
     |    |    SurveyMapper.java
     |    |    Task.java
     |    |    TaskController.java
     |    |    TaskDAO.java
     |    |    TaskDAOImpl.java
     |    |    TaskMapper.java
     |    |    User.java
     |    |    UserJoins.java
     |    |    UserJoinsMapper.java
     |    |    UserMapper.java
     |    |    mySpring.xml

```

# ECB

1. Boundary 对象：表示参与者与系统之间进行的交互以及信息交流

2. Controller 对象：一个用例具有的事件流的控制行为

3. Entity 对象：表示数据库中存储的信息及相关行为

## ECB中：

-    Entity：代表系统数据，如：客户、订单、任务等

-    Boundary：与用户的接口，如：界面、网关、代理等

-    Controller：连接 Boundary 和 Entity 的媒介，编排来自 Boundary 的命令的执行


1. Entity 包含： 服务器框架目录设计中，model 目录下定义了所有服务器与 MySQL 相关的实体，承载系统数据
2. Controller 包含： 服务器框架目录设计中，controller 目录下定义了所有的相关内容，它们接收来自上述 Boundary 的命令，并调用 service 的对象来获取或更新模型或其他请求。 服务器框架目录设计中，service 目录下定义了部分相关内容，封装可能在控制器中的所有业务逻辑。这样，控制器就在那里转发和控制执行。
3. Boundary有三个部分： 管理员端 Web 程序用户界面 客户端小程序用户界面 服务端 Nginx 反向代理服务器

ECB是框架目录设计与逻辑架构的一种具体方法。