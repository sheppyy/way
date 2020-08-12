# 1 当前页面是设置

## 1 list_display：显示

```
list_display = ['username', 'gender', ...]
```

- 该配置是设置在数据页面要显示的字段，显示的顺序和列表的顺序一致。



## 2 list_per_page：分页

```
list_per_page = 30
```

- 设置每页的记录条数，默认是100，超出后自动分页



## 3 ordering：排序

```
ordering = ['name', 'add_time', ...]
```

- 根据设置的字段排序
- 默认根据id，降序排序
- ordering = ['name']升序，ordering = ['-name']降序



## 4 list_editable：可编辑

```
list_editable = ['name', 'age', ...]
```

- 在页面可以直接编辑的字段
- 可编辑的字段，需要在显示的字段里。（都看不见，怎么编辑:cowboy_hat_face:）



## 5 list_filter：筛选

```
list_filter = ['gender', ...]
```

- 设置根据那些字段进行筛选数据
- 如果字段是一个时间日期类型或日期类型，则成了一个时间段筛选器



## 6 search_fields：搜索

```
search_fields = ['name', 'address', ...]
```

- 在顶部重写一个搜索框，可以搜索字段设置的数据



## 7 date_hierarchy：日期等级划分

```
date_hierarchy = 'join_time'
```

- 相当于一个快速的日期筛选



# 2 站点设置

## 1 设置站点的标题和页签上面的文字

```
admin.site.site_header = 'SSS'
```

- 把默认的”Django 管理“改成'SSS'



```
admin.site.site_title = 'SSS'
```

- 把页签上面的”Django 站点管理员“改成”SSS“



2 





