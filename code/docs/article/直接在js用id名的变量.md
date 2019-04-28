---
classify: JS及jQuery
tags: [JavaScript, 技巧]
---

## 直接在 js 用 id 名的变量

### 背景
刚刚在写一个创建表格的函数，用于在指定 id 名的 div 下创建一个表格，然后函数参数为 div 的 id 名，一开始写的时候用

```
const table1 = document.getElementById('table1');
```

没问题，然后再整理成函数时发现把 id 名传入时，id 名并不是一个字符串，而是这个 id 对应的 dom 节点，

```
<div id="table1">
    </div>
```

我猜测是传入的 id 名并不是常量，而是变量，但是我并没有定义它，而后问了下朋友，告诉我
**你可以在全局变量中获得以 id 值为命名的变量，无需自己定义**
这就清楚了，以前一直用的 document.getElementById，然后我还在想怎样把这个变量变成字符串，然后一想，没必要啊，直接在函数里定义个变量，把传入的 id 名复制给他就可以了。

```
        let th = [
            '姓名',
            '年龄'
        ];
        let td = [
            {
                name: '张三',
                age: '18'
            },
            {
                name: '李四',
                age: '22'
            }
        ];
        const createdTable = (idname, th, td) => {
            //const table1 = document.getElementById('table1');这句话可以不用了，直接用下面的
            const table1 = idname;
            let thPart = '';
            let tdPart = '';
            th.forEach(v => {
                thPart += ` <th>${v}</th>`;
            });
            td.forEach(v => {
                tdPart +=
                    `<tr>
                        <td> ${v.name} </td>
                        <td> ${v.age} </td>
                    </tr>
                    `
            });
          //应该有thead和tbody，但是懒了
            let tablePart =
                `
                <table >
                    <tr>
                    ${thPart}
                    </tr>
                    ${tdPart}
                </table>
                    `;

            table1.innerHTML = tablePart;
        };
        createdTable(table1,th,td);//表格所在div的id名，表头数据，表体数据
```
