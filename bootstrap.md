#bootstrap
> * dataTables使用
> * bootstapValidate 验证
> * tooltip和sco扩展

##dataTables使用
参考文章：

[datables中文网](http://datatables.club/)

[bootstrap中使用datatables](https://my.oschina.net/tongjh/blog/213932)

[datables遍历方法](http://blog.csdn.net/zhangchuccc/article/details/5648533)

[jquery datatables 学习笔记](http://www.cnblogs.com/178mz/p/4383519.html)

相关参数(json格式，name对应实际使用的字符串，后面是字符串的值)：
```json
[{"name":"sEcho","value":1},            //请求次数
{"name":"iColumns","value":5},          //列数 
{"name":"sColumns","value":",,,,"},     //这个没查到
{"name":"iDisplayStart","value":0},     //记录起始，0表示第一条
{"name":"iDisplayLength","value":10},   //每页显示的记录数
{"name":"mDataProp_0","value":"key"},   //第一列名称 
{"name":"bSortable_0","value":false},   //第一列是否可以排序
{"name":"mDataProp_1","value":"rentRuleId"},//第二列名称
{"name":"bSortable_1","value":true},    //第二列是否可以排序  
{"name":"mDataProp_2","value":"ruleName"},
{"name":"bSortable_2","value":true},
{"name":"mDataProp_3","value":"isEnable"},
{"name":"bSortable_3","value":true},
{"name":"mDataProp_4","value":"id"},
{"name":"bSortable_4","value":true},
{"name":"iSortCol_0","value":1},//第一个排序，value值得是第几列 
{"name":"sSortDir_0","value":"asc"},//排序是正序还是倒叙
{"name":"iSortingCols","value":1}]  //一共有几列排序
```

前端分页ajax一次性读取所有数据使用例子
```js
//初始化dataTables

var oTable = $('#sample_editable_1').dataTable({
    "bProcessing" : true,
                "aLengthMenu": [
                    [5, 15, 20, -1],
                    [5, 15, 20, "All"] // change per page values here
                ],
                // set the initial value
    "aaSorting" : [[0, "desc"]],/*排序*/
                "iDisplayLength": 5,
                "sDom": "<'row-fluid'<'span6'l><'span6'f>r>t<'row-fluid'<'span6'i><'span6'p>>",
                "sPaginationType": "bootstrap",
                "oLanguage": {
     "sProcessing" : "正在获取数据，请稍后...",
                    "sLengthMenu": "每页显示_MENU_条记录",
      "sInfo": "从 _START_ 到 _END_ /共 _TOTAL_ 条数据",
                    "oPaginate": {
                        "sPrevious": "前一页",
                        "sNext": "后一页",
                    },
     "sZeroRecords": "抱歉， 没有找到",
     "sInfoEmpty": "没有数据"
                },
    
               "aoColumnDefs": [{
                        'bSortable': false,
                        'aTargets': [0]
                    }
                ]
            });

//读取数据的做法
 function getAllData(){
    //数据量不是特别大，直接全部读取吧
    $.getJSON("/admingetServerData",function(data,status){  
     if(status == "success"){
       $.each(data, function(i, item) {
         var aiNew = oTable.fnAddData([item.id, item.ip, item.name, item.other,
        '<a class="edit" href="">编辑</a>', '<a class="cancel" data-mode="new" href="">删除</a>'
        ]);
         var nRow = oTable.fnGetNodes(aiNew[0]);
         oTable.fnUpdate(item.id, nRow, 0, false);
         oTable.fnUpdate(item.ip, nRow, 1, false);
         oTable.fnUpdate(item.name, nRow, 2, false);
         oTable.fnUpdate(item.other, nRow, 3, false);
         oTable.fnUpdate('<a class="edit" href="">编辑</a>', nRow, 4, false);
         oTable.fnUpdate('<a class="delete" href="">删除</a>', nRow, 5, false);
        
        
       });
        oTable.fnDraw();
     }
                }); 
   }

```






