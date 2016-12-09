#bootstrap
> * dataTables使用
> * bootstapValidate 验证
> * tooltip和sco扩展

##dataTables使用
参考文章：

[bootstrap中使用datatables](https://my.oschina.net/tongjh/blog/213932)

实际使用例子
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




