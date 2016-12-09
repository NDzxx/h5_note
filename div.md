#局部刷新
前端代码

```js
var selectedRoomId = null
    var selectedAuchorId = null    
    $(".c-items li").click(function () {
        delBackgroundColor();
        this.children[0].style.background = "url(static/res/home/AnchorSelect.png) no-repeat";
        selectedRoomId = this.getAttribute("roomId")
        selectedAuchorId = this.getAttribute("auchorId")
/*发送ajax只获取部分图像信息数据*/
        $.ajax({
            type: "get",
            dataType:'json', //接受数据格式
            cache:false,
            url: "/liveroom/menuinfo/" 
                 + this.getAttribute("roomId")
                 + "/" + this.getAttribute("auchorId"),
            beforeSend: function(XMLHttpRequest){
            },
            success: function(data){
                $(".roomInfo")[0].style.display="block"
                $(".snapshot")[0].style.display="block"
                if (data.code == null || data.code != 200){
                    return
                }
                var miniVideo = document.getElementById("miniVideoImg")
                var roomTitle = document.getElementById("roomTitle")
                var onlineCount = document.getElementById("onlineCount")
                var anchorName = document.getElementById("anchorName")

                miniVideo.setAttribute("src",data.RoomImg)
                roomTitle.innerHTML = data.RoomTitle
                onlineCount.innerHTML = data.LookerCount
                anchorName.innerHTML = data.AnchorName
                var enterBtn = document.getElementById("enterBtn")
                if(data.IsOnline){
                    enterBtn.setAttribute("IsOnline","true")
                }else{
                    enterBtn.setAttribute("IsOnline","false")
                }
            },
            error: function(msg){
                //请求出错处理
                console.log(msg)
            }
        });
    })
```
后端代码golang
```golang
func (p *LiveRoomController) MenuInfo() {
	idRoom, err := strconv.Atoi(p.Ctx.Input.Param("0"))
	if err != nil {
		p.Abort("404")
		return
	}

	idMenu, err := strconv.Atoi(p.Ctx.Input.Param("1"))
	if err != nil {
		p.Abort("404")
		return
	}

	roomDataLogic := models.NewRoomDataLogic(int32(idRoom))
	var menu models.TblMenu
       /*获取sql数据然后组装json给前端*/
	if !roomDataLogic.LoadMenu(int32(idMenu), &menu) {
		return
	}

	if menu.RoomId != int32(idRoom) {
		return
	}

	p.Data["json"] = map[string]interface{}{
		"code":       200,
		"AnchorName": menu.AnchorName,
		"RoomTitle":   menu.Desc, //暂时先不改页面那边的名称
		"LookerCount": roomDataLogic.GetWatcherNum(),
		"RoomImg": "static/res/snapshot/" + roomDataLogic.GetSnapshot(idMenu),
		"IsOnline":roomDataLogic.IsOnline(idMenu),
	}
	p.ServeJSON()
}
```




