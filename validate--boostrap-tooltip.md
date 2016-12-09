参考文章：

[Validate + Boostrap tooltip 表单验证示例](http://www.cnblogs.com/qiufengke/p/4964893.html)
实际使用代码片段
```js

function CheckRegister(){
		$("#idRegform").validate({
	    errorClass: "error",
        success: 'valid',
	    rules: {
	      nickname: {
			required: true,
			minlength: 5,
			maxlength: 32,
			remote:{     
			   type:"POST",
               url:"/nickNameAuth",             
               data:{
                 nickname:function(){return regform.nickname.value;}
               } 
			}
	      },
	      password: {
	        required: true,
			minlength: 5,
			maxlength: 25
	      },
	      password2: {
			 required: true,
			 equalTo:"#idpassword"
	      },
		  protocol:{
			  required:true
		  }
	    },
	    messages: {
	     nickname: {
			required: "注册后昵称不能随意更改",
			minlength: "您的昵称过短",
			maxlength:"您的昵称过长",
			remote:"用户名非法或者被占用"
		  },
		password: {
			required: "请输入密码",
			minlength: "密码长度不正确，仅限5-25个字符",
			maxlength:"密码长度不正确，仅限5-25个字符"
		 },
		 password2:{
			required: "请输入确认密码",
			equalTo: "两次密码输入不一致"
		 },
		  protocol:{
			  required:"您还没有同意注册协议"
		  }
	    },

		unhighlight: function (element, errorClass, validClass) { //验证通过
			$(element).tooltip('destroy').removeClass(errorClass);
		},
		
		errorPlacement: function (error, element) {
                        /*tooltip使用*/
			$(element).tooltip('destroy'); /*必需*/
			$(element).attr('title', $(error).text()).tooltip('show'); 
		},
		submitHandler: function (form) {
			//alert(regform.nickname.value);
			 var hash = hex_md5(regform.password.value);
			 var c2sReg = {
                "nickname":regform.nickname.value,
                "password":hash,
				};
				alert($("#idpassword").val());
			$.post("/reg", c2sReg, function (text, status) { alert(status); });
			
		}
	});
}

```
改进：或可利用sco.tooltip.js改变默认bootstrap的黑色样式
