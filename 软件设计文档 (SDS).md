# 软件设计文档 

注：挣闲钱同时也是该小组成员在 2019 年春季学期中山大学系统分析与设计课程的大作业。按照本课程作业要求在 SD 文档开头点明。

1.[Client](#1-client)
- [技术选型及理由](#11-技术选型及理由)
- [架构设计](#12-架构设计)
- [模块划分](#13-模块划分)
- [软件设计技术](#14-软件设计技术) 

2.[Server](#2-Server)
- [技术选型及理由](#21-技术选型及理由)
- [架构设计](#22-架构设计)
- [模块划分](#23-模块划分)
- [软件设计技术](#24-软件设计技术)

## 1. Client


#### 1.1 技术选型及理由

**安卓端app**

- 使用Android Studio进行开发，开发过程简洁，Android Studio功能强大，开发者体验较好
- 项目部署安装过程简洁，只需用户下载apk文件安装
- 安卓用户群体庞大，发展前景非常好

**OkHttp**

- 支持HTTP/2，允许所有同一个主机地址的请求共享同一个socket连接
- 连接池减少请求延时
- 透明的GZIP压缩减少响应数据的大小
- 缓存响应内容，避免一些完全重复的请求

**RxJava**

- 简化异步程序的流程
- 使用近似于Java8的流的操作进行编程

**Retrofit2**

- 其通过动态代理的方式将一个基本的Java接口翻译成一个HTTP请求，并通过OkHttp去发送请求
- 具有强大的可扩展性，支持各种格式转换以及RxJava

#### 1.2 架构设计

**项目目录结构：**

![在这里插入图片描述](https://github.com/sysu-abi/image/blob/master/%E8%BD%AF%E7%BB%BC%E8%AE%BE%E8%AE%A1%E6%96%87%E6%A1%A3-%E5%89%8D%E7%AB%AF%E9%A1%B9%E7%9B%AE%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.PNG)

**其中src目录具体结构如下：**

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



#### 1.3 模块划分

根据业务逻辑和UI设计，可以将前端分为以下的**主要模块**：

- **注册登录模块：** 用户输入用户名和密码进行注册登录

- **用户信息模块：** 用户个人信息显示

- **编辑用户信息模块：** 实现用户个人信息的修改

- **任务列表模块：** 用户获取当前可接受任务的列表

- **发布任务模块：** 用户发布任务，填写相关标题、任务描述、报酬、任务类型、参与总人数、以及截止时间等信息，可发布问卷等类型任务

- **接受任务列表模块：** 用户接受任务列表

- **发布任务列表模块：** 用户发布任务列表

- **编辑任务模块：** 用户编辑更改任务

- **任务详情模块：** 任务的详细信息

- **创建留言模块：** 用户为任务创建留言

- **留言列表模块：** 任务的留言列表

- **创建调查问卷模块：** 用户创建问卷，填写问题，点击按钮添加下一个问题或完成提交问卷

- **填写问卷模块：** 接受任务的用户进行问卷填写

  

另外，还定义了三个组件，构成**组件模块**：

- **ViewPaper组件：** 多界面切换组件，实现横向滑动切换页面
- **navbar组件：** 底部导航栏组件
- **timepicker组件：** 时间选择器

除此之外，资源文件构成**资源模块**，**API模块**用于请求数据。

**各模块间关系如下：**

![在这里插入图片描述](https://github.com/sysu-abi/image/blob/master/%E8%BD%AF%E7%BB%BC%E8%AE%BE%E8%AE%A1%E6%96%87%E6%A1%A3-%E6%A8%A1%E5%9D%97%E9%97%B4%E5%85%B3%E7%B3%BB.PNG)

#### 1.4 软件设计技术

**面向对象编程**

从上面的1.2中文件结构可以看到，数据库中的表对应的实体被封装成类，实现面向对象，在逻辑层代码进行操作时只需调用类中函数即可，model的具体结构如下：

![在这里插入图片描述](https://github.com/sysu-abi/image/blob/master/%E8%BD%AF%E7%BB%BC%E8%AE%BE%E8%AE%A1%E6%96%87%E6%A1%A3-%E5%89%8D%E7%AB%AFmodel%E7%BB%93%E6%9E%84.PNG)

**md5算法加密密码**

![在这里插入图片描述](https://github.com/sysu-abi/image/blob/master/%E8%BD%AF%E7%BB%BC%E8%AE%BE%E8%AE%A1%E6%96%87%E6%A1%A3-md5.PNG)


## 2. Server

#### 2.1 技术选型及理由

**Spring框架**

Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器框架。它是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理bean的方式。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190627185657471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NIRU5YSTEyMTM4,size_16,color_FFFFFF,t_70)

**优点：**

- JAVA EE更加容易使用
- 面向对象的设计比任何实现技术都重要
- Spring将使用接口的复杂度降低到零
- 代码易于测试

**MySql**

MySql是一种关系数据库管理系统，关系数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，因此提升了速度和灵活性。由于其体积小、速度快、总体拥有成本低，尤其是开放源码这一特点，Mysql很适合用于作为小型网站开发的数据库。



#### 2.2 架构设计

整体是 **Client-Server(CS)** 架构，如下图所示：

![在这里插入图片描述](https://github.com/sysu-abi/image/blob/master/%E6%9E%B6%E6%9E%84%E8%AE%BE%E8%AE%A1-%E7%89%A9%E7%90%86%E8%A7%86%E5%9B%BE.PNG)

#### 2.3 模块划分

由于使用的是**MVC**框架，因此模块可划分为**Model-View-Controller**三部分。后端涵盖的模块分别是**Model**和**Controller**。

**其中src目录具体结构如下：**

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

**TaskController.java**文件对应Controller，它负责与客户端交互。通过接收来自View的Http POST请求，它会调用特定的Model模块，并将获取的数据添入JSON返回给View。

**TaskDAOImpl.java**文件对应Model，它负责数据库的交互。当需要某些对象的数据时，它会调用相应的SQL语句来获取数据，并且通过RowMapper将每一行数据封装成所需要的类。

**具体的类包括：**

- **User：** 表示用户实体的类。具有uid,name,phone,email，password这些基本属性，以及money和credit这两个附加属性。用户可发布或接受任务。</br>
  **UserMapper**是User对RowMapper接口的实现。
- **Task：** 表示任务实体的类。具有标题、内容、酬劳等属性。部分任务与调查问卷关联。</br>
  **TaskMapper**是Task对RowMapper接口的实现。
- **Message：** 表示留言实体的类。具有内容、所属楼层等属性。与任务和用户相关联。</br>
  **MessageMapper**是Message对RowMapper接口的实现。
- **Survey：** 表示问卷实体的类。与任务相关联。</br>
  **SurveyMapper**是Survey对RowMapper接口的实现。
- **Question：** 表示问题实体的类。具有内容、所属楼层等属性。与调查问卷相关联。</br>
  **QuestionMapper**是Question对RowMapper接口的实现。
- **AnswerStatistics：** 表示问题统计结果的类。记录了问卷id，问题id，和各项答案的统计数。</br>
  **AnswerStatisticsMapper**是AnswerStatistics对RowMapper接口的实现。
- **Session：** 记录用户的登录，用于核对用户信息。其属性包括cookie和uid。</br>
  **SessionMapper**是Session对RowMapper接口的实现。
- **UserJoin：** 记录用户参与的任务。</br>
  **UserJoinMapper**是UserJoin对RowMapper接口的实现。



#### 2.4 软件设计技术

**面向对象编程**

在编写后端代码时，我们将数据库中的每一张表都封装为一个类，并通过TaskDAOImpl类封装对数据库的所有操作。而在TaskController中，通过调用TaskDAOImpl中的函数可实现对数据库的操作。当需要添加新的表时，只需加入表的封装类、该类对RowMapper的实现、以及相应的增删改SQL语句即可。

TaskDAOImpl的结构包括:

```java
public void setDataSource(DataSource ds);

 

//User

public void creatUser(String name, String phone,  String email, String password);

public User getUserbyUid(Integer uid);

public User getUserbyName(String name);

public void updateUser(int uid, String name, String phone, String email, String password, int money, int credit);

public void deleteUser(Integer uid);

 

//TASK

public void creatTask(int uid,String title,String detail,int money,String type,int total_num,Timestamp end_time,String state);

public Task getTask(Integer tid);

public List<Task> getpublishTask(Integer uid);

public void updateTask(int tid,String title,String detail,int money,String type,int total_num,int current_num,Timestamp start_time,Timestamp end_time,String state);

public void deleteTask(Integer tid);

public List<Task> listTasks(String extra);

public List<Task> listTasksbyDLL(String mode, String extra);

public List<Task> listTasksbyMoney(String mode, String extra);

public List<Task> listTasksbyStartTime(String mode, String extra);

public boolean isValid(Task task);

public List<Task> removeInvalid(List<Task> tasks);

 

//MESSAGE

public void createMessage(int tid, int uid, int rank, String detail);

public Message getMessage(int tid, int rank);

public void deleteMessage(Integer tid, int rank);

public List<Message> listMessages(Integer tid);

 

//USERJOINS

public void creatUserJoins(int tid, int uid, Timestamp time);

public UserJoins getUserJoins(Integer tid, int uid);

public void deleteUserJoinsbyTid(Integer tid);

public void deleteUserJoinsbyUid(int tid, int uid);

public List<UserJoins> listUserJoinsbyTid(Integer tid);

public List<UserJoins> listUserJoinsbyUid(int uid);

 

//Survey

public void createSurvey(int tid);

public List<Survey> getSurveybyTid(int tid);

public Survey getSurveybySid(int sid);

public void deleteSurvey(int sid);

 

//Question

public void createQuestion(int sid,int qid,String qtype,String qtitle,String answer_a,String answer_b,String answer_c,String answer_d);

public List<Question> getQuestionbySid(int sid);

public Question getQuestionbyId(int sid,int qid);

public void deleteQuestion(int sid);

 

//AnswerStatistics

public void createAnswerStatistics(int sid,int qid);

public List<AnswerStatistics> getStatisticsbySid(int sid);

public AnswerStatistics getAnswerStatisticsbyID(int sid,int qid);

public void updateAnswerStatistics(int sid,int qid,int a,int b,int c,int d);

public void deleteAnswerStatistics(int sid);

 

//Session

public void createSession(int uid);

public Session getSessionbyCookie(int cookie);

public Session getSessionbyUid(int uid);

public void deleteSession(int uid);
```



**基于Spring框架的设计**

Spring提供的RowMapper可以将数据中的每一行封装成用户定义的类，在数据库查询中，如果返回的类型是用户自定义的类型则需要包装，通过使用实现RowMapper接口的类，程序能准确获得数据库的数据。

书写Spring的配置文件时，通过在相应的xml中声明bean来管理Spring容器管理。
