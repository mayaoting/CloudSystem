# CloudSystem
@Cloud云存储系统
### 系统要求
##### 任务一：<br>
* 根据要求完成系统体系结构图和系统ER图，
* 进行技术选型给出相应的前端、后端、数据库等技术框架的选择 <br>
##### 任务二：<br>
* 文件的分片上传，断点续传 <br>
    当用户上传文件时对文件先进行分片后再一次上传，当上传过程中因网络、人为、等原因造成传输中断，待传输恢复后，能够继续上传。 <br>
##### 任务三： <br>
* 文件的下载和预览 <br>
    用户能够下载文件，并对图片、txt、word、ppt、pdf等文件进行预览。
##### 任务四： <br>
* 云文件的管理 <br>
    用户在云存储系统中，能够实现删除文件、移动文件、重命名文件、新建文件夹、删除文件夹、重命名文件等操作。
##### 任务五： <br>
* 云存储系统的后台管理系统和个人中心
    实现云存储的的用户管理（包括用户的权限、能否使用、磁盘容量）；
    用户个人中心（个人信息的修改、云存储系统的展示）。
##### 任务六：
* 分布式文件系统的搭建
    基于NFS技术搭建出集群化的文件系统、作为云存储的文件系统。
##### 附加功能：
    好友管理（加好友，关注）
    文件分享，视频，音频在线播放
    分享圈点赞，关注、预览、下载
    在线好友聊天
    批量操作文件
    通知管理
    管理员查看分布式文件系统状态

#### 前后端框架技术选择
###### 前端使用的技术与框架：
    Vue.js 、ElementUI、JQuery 、 IView 、 Pdf.js、 QQFace 、 
    数据可视化： Echarts
###### 后端使用的技术与框架：
    Spring boot 、Mybatis-plus、Nginx、 WebUploader、 Aspose、 JWT 、FastDFS、
    会话：WebSocket
    安全框架：Spring Security
    数据库：MySqL Redis
    测试工具：postman
###### 注：该系统前后端分离
### 项目截图
系统流程图：
![系统流程图](https://github.com/mayaoting/CloudSystem/blob/master/static/SystemInterfacesImages/新流程图.png)
@cloud主页截图：
![主页截图](https://github.com/mayaoting/CloudSystem/blob/master/static/SystemInterfacesImages/@cloud主页截图.png)
系统会话页面：
![系统会话页面](https://github.com/mayaoting/CloudSystem/blob/master/static/SystemInterfacesImages/系统会话页面.png)
后台管理系统界面：
![后台管理系统部分界面图](https://github.com/mayaoting/CloudSystem/blob/master/static/SystemInterfacesImages/后台管理系统页面.png)