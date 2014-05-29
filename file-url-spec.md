# 目录文件及URL命名规则

### 通用命名约定
* 所有文件/目录名一律以中划线分隔

### css背景图片命名
* 一般命名为复合命名：类别-名称，如icon-add, icon-search, bg-main, btn-close, img-separator等，尽量使用btn/icon/bg/img/logo等前缀开始；
* 如果一些图标属于一组的（如评分），可以单独创建一个目录存放，便于管理也便于css sprite自动化；

### css/js文件命名
* 公共代码一般按照应用级别一般有reset、base、bootstrap等
* 业务代码一般为“模块名-页面"，如accounts-manage, news-list之类，也可以按照模块分目录存放，便于自动合并

### url命名
* controller 一般用复数形式，如candidates/positions/accounts
* method 一般为动词，如list/create/edit/view/save/delete 或者为复合命名：名词+动词，如resume-upload/partial-save
