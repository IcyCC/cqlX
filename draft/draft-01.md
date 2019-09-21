
# 使用模式

1. 根据数据库信息 生产基本表结构文件 cqlx_base_db.h

```
cqlx.gen_base -h 0.0.0.0 -p password  -db tests --driver mysql 
```

产生的头文件内容类似

```c++

class UserMetaBase : public TableMeta {
    public:
        inline static Filed id(name= "id", type="INT", primary=true, not_null=true);
        inline static Filed name(name= "name", type='VarChar(255)', index='idx_name(50)')
    public:
        .....
    protect:
        virtual void serilizer();
    pirvate:
        std::map<std::string, ValueBase> _data; //实例化的子放在这里
}

```

2. 业务逻辑中继承并重置

```c++
class UserDao: public UserMetaBase {

}

auto user1 = UserDao(id=1, name="2")
user1.Save(ctx=ctx) // ctx 表示事务

user1.name = "123";
user1.Save()

auto user_query1 = UserDao.Query.Filter(UserDao.id > 1)
users = user_query.FetchAll() // 此处接口参考 sqlalchemy

```

3. 重写Base方法

```c++
class UserDao: public UserMetaBase {

    virtual void serilizer() {
        // 可以定义字段名字和序列化方式
    }

    virtual void tableHook (){
        // 可以
    }

}
```

