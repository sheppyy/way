# 1 当前页面是设置

## 1.1 list_display：显示

```
list_display = ['username', 'gender', ...]
```

- 该配置是设置在数据页面要显示的字段，显示的顺序和列表的顺序一致。



## 1.2 list_per_page：分页

```
list_per_page = 30
```

- 设置每页的记录条数，默认是100，超出后自动分页



## 1.3 ordering：排序

```
ordering = ['name', 'add_time', ...]
```

- 根据设置的字段排序
- 默认根据id，降序排序
- ordering = ['name']升序，ordering = ['-name']降序



## 1.4 list_editable：可编辑

```
list_editable = ['name', 'age', ...]
```

- 在页面可以直接编辑的字段
- 可编辑的字段，需要在显示的字段里。（都看不见，怎么编辑:cowboy_hat_face:）



## 1.5 list_filter：筛选

```
list_filter = ['gender', ...]
```

- 设置根据那些字段进行筛选数据
- 如果字段是一个时间日期类型或日期类型，则成了一个时间段筛选器



## 1.6 search_fields：搜索

```
search_fields = ['name', 'address', ...]
```

- 在顶部重写一个搜索框，可以搜索字段设置的数据



## 1.7 date_hierarchy：日期等级划分

```
date_hierarchy = 'join_time'
```

- 相当于一个快速的日期筛选



## 1.8 自定义日期显示格式

```
USE_L10N = False
DATETIME_FORMAT = 'Y-m-d H:i:s'
DATE_FORMAT = 'Y-m-d'

说明：
	1. 修改后的格式：2020-08-12 16:42:10
```

- 默认是2020年8月15日





## 1.9 进入详情页



默认是点击第一个字段进入详情页的，可以自定义进入详情页的字段

```
list_display_links = ['id', 'name', ...]
```







# 2 站点设置



## 2.1 设置站点的标题和页签上面的文字

```
admin.site.site_header = 'SSS'
```

- 把默认的”Django 管理“改成'SSS'



```
admin.site.site_title = 'SSS'
```

- 把页签上面的”Django 站点管理员“改成”SSS“



2 





# 3 actions设置



## 3.1 屏蔽默认的按钮



```
class UserAdmin(admin.ModelAdmin):
    list_display = User.keys()
    
    # 在对应的Admin模型增加权限函数屏蔽即可；这里屏蔽了add按钮
    def has_add_permission(self, request):
        return False
```







```

```





```

```



```

```



















