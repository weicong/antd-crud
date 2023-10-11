# Antd-CRUD

一个基于 React + Ant.Design 的增删改查组件。

![](./docs/assets/images/01.png)


### 开始使用

```tsx
function App() {

    const columns: ColumnsConfig<Account> = [
        {
            title: '姓名',
            dataIndex: 'name',
            key: 'name',
            placeholder:"请输入姓名",
            supportSearch:true,
            render: (text) => <a>{text}</a>,
        },
        {
            title: '年龄',
            dataIndex: 'age',
            key: 'age',
            supportSearch:true,
        },
        {
            title: '标题',
            dataIndex: 'address',
            key: 'address',
            supportSearch:true,
        },
        {
            title: '标签',
            key: 'tags',
            dataIndex: 'tags',
            supportSearch:true,
            render: (_, { tags }) => (
                <>
                    {tags.map((tag) => {
                        let color = tag.length > 5 ? 'geekblue' : 'green';
                        if (tag === 'loser') {
                            color = 'volcano';
                        }
                        return (
                            <Tag color={color} key={tag}>
                                {tag.toUpperCase()}
                            </Tag>
                        );
                    })}
                </>
            ),
        }
    ];


    const data: Account[] = [
        {
            key: '1',
            name: 'John Brown',
            age: 32,
            address: 'New York No. 1 Lake Park',
            tags: ['nice', 'developer'],
        },
        {
            key: '2',
            name: 'Jim Green',
            age: 42,
            address: 'London No. 1 Lake Park',
            tags: ['loser'],
        },
        {
            key: '3',
            name: 'Joe Black',
            age: 32,
            address: 'Sydney No. 1 Lake Park',
            tags: ['cool', 'teacher'],
        },
    ];

    const actions:Actions<Account> = {
        onCreate:(row)=>{
            console.log("onCreate",row.age);
        }
    };


    return (
        <div style={{width:"960px"}}>
            <AntdCrud<Account> columns={columns} dataSource={data} actions={actions} />
        </div>
    )
}
```

`ColumnConfig` 类型说明：

> `ColumnConfig` 继承了 Antd 的 Table 的 Column 的所有配置，参考：https://ant-design.antgroup.com/components/table-cn#column

在此基础上，增加了自己的配置：

* **placeholder**: 搜索框和编辑页面的占位内容
* **supportSearch**: 是否支持搜素
* **form**: 编辑表单的 form 设置
* **dict**: form 的数据字典设置


`Actions` 类型说明：

> `Actions` 是用于定义 AntdCrud 组件的监听方法

`Actions`  定义的类型如下：

```ts
type Actions<T> = {
    //获取数据列表
    onFetchList?: (currentPage: number, pageSize: number, totalPage: number, searchParams: any, orderByKey: string, orderByType: "asc" | "desc") => void,

    //获取数据详情
    onFetchDetail?: (row: T) => T,

    //删除单条数据
    onDelete?: (row: T) => void,

    //批量删除数据
    onDeleteBatch?: (rows: T[]) => void,

    //数据更新
    onUpdate?: (row: T) => void,

    //数据创建
    onCreate?: (row: T) => void,
}
```
需要用户在 `Actions` 定义以上方法，用于对数据进行操作或查询。