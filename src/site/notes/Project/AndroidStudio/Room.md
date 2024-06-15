---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Room/","dgPassFrontmatter":true}
---

Room是一个用来持久性保存数据的库
Room包含三个组件：
1. Room实体：应用数据库中的表，可以使用它们更新表中的行所存储的数据，以及创建要插入的新行
2. Room DAO：提供了供应用在数据库中检索、更新、插入和删除数据的方法
3. Room DataStore类：一个数据库类，为应用提供与该数据库关联的DAO实例
# 使用
## 创建item实体
Entity类定义表，此表的每一个实例都表示数据库中的一行
![Pasted image 20240612154418.png](/img/user/Project/AndroidStudio/%E5%9B%BE%E7%89%87/Pasted%20image%2020240612154418.png)

# REF:
1. https://developer.android.com/codelabs/basic-android-kotlin-compose-persisting-data-room?hl=zh-cn#0