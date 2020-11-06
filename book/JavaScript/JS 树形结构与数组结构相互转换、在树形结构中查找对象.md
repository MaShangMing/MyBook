总是有很多需求是关于处理树形结构的，所以不得不总结几个常见操作的写法。¯\_(ツ)_/¯

首先假设有一个树形结构数据如下

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
var tree=[
  {
    'id': '1',
    'name': '教学素材管理',
    'children':[
      {
        'id': '101',
        'name': '教学素材',
        'children':[
          {
            'id': '10101',
            'name': '修改',
          },
          {
            'id': '10102',
            'name': '添加',
          }
        ]
      },  {
        'id': '102',
        'name': '测试试题',
      },
      {
        'id': '103',
        'name': '问题任务',
      }
    ]
  }, {
    'id': '2',
    'name': '基础数据管理',
    'children':[
      {
        'id': '201',
        'name': '专业设置',
      },
      {
        'id': '202',
        'name': '专业管理',
      }
    ]
  }
]
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

**1、如何在tree中找到id=10102的对象？**

思路一：深度遍历，从顶点开始，当前节点有子节点则遍历当前节点的子节点（递归）。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
function deepQuery(tree,id) {
    var isGet = false;
    var retNode = null;
    function deepSearch(tree,id){
        for(var i = 0; i<tree.length; i++) {
            if(tree[i].children && tree[i].children.length>0) {
                deepSearch(tree[i].children,id);
            }
            if(id === tree[i].id || isGet) {
                isGet||(retNode = tree[i]);
                isGet = true;
                break;
            }
        }
    }
    deepSearch(tree,id);
    return retNode;
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
var getNode = deepQuery(tree,10102);
console.log(getNode)
```

思路二：广度遍历，遍历根节点的所有子节点，再从第一个子节点开始依次遍历。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
function breadthQuery(tree, id) {
    var stark = [];
    stark = stark.concat(tree);
    while(stark.length) {
        var temp = stark.shift();
        if(temp.children) {
            stark = stark.concat(temp.children);
        }
        if(temp.id === id) {
            return temp;
        }
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
var getNode=breadthQuery(tree,10102);
console.log(getNode);
```

**2、如何将树形结构转换为有父子关系属性的数组结构？**

思路一：初始化一个空数组，从tree的顶端开始遍历，当前节点有子节点时，一边继续遍历子节点，一边在当前节点上删除子节点，将当前节点push到空数组。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
function flatten1(tree) {
    var arr = [];
    function spread(tree,pid) {
        for (var i=0; i < tree.length; i++ ) {
            item = tree[i]
            let {id,name}=item;
            arr.push({id,name,pid})
            if (item.children) {
                spread(item.children,item.id)
                delete item.children
            }
        }
    }
    spread(tree,0)
    return arr;
}
var newArr = flatten1(tree);
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

思路二：

```
function flatten2 (data,pid) {
  return data.reduce((arr, {id, name, children = []}) =>
    arr.concat([{id, name,pid}], flatten2(children,id)), [])
} 
var newArr = flatten2(tree,0);
```

结果：

![img](https://img2018.cnblogs.com/blog/1663175/201909/1663175-20190919102852065-10871294.png)

**3、如何将数组结构转换为树形结构？**

下面是偶然看到一位大佬很秀的写法（读书人的事怎么能叫抄呢ヽ(ﾟ∀ﾟ)ﾒ(ﾟ∀ﾟ)ﾉ 原文[链接](https://blog.csdn.net/Mr_JavaScript/article/details/82817177) ）

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
function treeData(data){   
    let cloneData = JSON.parse(JSON.stringify(data))
    return cloneData.filter(parent=>{
        let branchArr = cloneData.filter(child => parent['id'] == child['pid']);
        branchArr.length>0 ? parent['children'] = branchArr : '';
        return parent['pid'] == 0 ;
    })
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
var newTree = treeData(newArr)
```

用之前测试生成的数组试一下

![img](https://img2018.cnblogs.com/blog/1663175/201910/1663175-20191011160108305-621249465.png)

 结果如上，完美的生成了最初的树形结构。