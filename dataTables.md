#dataTables使用
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

前端分页ajax一次性读取所有数据例子
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
后端分页例子

```js
  var oTable = $('#sample_editable_1').dataTable({
                "aLengthMenu": [
                    [5, 15, 20, -1],
                    [5, 15, 20, "All"] // change per page values here
                ],
                // set the initial value
    bFilter: false,    //去掉搜索框方法三：这种方法可以
     "sortable": true,      //是否启用排序
     "sortOrder": "desc",     //排序方式
    "aaSorting" : [[0, "desc"]],
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
    "bProcessing": true, //当datatable获取数据时候是否显示正在处理提示信息。
    "bServerSide": true, //服务端处理分页
    "sAjaxSource": "admingetServerData", //ajax请求地址
    
    "aoColumns": [   
     {"mDataProp":"id"},
     {"mDataProp":"ip"},
     {"mDataProp":"name"},
     {"mDataProp":"other"},
     {"mDataProp":"编辑"},
     {"mDataProp":"删除"},
             ],    
                "aoColumnDefs": [
     {
                        "aTargets": [0],
       "bSortable": true,
         "bSearchable": true,
                    },
     {
                        "aTargets": [1],
       "bSortable": false,
         "bSearchable": true,
                    },
     {
                        "aTargets": [2],
       "bSortable": false,
         "bSearchable": true,
                    },
     {
                        "aTargets": [3],
       "bSortable": false,
         "bSearchable": true,
                    },
     {
                        "aTargets": [4],
       "bSortable": false,
         "bSearchable": false,
      }
                    },
     {
      "aTargets": [5],
       "bSortable": false,
          "bSearchable": false,
     }
                ]
            });
```
服务器端golang
注意:如果js开启bServerSide选项，搜索分页功能全部依赖后端完成
```go
func (self *RouterService) handleGetAllServerDataByAdmin(webAdminMysql *mysql.DbWebAdmin) {
 self.router.GET("/admingetServerData", func(c *gin.Context) {

  bAuth := webAdminMysql.AuthUserIsLogin(c.ClientIP())
  if bAuth {
   //后端处理客户端要求的分页
   iDisplayStart, _ := strconv.Atoi(c.Query("iDisplayStart"))   //记录开始数
   iDisplayLength, _ := strconv.Atoi(c.Query("iDisplayLength")) //每页显示记录数
   iSortCol_0 := c.Query("iSortCol_0")                          //按第几列排序
   sSortDir_0 := c.Query("sSortDir_0")                          //正序还是逆序
   sSearch := c.Query("sSearch")                                //搜索
   fmt.Printf("iDisplayStart=%v,iDisplayLength=%v\n", iDisplayStart, iDisplayLength)
   fmt.Printf("iSortCol_0=%v,sSortDir_0=%v\n", iSortCol_0, sSortDir_0)
   fmt.Printf("sSearch=%v\n", sSearch)
   s2wSlice := webAdminMysql.GetAllServerCfg()
   //先排序
   nSortCol, _ := strconv.Atoi(iSortCol_0)
   if nSortCol == 0 {
    switch {
    case sSortDir_0 == "desc":
     common.SortDesc(&s2wSlice)
    case sSortDir_0 == "asc":
     common.SortAsc(&s2wSlice)
    default:
     routerlog.Errorf("errValue sSortDir_0:%v", sSortDir_0)
    }
   }
   
   lenAllData := len(s2wSlice)
   var index int
   if iDisplayStart+iDisplayLength >= lenAllData {
    index = lenAllData
   } else {
    index = iDisplayStart + iDisplayLength
   }
   limitSlice := s2wSlice[iDisplayStart:index]
   fmt.Println(limitSlice)
   counts := len(limitSlice)
   data := make(map[string]interface{}, counts)
   data["sEcho"] = c.Query("sEcho")
   data["iTotalRecords"] = lenAllData
   data["iTotalDisplayRecords"] = lenAllData
   data["aaData"] = limitSlice
   c.JSON(http.StatusOK, data)
  }

 })
}
```
