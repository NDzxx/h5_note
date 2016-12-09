#bootstrap
> * dataTables使用
> * bootstapValidate 验证
> * tooltip和sco扩展


bootstrap扩展：

[sco扩展](/ http://github.com/terebentina/sco.js)
```js
var selectType = $("#idCombox option:selected").text();


 var selectType = $("#idCombox option:selected").text();
		var selInt = 0;
        switch(selectType){
			case "按小时":
			{
				selInt = SELECT_TYPE.SEARCH_HOUR;
			}
			break;
			case "按分钟":
			{
				selInt = SELECT_TYPE.SEARCH_MIN;
			}
			break;
			case "按秒钟":
			{
				selInt = SELECT_TYPE.SEARCH_SEC;
			}
			break;
			default:
			break;
		}
		alert(selInt);





```

  






